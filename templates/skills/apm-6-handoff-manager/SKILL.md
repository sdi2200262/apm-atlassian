---
name: apm-6-handoff-manager
description: Perform a Handoff with an APM Manager. Use only when the User manually invokes this skill.
disable-model-invocation: true
---

# APM {VERSION} - Manager Handoff

Only the current Manager can prepare this handoff. If the User gives you this file or requests a handoff in another conversation, tell them to ask in the current Manager conversation and stop.

Begin when the User requests a Manager Handoff or an APM instruction directs you here.

Use §4.1 for teammate handoffs between Stages.

A Manager Handoff continues coordination in a new Manager conversation in the same session. Write a Handoff Log to preserve your working context and a Handoff Prompt to direct the incoming Manager. It also reads the planning documents, Tracker and Index per `{SKILL_PATH:apm-2-initiate-manager}`.

Your conversation number starts at 1 and advances only at Handoff. Use the number from your incoming Handoff Prompt if present; the next conversation is one higher.

## 1. Current State

If subagents are running when a Handoff is requested, tell the User, pause them and collect their progress, outputs and pending checks before preparing the Handoff.

Check current state before writing either file:

- Read the Tracker and each Worker’s bus files. Check progress, outputs and pending validation for your active Tasks, including subagent contributions.
- If using version control, verify branches and integration. For Git, check worktrees as well.

Check the continuation details in §3. Before writing the Handoff files, use verified state and agreed changes to update the Tracker and any affected planning documents, Index Notes and Task instructions. Record pending Task actions in the Tracker’s Next Action.

## 2. Handoff Log

Write the Handoff Log in past tense per §5.1. If context was compacted or recovered, identify what you rebuilt from summaries or files. Name the sources and any uncertainty or unverified claims that remain.

## 3. Handoff Prompt

Write `.apm/bus/manager/handoff.md` per §5.2. Name the latest relevant attempt logs for current Tasks and earlier work needed by pending reviews or dependencies; earlier attempts remain accessible through links. Include resume points and sources for pending alignment or verification gaps.

Record what work the User holds, any assistance needed, where to resume and what allows work to resume. Carry forward agreed review points, pending decisions and accepted limits with evidence links.

## 4. Continuation Instructions

Give a small chat recap of the work and context being passed on, pending decisions or checks, and the next action. Explain how the User starts a new Manager conversation; it finds the Handoff Prompt. You may link the files for optional review, but do not require inspection or approval.

By default your preparation ends here: wait while the User completes delivery to the new conversation. Do not claim to transfer yourself. Follow a different coordination mechanism only when explicitly agreed with the User.

### 4.1 Teammate Handoff

1. Confirm publication of the completed Stage, including closure of its Epic, per `{GUIDE_PATH:task-review}` §7. Resolve any pending publication the next teammate needs. Confirm the next Stage exists and has yet to start in the Tracker and Jira. Record the agreed incoming teammate and next Stage in Tracker Notes. Use the Tracker and Plan to find every started Worker with a Task in a later Stage. Ask the User about any chat you cannot confirm.
2. Ask the User to request a Worker Handoff in each of those chats and say that it is for a teammate handoff. Reuse any verified Prompt and Log for this transfer. Wait for the rest. Check each Worker ID, next conversation number and log path. Resolve any leftover messages so each continuing Worker's `task.md` and `report.md` are empty.
3. Prepare the Manager Handoff per §§1–3. Identify it as a teammate handoff in the Prompt. Record the next Stage and Epic, its approval or pending decision, and each continuing Worker's ID, next conversation number and Prompt path.

   - In Current State, give or link the repository URLs, relative paths, branches and exact commits to fetch. Note what is merged and what still needs integration. Identify any worktrees to recreate by repository, relative path, branch and commit.
   - In Next Action, give or link the setup steps, relevant test environment, and any build or evidence the next Stage needs. Say where to obtain local inputs or access. Keep credentials out of the Prompt.

4. Verify that the commits named in the Prompt are fetchable. Commit and push the Manager and Worker Handoff files and shared session files, including any agreed `AGENTS.md` changes. After the push, tell the User to end work in the outgoing chats and pass the Epic, Bitbucket repository and branch to the incoming teammate. The incoming teammate can then claim the Epic and open a new Manager chat at the session root, naming the branch and saying it is a teammate handoff. Stop work in this chat.

## 5. Formats

### 5.1 Handoff Log Format

The Handoff Log is written at `.apm/memory/handoffs/manager/handoff-<NN>.log.md`, where `<NN>` is your conversation number, zero-padded, such as `handoff-01.log.md`.

```markdown
---
agent: manager
conversation: <number>
stage: <N>
---

# Manager Handoff Log - Conversation <number>

## Summary
<The Stages, Tasks, reviews and dispatches handled in this conversation>

## Working Context
<Decisions with reasons, User preferences, dispatch patterns and coordination observations that the Tracker, the Index and the Task Logs do not hold>
```

### 5.2 Handoff Prompt Format

```markdown
---
agent: manager
conversation: <number + 1>
handoff_log: <path of the Handoff Log>
---

# Manager Handoff Prompt - Conversation <number + 1>

<What to read: the Handoff Log and the Task Logs the next conversation needs, by path>

## Current State
<Checked state: your active Tasks and progress, reports awaiting review, Workers with dispatched Tasks, work locations and pending integration or cleanup; pending alignment decisions, accepted limitations and verification gaps, with source paths>

## Next Action
<The coordination step to resume with>
```
