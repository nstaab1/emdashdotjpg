---
name: skill-feedback
description: "File skill feedback, an issue in Nathan's skills repo about a skill that misled you, then carry on with the task. Use when following a skill's instruction produced a wrong result or a failed step, when you had to deviate from a skill, or when Nathan corrects what a skill told you to do."
metadata:
  never-shared: true
---

**Skill feedback** is a report that one of Nathan's skills **misled** an agent. It is an issue on `nstaab1/nathan-agent-skills`, the repo where the skills are edited, and an audit reads it there later.

File it on your own, when it happens, while you still hold the detail. Nathan hears about it once, in your final message. Then pick the task up at the step you left.

Pass `-R nstaab1/nathan-agent-skills` on every `gh` call here, because this project's own remote is a different repo.

## When to file

A skill misled you when one of these happened:

- Following its instruction produced a wrong result or a failed step.
- You had to deviate from it, because it was ambiguous, contradicted the repo or pointed at something that doesn't exist.
- Nathan corrected behaviour it prescribed.

These are not skill feedback, so file nothing for them:

- A bug in the task itself. The skill's instruction was sound, and the code, the tests or the environment were wrong.
- Nathan's preference about one change. The correction settles this change and would leave the skill's text as it is.
- A skill installed from another source. Step 1 decides this.

A gap that a code review or a reviewer's comment showed you is skill feedback, but don't file it on your own, even when you then changed the code against the skill. The skill running the review proposes it in its Skill feedback section, and you file it only once Nathan answers `file`.

## 1. Check the skill is in scope

Read `skills-lock.json` at the project root. The skill is in scope when its entry's `source` is `nstaab1/nathan-agent-skills`. Keep the entry's `commit` for the issue, or `unknown` when the entry has none.

With no lock file, no entry for the skill, or another `source`, the skill is out of scope. File nothing and go back to the task.

Done when you hold the skill's name as the lock file spells it and its installed commit, or you are back in the task.

## 2. Write the issue

The title is `<skill>: <one-line problem>`, such as `ns-to-spec: the publish step names a label the tracker doc doesn't define`.

The body is this template. Keep every label and heading exactly as written, in this order, because the audit parses them.

```markdown
Skill: <skill>
Installed at: <commit, or unknown>
Source: <owner/repo>, <PR URL, or session reference>
Severity: <blocked | wrong | friction | polish>
Filed by: <agent | approved by Nathan>

## What happened

<The instruction, quoted from the skill, and what following it led to.>

## Expected

<What the skill should have had you do.>

## Suggested change

<The edit to the skill's text that would have prevented this, or None.>
```

- **Source.** `<owner/repo>` comes from `git remote get-url origin`, or is the project folder's name when there is no remote. After it goes the URL of the branch's open PR. With no PR, write `session <branch> <YYYY-MM-DD>`.
- **Severity.** Take the first that fits.

  | Severity | What happened |
  | --- | --- |
  | `blocked` | You couldn't finish what the skill is for. |
  | `wrong` | Following it produced a wrong result, which you redid or Nathan corrected. |
  | `friction` | The result came out right, but only because you deviated or guessed. |
  | `polish` | The wording or layout slowed you down and changed nothing you did. |

- **Filed by.** `agent` when you file on your own. `approved by Nathan` when Nathan approved this item before you filed it.
- **What happened.** For a correction, give what Nathan said in your own words.

Write about the skill, in prose. Quote the skill's own text freely, since it comes from the skills repo. Everything that belongs to this project stays in this project, so paste none of its code, file contents, command output or secrets. Where the account needs a file, name its role, as in "the test script in `package.json`".

Write the body to a file in the system temp directory, so the working tree stays as it was.

Done when the title and the body file exist and the body holds all five lines and all three headings.

## 3. File it, or add it to an open issue

List the open feedback for this skill:

```bash
gh issue list -R nstaab1/nathan-agent-skills --label skill-feedback --state open --limit 200 \
  --json number,title,url,body --jq '[.[] | select(.title | startswith("<skill>: "))]'
```

Take the first of these that applies:

1. The list holds an issue you filed or commented on earlier in this session. Comment on it. A session uses one issue for each skill, so a second problem with the skill goes there even when it is a different problem.
2. The list holds an issue about the same problem, which is the same instruction failing the same way. Comment on it, so a repeat adds weight to one issue.
3. Otherwise create the issue:

   ```bash
   gh issue create -R nstaab1/nathan-agent-skills --title "<title>" \
     --label skill-feedback --label needs-triage --body-file <file>
   ```

A comment is the title on its first line, a blank line, then the same body:

```bash
gh issue comment <number> -R nstaab1/nathan-agent-skills --body-file <file>
```

GitHub can't be reached when `gh` is missing or signed out, the network is closed, or a call still fails on a second try. Keep the title and the body for your final message and move on.

Done when the issue or the comment has a URL, or the title and body are set aside for the final message.

## 4. Report it in your final message

Work these lines out now, go back to the task, and put them at the end of the final message you send when the task is done.

- For each issue or comment, one line: `Skill feedback: <title> (<URL>)`.
- For feedback GitHub couldn't take, one fenced block that Nathan can paste into a new issue on `nstaab1/nathan-agent-skills`. Fence it with four backticks, so a fence inside the body survives.

  ````markdown
  Title: <title>
  Labels: skill-feedback, needs-triage

  <body>
  ````

- When neither `AGENTS.md` nor `CLAUDE.md` at the project root contains `/skill-feedback`, say that the project lacks the skill-feedback line and print [`agents-md-line.md`](agents-md-line.md) for Nathan to add under `## Agent skills`. Leave both files as they are.

Done when the final message carries one line or one block for each piece of feedback, and the missing line when the project lacks it.
