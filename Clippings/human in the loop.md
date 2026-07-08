---
title: /human in the loop
source: https://x.com/alex_prompter/article/2072003526755266744
author:
  - "[[Alex Prompter (@alex_prompter)]]"
published: 2026-07-01
created: 2026-07-03
description: Stop approving every action your AI takes. Start designing where you sit in the loop.Approving every action gets you consent fatigue and a r...
tags:
  - clippings
---
![图像](https://pbs.twimg.com/media/HMEo_rzW0AI-QlA?format=jpg&name=large)

Stop approving every action your AI takes. Start designing where you sit in the loop.

Approving every action gets you consent fatigue and a rubber-stamp reflex. Designing your checkpoints gets you output quality that improves with every session.

Anthropic published internal telemetry from Claude Code showing that users approve 93% of permission prompts. Their own engineers named the problem consent fatigue. The human is nominally in the loop but functionally checked out.

Their fix wasn't to remove humans from the process. It was to restructure where humans participate, moving from per-action approval to plan-level review, monitoring, and targeted intervention.

![图像](https://pbs.twimg.com/media/HMEpUfjWUAAaWO3?format=jpg&name=large)

Chip Huyen said the same thing from a different direction. In AI Engineering, the most-read technical book on O'Reilly since its release, her advice is to start with a human in the loop and gradually increase automation as confidence in the AI grows. Microsoft built an entire adoption framework around the same principle and called it Crawl-Walk-Run.

Human in the loop is a workflow architecture. You define where human judgment enters the process, the AI iterates against those checkpoints, and what comes out improves until neither side could match the result alone.

## what human in the loop actually is

Anthropic analyzed roughly 400,000 Claude Code sessions between October 2025 and April 2026. One pattern held across nearly all of them. People make the planning decisions. The AI makes the execution decisions.

The more domain expertise a person brings to a session, the more work the AI completes per instruction and the higher the success rate. That's not a safety concept. That's a workflow architecture, and it breaks into three phases.

**Input** covers what happens before the AI starts working. Your task description, your constraints, your context, the examples you provide. People almost always underinvest here and then spend twice the time correcting outputs that went sideways from the first prompt.

**Steering** is what happens while the AI is working. Monitoring its direction, intervening when something drifts, reviewing the plan rather than approving each individual action.

Anthropic's data showed that experienced users auto-approve individual actions at twice the rate of new users but interrupt the agent mid-execution far more often. They watch the trajectory, not steps.

**Output review** is the final gate. What you accept, what you reject, and the criteria you judge against. This sounds obvious, but nearly everyone skips it and ships the first output they receive.

Every working human-in-the-loop workflow is some combination of these three.

![图像](https://pbs.twimg.com/media/HMEphIAWIAAxOMW?format=jpg&name=large)

## start with output review

This is the simplest entry point and the one that pays off fastest.

At their Code with Claude event, Anthropic shipped a feature called Outcomes. It adds a grading loop where you define what "good enough" looks like, and the system evaluates its own output against those criteria before delivering. Nothing about the model changed. The same AI ran the same prompts with one addition, a grading checkpoint.

The result was a 10.1% quality improvement on slide generation and 8.4% on document generation.

That number matters because it proves a meaningful portion of bad AI output isn't a model problem. It's an evaluation problem. Nobody was checking whether the output was actually good before it shipped.

You don't need a platform feature to do this yourself. The practice is simpler than the tooling. Before you start any AI task, write down what a good result looks like in concrete terms.

Then compare the output against your own criteria before you use it. One pass of intentional review against a defined standard catches the majority of the errors that make AI output feel unreliable.

Don't overhaul your entire workflow on day one. Pick one recurring AI task you already run, whether that's emails, first drafts, summaries, or code, and add a single review checkpoint. That alone is already more human-in-the-loop than nearly anyone practices.

## add the steering layer

Output review catches problems after they happen. Steering prevents them from happening in the first place.

The old model of steering was per-action approval. The AI proposes something, you approve or reject, the AI proposes the next step. Anthropic's 93% approval rate proved this approach is broken at scale.

At hundreds of actions per session, people stop reading what they're approving. Ilya Kabanov's analysis of Anthropic's own research described the reality plainly. What's actually happening is incident response, not oversight.

Anthropic's alternative is a pattern they call Plan Mode. Instead of approving each action individually, the AI presents its intended sequence of actions upfront. You review, edit, and approve the plan as a whole before anything executes.

You can still intervene during execution, but the cognitive load moves from "evaluate 200 micro-decisions" to "review one plan and watch for drift."

This pattern translates directly to any AI workflow regardless of the tool. Give the AI a structured task. Ask it to outline its approach before executing. Review the outline, then let it run and check back at natural breakpoints rather than hovering over every step.

The practical implementation for daily work is a steering document. A CLAUDE.md file, a custom instruction set, or a system prompt that encodes your standards, constraints, and recurring preferences. One well-maintained steering document replaces hundreds of mid-session corrections because the AI already knows your rules before you start typing.

## build the input layer

This is the phase that separates people who get reliable results from people who keep fighting their AI tools.

People almost always start a session by typing a vague instruction and then spend 80% of their time correcting the output. That's the loop running backward. The human effort lands at the end, in re-prompting and output correction, when it should land at the beginning.

Anthropic's session data showed this pattern directly. The greater the domain expertise a person brings, the more work the AI completes per instruction.

Expertise here doesn't mean technical skill. It means knowing what you want and being able to describe it precisely. A senior engineer who writes a clear task description with constraints and context gets more done per prompt than someone who writes "fix the bug" and waits.

![图像](https://pbs.twimg.com/media/HME5OiSasAAN56x?format=jpg&name=large)

The input layer is where you encode that precision. Your task descriptions, your context files, your reference examples, your boundaries. What the AI needs to understand is not just what to do but how you want it done and where the limits are.

A reliable structure is three parts. First, state the goal in one sentence. Second, define the constraints, meaning what the output must include and must not include.

Third, provide an example of what good looks like. That triad gives the AI enough structure to produce something usable on the first pass, which means your output review catches refinements instead of fundamental rewrites.

Building this feels like extra work upfront, and it is. But it saves multiples of that time on every session that follows. The people who invest in their input layer once start improving immediately. The people who never build one keep resetting to zero every time they open a new chat.

## run the full loop

Here's what all three phases look like working together in a single task.

A developer needs to refactor a payment processing module. Without human-in-the-loop design, this starts with a prompt like "refactor the payment module" and ends with a 500-line diff that changed the public API, broke two tests, and used a different naming convention than the rest of the codebase. The developer spends an hour manually fixing the output. Net time saved approaches zero.

With the three phases in place, the same task runs differently.

**The input** is structured before the AI touches anything. "Refactor the payment processing module. Don't change the public API. Keep backward compatibility with v2 endpoints. Match the naming conventions in /src/auth. Here's the current test suite as context." That takes about five minutes to write and eliminates 80% of the corrections that would have happened later.

**The steering** happens when the AI outlines its refactoring plan before executing. It proposes extracting three helper functions, consolidating error handling, and restructuring the retry logic. The developer spots that the AI planned to merge two functions that need to stay separate for rate-limiting reasons. One redirect, ten seconds. The AI revises its plan and executes.

**The output review** takes about ten minutes. The developer checks the diff against their original constraints. Public API unchanged. Tests pass. Naming conventions match. They accept.

The first approach (no loop) took 90 minutes. The second (with the loop) took about 25. The difference isn't that the AI got smarter between the two attempts. It's that the human invested their time at the front of the process instead of the back.

That's the pattern for any AI-assisted task, not just code. Writing, research, analysis, design. Front-load your input, steer the plan, review the output against defined criteria. The loop is the same regardless of the domain.

## watch what your work becomes

When all three phases click into place, your daily interaction with AI changes shape entirely.

You stop typing vague requests and correcting outputs for an hour. You start writing structured inputs that take five minutes and produce usable results on the first or second pass. Your steering documents accumulate knowledge between sessions, so the AI improves at your specific work over time without any model upgrade needed.

Your task list stops being a list of things to produce and becomes a list of things to review. The center of your day moves from production to direction. You're not writing the first draft anymore. You're defining what a good draft looks like, reviewing what comes back, and refining the standard for next time.

This doesn't mean you stop thinking. It means your thinking moves to a different altitude. Instead of choosing words, you're choosing direction. Instead of debugging code line by line, you're reviewing architecture and catching mistakes before they cascade.

The parts that genuinely need a human, things like judgment, taste, domain knowledge, and strategic decisions, get more of your attention because the parts that don't are handled.

The people who build this workflow once will keep building on it for years. The people who keep typing one-off prompts and hoping for the best will keep resetting to zero every session.

The old version of human in the loop asked you to approve every action. That was never going to scale, and the data proves it.

The new version asks you to invest in three things. Your input, your steering, and your review criteria. Build those once, and the AI handles more with each session while you handle less. Not because you're being replaced, but because you're finally operating at the altitude where a human actually makes a difference.