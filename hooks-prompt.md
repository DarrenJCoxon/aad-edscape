 Create 3 global Claude Code protective hooks in ~/.claude/hooks/ for users running bypassPermissions   
  mode. These hooks are the last line of defence against irreversible commands.                          
                                                                                                         
  Implementation rules for all scripts:                                                                  
  - Read JSON from stdin with INPUT=$(cat)                        
  - Extract fields with jq -r                                                                            
  - Check each pattern with grep -qE (or -qiE for case-insensitive)
  - On match: output {"decision":"block","reason":"<message>"} to stderr and exit 2                      
  - If no match: exit 0 silently                                                                         
  - Add set -euo pipefail and shebang #!/usr/bin/env bash                                                
                                                                                                         
  ---                                                                                                    
  1. block-destructive-git.sh — PreToolUse / Bash matcher                                                
                                                                                                         
  Block these git patterns (use grep -qE per pattern in a loop with paired reason strings):              
                                                                                                         
  ┌──────────────────────────────────────┬──────────────────────────────────────────────────────┐
  │               Pattern                │                        Reason                        │        
  ├──────────────────────────────────────┼──────────────────────────────────────────────────────┤        
  │ git\s+push\b.*(\s--force|\s-f)(\s|$) │ Use --force-with-lease instead, or confirm with user │
  ├──────────────────────────────────────┼──────────────────────────────────────────────────────┤        
  │ git\s+reset\s+--hard                 │ Permanently discards uncommitted changes             │        
  ├──────────────────────────────────────┼──────────────────────────────────────────────────────┤        
  │ git\s+clean\s+.*-[a-zA-Z]*f          │ Permanently deletes untracked files                  │        
  ├──────────────────────────────────────┼──────────────────────────────────────────────────────┤        
  │ git\s+checkout\s+--\s+\.             │ Permanently discards all working-tree changes        │
  ├──────────────────────────────────────┼──────────────────────────────────────────────────────┤        
  │ git\s+branch\s+-D\b                  │ Irreversible branch deletion                         │
  ├──────────────────────────────────────┼──────────────────────────────────────────────────────┤        
  │ git\s+stash\s+drop                   │ Permanently discards stashed changes                 │
  └──────────────────────────────────────┴──────────────────────────────────────────────────────┘        
                                                                  
  Extract command with: jq -r '.tool_input.command // empty'                                             
                                                                  
  ---                                                                                                    
  2. block-dangerous-bash.sh — PreToolUse / Bash matcher          
                                                        
  Use grep -qiE (case-insensitive). Block these patterns:
                                                                                                         
  Pattern: rm\s+-[a-zA-Z]*r[a-zA-Z]*f|rm\s+-[a-zA-Z]*f[a-zA-Z]*r                                         
  Reason: Recursive forced deletion                                                                      
  Notes: Allow if target matches safe dirs:                                                              
    node_modules|dist|\.next|\.turbo|build|coverage|\.cache|tmp|__pycache__
  ────────────────────────────────────────
  Pattern: rm\s+-f\s+/                                                                                   
  Reason: Forced deletion of absolute root path
  Notes: Always block                                                                                    
  ────────────────────────────────────────                        
  Pattern: DROP\s+TABLE                                                                                  
  Reason: Destructive DB command
  Notes: Always block                                                                                    
  ────────────────────────────────────────                        
  Pattern: DROP\s+DATABASE                                                                               
  Reason: Destructive DB command
  Notes: Always block                                                                                    
  ────────────────────────────────────────                        
  Pattern: \bTRUNCATE\b                                                                                  
  Reason: Destructive DB command
  Notes: Always block                                                                                    
  ────────────────────────────────────────                        
  Pattern: kill\s+-9\b                                                                                   
  Reason: Force-kills processes, can corrupt state
  Notes: Always block                                                                                    
  ────────────────────────────────────────                        
  Pattern: \bkillall\b                                                                                   
  Reason: Mass process termination
  Notes: Always block                                                                                    
  ────────────────────────────────────────                        
  Pattern: chmod\s+(-R|--recursive).*777|chmod\s+777.*(-R|--recursive)                                   
  Reason: World-writable recursively
  Notes: Recursive only                                                                                  
  ────────────────────────────────────────                        
  Pattern: \bmkfs\b                                                                                      
  Reason: Formats filesystem, destroys all data
  Notes: Always block                                                                                    
  ────────────────────────────────────────                        
  Pattern: dd\s+if=/dev/zero                                                                             
  Reason: Zeros a device, destroys all data
  Notes: Always block                                                                                    
                                                                  
  For the rm -rf check: first test if the command matches the safe-dir allowlist — if yes, skip to next  
  pattern. Only block if it does NOT match the allowlist.
                                                                                                         
  ---                                                             
  3. block-secrets.sh — PreToolUse / Edit|Write matcher
                                                                                                         
  Extract:
  - FILE=$(echo "$INPUT" | jq -r '.tool_input.file_path // empty')                                       
  - CONTENT=$(echo "$INPUT" | jq -r '.tool_input.new_string // .tool_input.content // empty')
                                                                                                         
  Block if FILE matches .env files:                                                                      
  - Pattern: (^|/)\.env(\.|$) — covers .env, .env.local, .env.production, etc.
  - Reason: "Writing to .env files is blocked. Use vercel env or your secrets manager."                  
                                                                                       
  Block if CONTENT matches (case-insensitive):                                                           
                                                                                                         
  ┌────────────────────────────────────────────┬──────────────────────────────────┐                      
  │                  Pattern                   │              Reason              │                      
  ├────────────────────────────────────────────┼──────────────────────────────────┤
  │ BEGIN (RSA |EC |DSA |OPENSSH )?PRIVATE KEY │ Private key in source file       │
  ├────────────────────────────────────────────┼──────────────────────────────────┤
  │ aws_secret_access_key                      │ AWS secret in source file        │                      
  ├────────────────────────────────────────────┼──────────────────────────────────┤                      
  │ AKIA[0-9A-Z]{16}                           │ AWS Access Key ID in source file │                      
  ├────────────────────────────────────────────┼──────────────────────────────────┤                      
  │ ghp_[A-Za-z0-9]{36}                        │ GitHub Personal Access Token     │
  ├────────────────────────────────────────────┼──────────────────────────────────┤                      
  │ sk-[A-Za-z0-9]{48}                         │ OpenAI API key                   │
  └────────────────────────────────────────────┴──────────────────────────────────┘                      
                                                                  
  ---
  settings.json update:
                                                                                                         
  Read the existing ~/.claude/settings.json first. Merge — do not overwrite — the following into the
  hooks.PreToolUse array and a new hooks.PreToolUse entry for Edit|Write:                                
                                                                  
  {                                                                                                      
    "matcher": "Bash",                                            
    "hooks": [
      { "type": "command", "command": "/Users/$USER/.claude/hooks/block-destructive-git.sh", "timeout":
  5, "statusMessage": "Checking for destructive git commands..." },                                      
      { "type": "command", "command": "/Users/$USER/.claude/hooks/block-dangerous-bash.sh", "timeout": 5,
   "statusMessage": "Checking for dangerous bash commands..." }                                          
    ]                                                             
  },                                                                                                     
  {                                                                                                      
    "matcher": "Edit|Write",
    "hooks": [                                                                                           
      { "type": "command", "command": "/Users/$USER/.claude/hooks/block-secrets.sh", "timeout": 5,
  "statusMessage": "Checking for secrets in file content..." }                                           
    ]                                                             
  }                                                                                                      
                                                                  
  Use the actual resolved home path (not ~ or $USER) in the command strings.                             
   
  After writing all files, run chmod +x on all 3 scripts, then run bash -n on each to confirm no syntax  
  errors.     
