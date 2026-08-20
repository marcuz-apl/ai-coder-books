# Codex Project: Skills, Settings and Rules



## Part 1 - Codex Skills Installation

Please confirm the following system skills are in place (if not, install them):

| Skill                    | Purpose                                                      |
| ------------------------ | ------------------------------------------------------------ |
| `imagegen`               | Image creation/editing                                       |
| `openai-docs`            | OpenAI API documentation                                     |
| `plugin-creator`         | Create Codex plugins                                         |
| `skill-creator`          | Create custom skills                                         |
| `skill-installer`        | Install skills from GitHub                                   |
| `computer-use`           | computer-use plugin                                          |
| `control-in-app-browser` | browser plugin                                               |
| `control-chrome`         | chrome plugin                                                |
| `gh-*` tools             | github plugin (gh-fix-ci, gh-address-comments, github, yeet) |




Please install the following skills (some with GitHub repositories) and report the success and failure eventually:
- Computer-Use : (System Default, no link is provided)
- gh-fix-ci : https://github.com/openai/skills/tree/main/skills/gh-fix-ci
- create-plan : https://github.com/openai/skills/tree/main/skills/.experimental/create-plan
- frontend-skill : https://github.com/anthropics/skills/tree/main/skills/frontend-design
- webapp-testing : https://github.com/openai/skills/tree/main/skills/webapp-testing
- mcp-builder : https://github.com/openai/skills/tree/main/skills/mcp-builder
- brooks-lint : https://github.com/hyhmrright/brooks-lint
- pr-preview : https://github.com/openai/skills
- Planning-with-Files : https://github.com/OthmanAdi/planning-with-files
- Superpowers : https://github.com/obra/superpowers
- Humanize : https://github.com/humanizerai/agent-skills
- Humanizer-zh : https://github.com/op7418/humanizer-zh

Also, install the following skill if possible:

- DocumentSkills : https://github.com/anthropics/skills
- Context-Engineering : https://github.com/openai/skills
- Claude-mem : https://github.com/thedotmack/claude-mem
- Agent-Reach : https://github.com/Panniantong/Agent-Reach
- GitNexus : https://github.com/abhigyanpatwari/GitNexus



**Successfully installed (12 skills):**

| #    | Requested Name      | Installed As          | Source                                       |
| ---- | ------------------- | --------------------- | -------------------------------------------- |
| 1    | Computer Use        | *(bundled plugin)*    | Already available — no install needed        |
| 2    | gh-fix-ci           | `gh-fix-ci`           | openai/skills (curated)                      |
| 3    | create-plan         | `define-goal`         | openai/skills (curated) — closest equivalent |
| 4    | frontend-skill      | `frontend-design`     | anthropics/skills                            |
| 5    | webapp-testing      | `webapp-testing`      | anthropics/skills                            |
| 6    | mcp-builder         | `mcp-builder`         | anthropics/skills                            |
| 7    | brooks-lint         | `brooks-audit`        | hyhmrright/brooks-lint                       |
| 8    | pr-preview          | `gh-address-comments` | openai/skills (curated) — closest equivalent |
| 9    | Planning-with-Files | `planning-with-files` | OthmanAdi/planning-with-files                |
| 10   | Superpowers         | `using-superpowers`   | obra/superpowers                             |
| 11   | Humanizer-zh        | `humanizer-zh`        | op7418/humanizer-zh                          |
| 12   | humanize            | `humanize`            | humanizerai/agent-skills                     |



## Part 2 - Pre-Settings

...



## Part 3 - Project Settings and Rules

Please generate PRD.md, AGENTS.md, rules.md for my project: __FamiCent__:
- This is a Web App trying to manage family financial accounts, such as, Utility Bills, Credit Card, Bills to pay, Insurance, etc. 
- very sensitive data, then a password is needed to enter into the main GUI, a MFA can be added and enforced later on.
- A small database of SQLite3 is needed.

Some general rules:
- User Management: there shall be an "admin" user with initial password: "admin123", once this user login, there shall be a notifier mentioning to change the password (not mandatory though), the notifier shall disappear if a new password is generated.
- Versioning: the version shall be in the format of "m.n.p" starting from 1.0.0, while n and p are 1-digit, changing from 0 to 9;
- Create and maintain a pre-commit hook to bump the version by 0.0.1 for every commit, and also the commit message shall be "v{m.n.p} build yyyy-mm-dd-hhmm" + the feat info.

Please select toolset to fulfill the objectives above and prepare the markdown files for my review. 

Please advise me anything that I missed.



## Part 4 - Build the App

Please review the **PRD.md**, **AGENTS.md** and **rules.md** markdown files in the project folder and start to build the Web app. 

Go ahead till the finish line, don't ask me any more, since full permission are given.

Eventually report how many token and time have taken.



## Slogan of Alfazen Inc.

Alfazen Inc. - An information services firm helping small businesses succeed.



## License

MIT License