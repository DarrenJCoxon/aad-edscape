Create 2 global Claude Code pre-commit hooks in ~/.claude/hooks/ that run before every git commit.     
                                                                  
  All scripts must:                                                                                      
  - set -euo pipefail, read stdin with INPUT=$(cat), extract fields with jq -r
  - Only act when tool_name is Bash and command matches git\s+commit\b                                   
  - On block: print {"decision":"block","reason":"<message>"} to stderr and exit 2
  - Warnings: print to stderr only, continue (exit 0)                                                    
  - Use resolved absolute paths everywhere (not ~)                                                       
                                                                                                         
  ---                                                                                                    
  1. pre-commit-checks.sh — PreToolUse / Bash matcher                                                    
                                                                                                         
  Find the project root by walking up from pwd until package.json is found. If none found, exit 0.
                                                                                                         
  Detect package manager: pnpm-lock.yaml → pnpm, yarn.lock → yarn, bun.lockb → bun, else npm.            
                                                                                                         
  Run each check only if the corresponding script exists in package.json (jq -e '.scripts.<name>'). Block
   on failure:                                                    
                                                                                                         
  - build — "Pre-commit check failed: build errors. Fix before committing."                              
  - lint — "Pre-commit check failed: lint errors. Fix before committing."
  - typecheck or type-check (try both) — "Pre-commit check failed: TypeScript errors. Fix before         
  committing." If neither exists but tsc is in PATH, run tsc --noEmit.                                   
  - test — "Pre-commit check failed: tests failing. All tests must pass before committing."              
                                                                                                         
  Suppress stdout from all checks (>/dev/null 2>&1), only surface the block reason.                      
                                                                                                         
  ---                                                                                                    
  2. check-staged-files.sh — PreToolUse / Bash matcher            
                                                      
  Run these checks against staged content only. Get the diff with DIFF=$(git diff --cached) and staged
  filenames with FILES=$(git diff --cached --name-only).                                                 
   
  Conflict markers (block) — if $DIFF contains <<<<<<<, =======, or >>>>>>>: "Merge conflict markers     
  found in staged files. Resolve all conflicts before committing."
                                                                                                         
  Secrets in staged diff (block, case-insensitive grep) — check $DIFF for:                               
  - BEGIN (RSA |EC |DSA |OPENSSH )?PRIVATE KEY
  - aws_secret_access_key                                                                                
  - AKIA[0-9A-Z]{16}                                              
  - ghp_[A-Za-z0-9]{36}                                                                                  
  - sk-[A-Za-z0-9]{48}                                                                                   
                      
  Block with: "Secret detected in staged changes. Remove credentials before committing."                 
                                                                                                         
  Debug artifacts (warn only) — check staged .js .ts .tsx .jsx files in $FILES for console\.log\(,       
  debugger;, TODO: (case-insensitive). Print matching lines to stderr, continue.                         
                                                                                                         
  Large files (warn only) — for each file in $FILES, check size with wc -c. If any exceeds 1MB, warn with
   filename and size to stderr, continue.
                                                                                                         
  ---                                                             
  After creating scripts: chmod +x both, run bash -n on each.
                                                                                                         
  Update ~/.claude/settings.json: read the file first, then merge a new PreToolUse entry with matcher: 
  "Bash" containing both scripts. Timeout 60s for pre-commit-checks.sh (build can be slow), 30s for      
  check-staged-files.sh. Use resolved absolute home path in command strings. Do not overwrite existing
  hooks.  
