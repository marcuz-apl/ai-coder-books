## Claude Code Tricks



## Set 1 - create custom command

Go to your project folder and create a folder called `.claude` and a subfolder named `commands`:

```shell
cd ./claudeDemo
mkdir -p .claude/commands
```

Inside which, create a text file called `quick-audit.md`, then open that file and write down the below:

```shell
touch quick-audit.md
nano quick-audit.md
```

You can write any prompt you want, and also pass any arguments, etc.

```text
Review the file: $ARGUMENTS

Scan for:
1. Performance issues
2. Security vulnerabilities
3. Best practice violations

Provide a summary with severity levels
```

Then, open Claude, and type in:

```shell
/quick-audit src/api/handler.ts
```

Then it runs your custom prompt on the exact file.



## Set 2 - Add remotion skill

Open your terminal and type in:

```shell
npx skills add remotion-dev/skills
```

Then, open Claude, and enter the following prompt to generate a 3D animated scene:

```text
Generate a 3D animated scene: a wildmill in a tulip field at sunset. Spinning blades, flowers swaying in the wind, golden clouds drifting, floating pollen particles. Camara slowly pans across the scene. 5 seconds, 60fps, 1080p. Save as tsx file, named WindmillSunset.tsx.
```

Get the file and go to this website: https://vidtsx.com; 

Input the tsx file.

Boom, you got the video!



## Set 3 - 

Open Claude and type in the prompt:

```text
Spawn 3 subagents to analyze the auth module in parallel:

1. Security agent: find vulnerabilities, injection risks, and auth bypass issues
2. Performance agent: find slow queries, memory leaks, and bottlenecks
3. Bug detection agent: find logic errors, edge cases, and unhandled errors

Report findings seperately.
```

