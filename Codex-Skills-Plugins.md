# Codex with Skills and Plugins

by Internet searching



Installing skills and plugins for Codex is straightforward, and you can do it right inside the chat interface, through the terminal, or via the plugin menu.



## Part 1 - How to Install Skills and Plugins

### General methods

- **Using the Chat Command:** Open a chat in Codex, type `$` to trigger the built-in `$skill-installer`, and supply the name of a curated skill or the GitHub folder URL of an experimental skill.
- **Using Natural Language:** Paste a skill's GitHub URL directly into the chat and instruct Codex to "install this skill". 
- **Via the GUI:** Go to the **Plugins** section in your Codex or ChatGPT desktop app, browse or search for the item, and click **Install** or the **plus** button. 
- **Using the Terminal:** Run `codex` and use `/plugins` to browse, or use a command like `npx skills add [repo] --skill [name]` for external repositories. 
- **Restart:** Restart Codex or refresh your app after installation to make sure new skills load into memory.

### Official OpenAI Skills and Plugins

Codex comes up with default 38 skills at its official repo: https://github.com/openai/skills, Install them? Please prompt:

```txt
Please install the official OpenAI Skills at https://github.com/openai/skills
```

but the Skills repo is deprecated. 

**For current Codex skill and plugin examples**, use the OpenAI Plugins repository: https://github.com/openai/plugins. If you want to add your own skills to Codex, follow the [Build plugins](https://developers.openai.com/codex/plugins/build) guide, which includes instructions for creating a skill-only plugin.

### Installing a skill

Skills in [`.system`](https://github.com/openai/skills/blob/main/skills/.system) are automatically installed in the latest version of Codex.

To install [curated](https://github.com/openai/skills/blob/main/skills/.curated) or [experimental](https://github.com/openai/skills/blob/main/skills/.experimental) skills, you can use the `$skill-installer` inside Codex.

Curated skills can be installed by name (defaults to `skills/.curated`):

```
$skill-installer gh-address-comments
```

For experimental skills, specify the skill folder. For example:

```
$skill-installer install the create-plan skill from the .experimental folder
```

Or provide the GitHub directory URL:

```
$skill-installer install https://github.com/openai/skills/tree/main/skills/.experimental/create-plan
```

After installing a skill, restart Codex to pick up new skills.



## Part 2 - Most Popular Skills and Plugins for Coding

- **find-skills / Find Skills:** An essential search utility skill used to explore and install new community workflows directly from directories like skills.sh. https://github.com/vercel-labs/skills/tree/main/skills/find-skills
- **Grill-Me:** An interactive requirements-gathering skill that interviews you with up to 60 targeted questions to pressure-test project scope and design direction before coding begins. https://skills.sh/mattpocock/skills/grill-me
- **Superpowers:** A comprehensive plugin suite that structures clean agent workflows around planning, execution, review, and verification. https://github.com/obra/superpowers
- **Composio:** Connects Codex instantly to hundreds of real external tools and developer apps without manual wiring. https://www.skills.sh/composiohq/awesome-claude-skills/composio
- **Context7-cli:** Keeps Codex updated with fresh library and framework documentation so the model avoids guessing outdated APIs. https://github.com/upstash/context7/tree/master/skills/context7-cli
- **Impeccable**: Make Interfaces Feel Better. Front-end design and UI polish skills that audit surfaces, animations, typography, and styling in a single pass. https://github.com/pbakaus/impeccable/tree/main/.agents/skills/impeccable

If you'd like, tell me **what kind of project or stack** you are working on, and I can recommend a specific set of skills or plugins tailored to your stack.



## Part 3 - More Skills and Plugins

### 1- ponytail

**Purpose**: Save token and slim codebase

**Here is the repo**: https://github.com/DietrichGebert/ponytail/tree/main/skills/ponytail

**How to install**:

```shell
## Claude Code
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@pinytail

## Codex
codex plugin marketplace add DietrichGebert/ponytail
codex plugin install ponytail@pinytail

## Gemini CLI
gemini extension install https://github.com/DietrichGebert/ponytail

## Cursor
## Copy the repo files in folder .cursor/rules/ to project folder .cursor/rules/

## General method: 
## Copy https://github.com/DietrichGebert/ponytail/AGENTS.md to project root or global config root.
```

**Operations**

```shell
## Switch strength
/ponytail full    # Default
/ponytail ultra   # superlite
/ponytail off     # temporarily close

## Audit current codebase
/ponytail-review  # audit current diff to find over-thinking

## Scan the codebase
/ponytail-audit   # Find all code which can be simplified

## Check out how much saved
/ponytail-gain    # Measure the gain dashboard
```

### 2- ruflo

**Purpose**: 

### 3- ECC-tools



## Part 4 - Popular Skills and Plugins for Codex

#### General Use

- Superpowers
- OpenAI Plugins
- claude-mem: ensure long-time chat
- Agent-Reach: search 17 platforms, https://github.com/Panniantong/Agent-Reach
- GitNexus: Code graph
- Humanizer: Eliminate AI footprints
- skill-creator
- mcp-builder: create external link factory

#### 5 Skills for media automation

- Hyperframes - Write HTML to generate video
- VoiceBox - Clone voice to dub
- Video-use - Auto-remove the rubbish words + generate subtitles
- OpenMontage - One sentence to generate video
- AiToEarn - Auto-publish onto multi-platforms

### 10 Codex Plugins

- chrome
- github
- computer use
- Build web apps
- Figma
- Documents
- Presentations
- Spreadsheets
- Hyperframes
- Remix
- better-harness - https://github.com/QoderAI/better-harness

Install through Codex maketplace or command of `/install plugin chrome`.

### 8 Skills for Documentation (Mainly for Chinese?)

- Lark CLI Skills - Lark Form, sheet, minutes to audit by Agent.
- compdf-skills - Convert PDF, Pic, Forms to Word, Excel
- PPT Master - PPT Generator
- last30days-skill - Search latest 30 days webpages and social status, and create a summary report
- gpt-image2-ppt-skills - For PowerPoint
- Anthropic Skills - Official skills to generate and check DOCX, PPTX, XLSX, and PDF
- pm-skills - Research, PRD, Priority, roadmap, planning, ... to workflow
- wps-skills - operate WPS Word/Excel/PPT

### Skills for FrontEnd

- Figma - design drafting
- Product-Design - Steer the project
- taste-skill master page details, https://github.com/leonxlnx/taste-skill/
- GSAP - complex animation, etc.



## Part 5 - Claude Skills

### General

- find-skulls
- agent-browser
- xlsx
- research
- skill-creator