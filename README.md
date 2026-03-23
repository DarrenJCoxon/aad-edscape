## Introduction

---

### How does Claude Code differ from AI chatbots like ChatGPT and Gemini?

The main difference between Claude code and the chatbots that you will have more likely used is that not only will Claude write an output but it will also organise. What this means is that you can actually get it to carry out actions on your computer. When we're coding this means that it will:

- create folders
- create files inside of those folders
- link files together
- run commands (we call these bash commands)

These are commands that you would normally type yourself into the terminal but Claude can do most of them on your behalf.

This takes a bit of getting used to. When we are first starting, we might feel that we are potentially going to be doing some quite dangerous things inside of our computer. That somehow AI might take things over and break things. There are some dangers and I'm going to talk about those dangers as we go through the day.

One of the first things that we're going to do is we're actually going to set up the coding environment even before we begin to code. We need to make sure that the environment is properly set up. This should give you the assurance that what we're going to be doing is safe and secure. It means that you can be a lot more experimental and really push things further, knowing that Claude can't really do anything too bad.

**IMPORTANT**: A lot of what we will do this morning will not be coding - we are going to ensure we have the optimal environment to get the most out of Claude Code.

## 1. Setting up the environment

---

1. My setup with VS Code - why VS Code and not Cursor? Just preference - can use either. Quick familiarisation with the interface. File browser, preview, Terminal, Status bar at the bottom via View.
2. Other preferences - Light and dark mode, turning off windows etc. 
3. How to open a new session using File - New Window - Create a test folder on the desktop, drag it into VS Code. Explain why this is so important at the start - MUST be using Claude Code inside a project folder. 
4. Show the other way to open a folder inside Terminal using cd and dragging in the folder. 

### Claude Code basics

1. How to open Claude Code inside VS Code:
    1. Inside the Terminal - Claude
    2. By adding the Claude extension (my preference for today)
2. How to log into Claude - through the CLI / through the browser
3. The various features of Claude Code, looking in the slash command and going through the most important features first. 
    1. Different default permission levels and what this means
    2. Plan mode (more on that later)
    3. Slash commands - basic overview (will revisit as the day goes on)

### Giving Claude more freedom

1. Setting up Claude Extension and enabling skip permissions/Yolo mode. What this is and why it is vital for our approach. And the risks!
2. What the different modes do - permissions on and off, planning mode.

### …And adding guardrails

- Putting in protections from the start - add the 2 hooks prompts in github into Claude in Terminal - DO NOT WORRY if you cannot understand anything in this prompt - this is talking to Claude, not to us!
- Why set this up from the start? Using skip permissions can result in Claude running destructive commands which can cause problems. This means we run it inside guardrails. This hook will also stop you from committing secrets in your codebase - critical!
- The second hook ensures that all tests are done and passing before you commit. This is critical to avoid storing up technical debt, which is a key risk with AI coding.
- Just copy from GH and paste into claude in Terminal in the root - open this up via VS Code.
- So -now we have maximum autonomy with maximum protection.
- Let’s build something simple together.

### Permission Levels - plan versus no plan

To see how different the output is when you even do a simple plan, here are two pages we will design. Follow along using the instructions in github. Feel free to write in your own ideas for a homepage.

Begin by setting up a folder, dragging it into VS Code, and ensuring Claude Code is enabled. 

### TRY: build a simple homepage without plan:

```
Create a file called index.html with a page that says Good Morning and shows today’s date.
Make the page fun with some interactive features.

```

Now with plan -note that you can choose Plan, or just know that using the word plan will automatically switch Claude to plan mode. 

```
I want to plan a homepage - index.html that says Good Morning and shows today’s date.
I want to make the page fun with some interactive features. Return your plan for the optimal way to achieve this.

```

## 2. Building a simple next.js app

---

We have our coding environment set up. Let’s focus on the language we will be building in. 

### 2.1 The language we will build in

Introduce next.js as an evolution of HTML - allows for greater interactivity, faster and lighter - much of the work is done client side, which makes the load on the server side lighter.

For us it is no different - we just tell Claude that’s what we want and it sets up the project. 

Let’s create a new project folder - Magic Teacher. Drag into a new VS Code window and enable Claude. 

```
Create a Next.js app with a landing page, nav bar, and card layout. This app will be called Magic Teacher.
Focus the landing page on [how teachers can save time with AI.
Bring in some humorous ideas - have fun with this!]
Make the landing page look like something build by Microsoft Frontpage in the 1990s.
```

1. See what Claude built - an entire scaffold - this is one of the real benefits of using Next.js
2. Look at the basic files - just high level explanation. 
3. Now, ask Claude to run build - this checks the codebase for errors. 
4. Now, ask it to run the dev server so you can see your creation on localhost:3000. You can also enable the preview in browser extension to do this automatically. 
5. Here’s mine - https://edskills-test1.vercel.app/
6. Have a play - choose a theme for your landing page and a style. Then we can share and compare. 

**Important note:**

Coding with AI does involve some waiting time. This is because Claude will take time to code everything you ask. Be prepared for this today and in general. It is not instant! 

### 2.2 Committing your code to Github and deploying on Vercel

Now we have a beautiful next.js site, let’s get it committed and deployed.

1. Ensure connected to Github CLI - if not follow instructions in Github 
2. Enable Github plugin in Claude - /plugin then toggle on 
3. Either - manually create the Github repo, or ask Claude to do it - make it either public or private and give it a name. 
4. Ask Claude to commit the page to git. If manual setup, give Claude the Git URL. Check the Repo to ensure committed. Specify the Main branch - this is the default, front-facing version of your website. It is important to have Main set up as we will be branching from this, or creating versions:
```
Now, commit this codebase to Github to the Main branch. 
Create the repo with the name [project-name]. Ensure the repo is Private. 
```
1. Head to Vercel. Ensure signed up using Git credentials. Choose New - Project
2. Connect Vercel to Github, add the repo, deploy
3. Now you have a beautiful homepage!

Recap what we have done thus far:

- Got Claude Code running inside the CLI
- Added autonomy and accountability to the process
- Built our first website and published it

### 2.3 Using Git Branches and creating PRs

Branches are critical for version control. Without them, you have no way to rewind once you have pushed the codebase to Github. 

The Main branch is the one that faces the world, but you can create named branches for interim work. Two of the most popular are feature and fix. 

You can simply ask Claude to create a new branch. I will usually say ‘create a feature branch for this next story’ (if I am using agile - more on that later), or ‘create a fix branch to work on this fix’. Work on a new feature (or a fix) then push the branch to git. You can preview the branch before you merge to main. 

There are two ways to do this. Either create the pull request (PR) manually, or ask Claude to create one. I prefer the latter. Then, once you have tested the preview branch, you can merge the changes into the main branch. 

This is a critical workflow and one you need to lock down from the start. Claude (and you) will make mistakes. You need a rewind button. 

Before you begin the next feature, ask Claude to set up a new git branch:
```
Now, start a new feature branch. Call it supabase-and-chatbot.
```
### 2.4 Adding a second page with authentication

Now we add simple auth. There are lots of options, but I prefer using Next.js native auth. It’s simple to set up and Claude knows it well. Notice how at this stage I am simply describing what I want, and where. Not too specific, allowing Claude to make some of those choices. Later we will lock this down more. 
```
We will now add a second page to the site, which is a private members area for VIP teachers only.
This will be hidden behind user authentication. We will use next auth for this.
At this stage, only email and password. Ensure you add an auth secret to env.local.
Users must sign up with an email and password before they can authenticate.
Create the VIP page and a login/register button top right. 
```
### 2.5 Adding environment variables to Vercel

Now, before we republish the page, we need to add the next auth env variable to Vercel for our authentication. Head to settings - environment variables, copy them from env.local and paste them into Vercel.

This MUST happen every time you add a new environment variable. It is a good idea to do this before pushing new commits to avoid build failures. 

Ask Claude to restart the dev server and check authentication. O

### 2.6 Adding a capture details form with backend in Supabase

Now we add our first backend service - Supabase. This will be to capture client details. 
```
We will now add a guestbook to our site, so that users can see other comments on the site.
We will also enable persistent user authentication. We will do this using a Supabase backend.
Please set this up, including adding env.example with instructions for what variables you need from supabase.
We will be calling the supabase project teacher-vip-97.
```
### 2.7 Adding some AI - a helper chatbot

Let’s now add a chatbot with a system prompt. We can either decide where we want this, or allow Claude to work out the optimal place on the page. System prompts specify the behaviour the bot will always show. In this case a harassed teacher:
```
Now, inside the auth VIP page we want a chatbot.
Consider the optimal place for this chatbot on the page and the optimal width for ease of use.
The chatbot needs to behave like a harrassed teacher.
Whatever we ask it needs to respond in the manner of a teacher who is trying to remain calm but whose patience is strained.
We will be using Openrouter for this chatbot: we want to specify the API and model in environment variables.
Ensure React Markdown is installed.
```
You can choose which AI model to use inside Openrouter. Test models that are free before committing. However, free models tend to glitch so it’s better just to add maybe £5 to credits. It’ll last you a long time if you’re just testing the cheaper models. 

Before you commit and push the changes, ensure you add Supabase and Openrouter environment variables to Vercel, or the backend guestbook and chatbot won’t work.
You can copy them as a block from env.local and paste them all at the same time into Vercel. You can also use the Vercel CLI, but I prefer to do this bit manually. 

### 2.8 Debugging tools

There are a couple of very useful debugging tools provided as Claude defaults. Ensure you enable them both:

1. **Playwright** opens a browser which Claude can ‘see’, as well as being able to access console logs. This is invaluable for debugging user interfaces (UI).
2. **Context7** is an excellent tool as it enables Claude to consult the latest ‘user manuals’ for whatever you’re working on. This is vital as Claude’s knowledge cut off is January 2025 so it often messes up when trying to fix problems based on only its knowledge.

Having Vercel and Supabase CLIs set up is also massively helpful as Claude can look into them both direct to check for errors. Ensure they are enabled in Claude plugins, then use supabase login and vercel login in terminal. This opens a webpage auth window. Follow instructions. You only need to do this once. 

## 3. Preparing for the main build

---

### 3.1 The importance of agents

Before we launch into the longer coding session (where we build our own project), we need to set up a few additional features. We will be using a team of agents - one for planning, one for coding, and one for checking. My recommendation is that you use Opus for planning, but Sonnet for coding and checking. This will save you a lot of tokens as Sonnet works well as a coder when following instructions. 

I call my three agents scrum master, story implementer and qa validator, but you can choose different names - such as planner, coder, and reviewer - if you wish. I guess you could give them human names too if it’s easier!

1. The scrum master takes the story summaries and turns them into full story files using the template in the Github repo
2. The story implementer codes the stories following strict coding rules
3. The QA validator checks the story implementer did the right job

Choose agents in the / menu and follow the instructions. You want all three to have all tools available to them. I recommend choosing personal over project so that you have access to agents in all your builds. 

### 3.2 Claude’s agent teams

You can spin up an agent team at any time. To enable, paste this into Claude:
```
Read this webpage and enable agent teams globally in Claude Code: https://code.claude.com/docs/en/agent-teams.
Add the **CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS** setting to settings.json but do not overwrite the file. 
```

Agent teams are great for the following: 

- Research and review: multiple teammates can investigate different aspects of a problem simultaneously, then share and challenge each other’s findings
- New modules or features: teammates can each own a separate piece without stepping on each other
- Debugging with competing hypotheses: teammates test different theories in parallel and converge on the answer faster
- Cross-layer coordination: changes that span frontend, backend, and tests, each owned by a different teammate

Be aware that they use a lot of tokens, so if you’re just on the Pro mode you might find your week’s quota is used up in one run. Better for Max plan holders. 

### 3.3 The BMAD planning cycle

I want to show you the framework I’ve used over the last year. I have fully automated this, but I feel it important you run it manually a few times first so you understand the process. Too much automation robs you of understanding. 

We will use Google AI Studio OR Google Gemini for this. I don’t think it matters too much. I generally find AI Studio gives me a better output but try both. Both are free.

WHY NOT USE CLAUDE? Simple, really[.](http://really.You) You will use up a lot of tokens with this process as the BMAD prompt is long. Save your tokens for coding, as Gemini does a good job. 

EITHER create a Gemini Gem, or just add the BMAD prompt to the system instructions in AI Studio. You can find it in the Github repo. Then, follow the instructions! 

I like to use Wispr flow for this, as it’s nice to chat through ideas rather than type them out. I can be a bit more expressive and can explain in more depth. Try it free - https://wisprflow.ai/

By the time you reach the end, you will have the following:

- Project brief
- Product requirements document (PRD)
- Architecture and UI/UX documents
- Epics and story summaries.

This is the most important part of any build. Without it, you will end up tied in knots. 

#### What tech stack do I recommend?

For this project today, I would suggest the following:

- Next.js (as we have above)
- Typescript
- Supabase for the database
- Drizzle for your ORM (object relationship manager - think of this like a table organiser that helps your codebase talk to the database - it is a massive unlock).
- Next Auth for authentication
- Vitest as your testing framework (builds and runs tests of all your major routes to ensure nothing is broken)
- Openrouter if you want to bring in AI (entirely up to you). If you MUST have EU AI, you can choose Scaleway or look at Alibaba’s new EU inference provision. Google Vertex is another option but is complex for beginners. Openrouter is perfect if you’re just playing.
- Tailwind and shadcn/ui for the user interface (this is a neat component library that has things like cards, buttons and menus prebuilt that you can ask Claude to modify. Saves a lot of time and makes things nice and consistent).

You can specify this from the very start and Gemini will factor this in, pushing back if what you suggest isn’t what it would recommend. You genuinely don’t need to understand any of this to a particular degree, as AI will lead the technical side. This is just a tech stack I’ve used a lot that I know works. 

**IMPORTANT POINT**
DO. NOT. WORRY. Things WILL go wrong. A lot. 
At any time you can ask Claude to guide you step by step. Just say something like “I am not sure what to do next. Take me step by step, assuming no prior knowledge.” I did this a lot in the early days. And learned a lot. 
When you’re building today, do this first if you get stuck or if errors appear. It’s a vital skill to learn.

### 3.4 Turning your planning docs into dev docs

Now, we add these documents into a VSCode folder. This can be a new folder for your project. Don’t worry if you’re still not sure what your project name should be - you can give it a working title for now. 

#### Using bash commands to set up your folder structure

You can set up folders and empty files using the normal icons in VSCode, or you can use bash commands inside the terminal. This is quite nice to learn as it makes you feel like a dev!

- mkdir [folder name] - make a new directory (folder)
- cd [folder name] - change directory (folder)
- touch [file name] - make a new file
- cd . - move back up one folder
- cd .. - move back up two folders

So to create a docs folder, add a core folder inside it and a blank prd file, you would type

1. mkdir docs
2. cd docs
3. mkdir core
4. cd core
5. touch prd.md

You can add several empty files in one command - just list them one after the other without commas in the command.

e.g. touch [file-1.md](http://file-1.md) [file-2.md](http://file-2.md) file-3.md

You must always have a hyphen or underscore between words or they will be added as separate files or folders.

**AI can do all this for you - but I think it’s good to learn manually first - and can often be quicker (and uses no precious tokens).**

#### Adding your core docs

Add your PRD, architecture, UI/UX, Epics and story template files into the core folder.  You can find the story template in the Github repo.

From here, we will be decomposing our epics file into story files. This is like the AI reading the chapter summary and turning it into completed sections. Add a prompt like this:
```
I want you to read the docs in /core. Use the scrum master agent to read the epics file, story-template and associated files and decompose Epic 1 into individual story files. Name these story files E1.1, E1.2 etc. Add these stories to docs/stories/todo.
```

Providing you have created the three agents above, this prompt will invoke the scrum master agent to create the first set of stories. Notice how I have asked it to create new folders. Having /todo and /completed inside /stories is very helpful, as it ensures you know where you are in the build. 

### 3.5 Creating our [CLAUDE.md](http://CLAUDE.md) build rules

One of the most important core documents is [CLAUDE.md](http://CLAUDE.md) / [AGENTS.md](http://AGENTS.md) (either is fine). This will give your build the persistent rules it needs through the build. We’ve actually already defined most, but code-related ones it’s worth adding. Add the CLAUDE.md prompt from Github. This will add your CLAUDE.md file into root. 

Notice the command to keep it brief. You don’t want or need a bloated [CLAUDE.md](http://CLAUDE.md) file - this wastes valuable AI tokens. Just the bare minimum rules that Claude can’t pick up through looking at the codebase itself.

OK, that’s it! We are ready to build. It’s a lot of setting up, but once you have all in place you’ll find the building part is much smoother.

## 4. The build

---

### 4.1 Build protocol

The moment of truth. And honestly, once you have the plan, you follow a pretty predictable workflow:

1. Begin a new git branch: “Start a new git branch for story [number].”
2. Call up the story implementer to complete the named story: “Now, use the story implementer agent to complete story [number].”
3. Then, ask the QA validator to validate: “Now, ask the QA Validator agent to assess the story code quality.”
4. If it’s part of the project you can check, start [localhost:3000](http://localhost:3000) and check the code in dev
5. Once checked and validated, push to the branch and create a PR: “Now, commit and push to the branch and create a PR.”
6. Merge the PR to main.

### 4.2 Automating the whole thing with a Skill

You can literally build an automation for the whole process above (or any stepped process). Automations are called Skills. Drop this into Claude:
```
I want to build a Claude Skill for the below workflow. Add it to my .claude folder inside /skills so that all projects have access to it. Name it /story-to-code.

1. Call up the story implementer agent to read the named story and write the code.
2. Once complete, invoke the qa validator to check the story code quality and push back to the story implementer if there are errors.
3. When passed, begin the [localhost](http://localhost) server on localhost:3000. If this port is in use, kill the current server and restart. Do not start on another port.
```

Restart Claude Code to access it. Then, add this prompt into Claude:
```
/story-to-code story E1.1
```

Or whatever you’ve called your story file.

### 4.3 Debugging

This is a hard one to explain, as every debugging session will be different. The key (always) is this: THE MORE SPECIFIC INFORMATION YOU CAN PASS TO CLAUDE, THE BETTER.

These are your friends:

- Playwright so Claude can see the user interface and read Console logs
- The dev server so Claude can read the dev server logs
- Any specific console error messages (to access, View - Developer - Developer Tools - Console)
- Context7 so Claude can check the latest user manuals
- Vercel CLI (once the app is in production, so Claude can read the production logs)

When you pass this information to Claude, explain the problem first. For example:

“When I try to login, I receive this message: [add console log message]. Use Playwright to try logging in and check Console log and dev server log messages.”

#### DO NOT allow Claude to come up with quick fixes, workarounds, or add masses of debug logging…

This is a biggie, and to be honest only really comes with time. If Claude cannot work out the reason for the bug, it will usually default to the lazy route. If it says any of the below, stop it and tell it to slow down, think hard, and look full stack.

- “Let me add some debug logging so we can trace the error.”
- “As we have time constraints, let me [add some lame excuse for a workaround.” (Do we Claude? Who said?)
- “Let me create a fix for now, and we can work on the root cause later.” (No Claude - let’s sort the problem now.)

If it still cannot work out the reason, then some temporary logging can help. But be careful - once your codebase is littered with debug logs you have stored technical debt and introduced vulnerabilities. 

To be fair, Claude Opus 4.6 does this MUCH less, but if you go down a cul de sac it will do its best to hack its way out. Be warned….

#### Learn the hard refresh

Your web browser will cache (store) old code. To remove the old code, it’s CMD+Shift+R on a Mac, and CTRL+Shift+R on a PC. Do this every time you make changes.

#### If that doesn’t work, remove all cache and cookies

Sometimes the old code will cache so hard you need to reset the entire thing. To do this - View - Developer Tools - Application - Storage - Clear site data. Tick include third party cookies.
