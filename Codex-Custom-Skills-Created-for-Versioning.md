# Creating Custom Skills with Codex

Marcuz-apl |  2026-08-11



## Intro

It turns out a skill shall be created for my versioning needs. then I turn to Codex.

```text
Hello GPT,
Please help create a versioning rule as stated below:
- Version format is like "m.n.p", while both n and p are 1-digit, incrementing from 0 to 9 only, that means the new version after 1.0.9 is 1.1.0.
- First version is 1.0.0
- please setup a pre-commit hook ensuring the version bump 0.0.1 per each commit and the git commit info includes the version + build in the format of "m.n.p build yyyy-mm-dd-hhmm", prefixing the feat message.

Please advise if the above rules are feasible or not. If yes, please create a skill of these rules since I will be using this for every projects. The Skills name is "alfazen-versioning".
```



## How to create the skill of `alfazen-versioning`?

Here are the feedback from GPT. Sounds great so far.

```text
Yes, the rules are feasible. I’ll formalize them as a reusable alfazen-versioning skill, first checking the skill-authoring requirements and the target global skills location. I’ll also call out one important design choice: the hook must avoid recursively creating a second commit when it updates version metadata.
```

```text
Using skill-creator, writing-skills, and using-superpowers to author and validate the reusable skill. The intended destination is the global Codex skills directory so it can be discovered across projects.
```

```text
The rules are feasible, but the requested “pre-commit hook” needs a small correction for reliability: version bumping belongs in pre-commit, while commit-message formatting belongs in prepare-commit-msg or commit-msg. I’m testing the likely failure modes before writing the skill so it explicitly handles recursion, skipped hooks, and carry-over from m.n.9.
```
Quite a few agents are created, running against the tasks ...

```text
The baseline review confirms the main traps: a single pre-commit hook cannot safely do both file mutation and message rewriting; hooks can be bypassed; timestamps need a timezone policy; and history-rewriting commands need idempotency rules. I’m now creating the global skill with a canonical VERSION file, a two-hook design, and explicit edge-case handling.
```

```text
The RED pass is complete. The skill will encode these observed failure modes directly: separate hook responsibilities, UTC stamps, idempotent message rewriting, carry rules through 1.9.9 → 2.0.0, and explicit limits of client-side hooks. I’m writing the minimal reusable guidance now.
```

Agents are running well...

```text
The GREEN pass was mostly successful, but one validation agent proposed a post-commit bump, which would not satisfy the requirement that the commit itself contain the bumped version. I’m tightening the skill to explicitly reject that workaround and then I’ll validate the final artifact and deploy it globally.
```

Closed some agents ...

```text
The final forward-test now rejects the post-commit workaround and consistently selects pre-commit plus prepare-commit-msg. The workspace copy contains only the intended skill files; I’m performing structural validation and copying that verified skill into the global Codex skills directory.
```

```text
The global skill is installed. A cleanup check found unrelated .githooks/ and VERSION files in this otherwise documentation-only workspace, apparently left by the validation agents. I’m inspecting those exact files before removing them so the requested skill does not alter this project root.
```



Eventually, Codex presents me with:

```markdown
Yes, the rules are feasible. I created the reusable global skill:
[alfazen-versioning/SKILL.md](C:\\Users\\MZou.codex\\skills\\alfazen-versioning\\SKILL.md)
It defines:
VERSION starting at 1.0.0
Digit carry: 1.0.9 → 1.1.0, 1.9.9 → 2.0.0
pre-commit for bumping/staging the version
prepare-commit-msg for:
m.n.p build yyyy-mm-dd-hhmm feat: message
UTC timestamps
Idempotent amend/rebase handling
--no-verify and rollback caveats
The current project root was left unchanged. Structural checks passed; the bundled validator could not run because Python’s yaml module is unavailable.
```



## How to engage this skill?

You do not need to run code to engage the skill.

There are two ways:

1. Automatic triggering

Ask for a matching task, for example:

> Set up automatic versioning and commit build stamps for this project.

Codex may automatically select `alfazen-versioning`.

1. Explicit triggering

Mention it directly:

> Use Alfazen Versioning to configure this project’s Git versioning.

The skill itself guides Codex; it does not modify every project automatically. Once Codex uses it to create `.githooks/`, `VERSION`, and configure:

```
git config core.hooksPath .githooks
```

the Git hooks run automatically on commits.

For guaranteed use, I recommend explicitly writing Alfazen Versioning in your request. If it does not appear in the skill list, restart Codex; skills are normally detected automatically after installation.
