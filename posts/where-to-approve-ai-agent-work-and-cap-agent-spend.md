# Where to approve AI agent work and how to cap each agent's spend

Put a human approval before publishing, sending, deploying or spending over a chosen amount; as a starting recommendation, let reversible preparation run. For token-billed agents, set a monthly dollar cap with the default 80% warning and 100% hard stop; for subscription connections, where runs record $0 and usually add no per-call charge, also use approval gates, wake settings and a turn cap, only after checking whether the runner honors it.

## Where to put human approval

Advice: require approval when an action leaves your control, changes shared infrastructure, or commits money beyond your chosen threshold. Prepare the exact action first, so the human can review something concrete. Paperclip lets your organization choose which actions to gate; the examples below are a suggested setup. Nothing here is gated until you configure it. The budget stop, covered below, is the exception.

| Gate | Trigger | Who approves |
|---|---|---|
| Plan review | A larger effort is ready to fan out into staffed work | Human reviewing the specific plan revision |
| Connector action | An action configured as Ask first reaches the tool gateway | Human, for each call |
| Spend request | An agent reaches the spending threshold written in its instructions | Board user reviewing the proposed action |
| Task completion | The executor tries to close a task with a configured review or approval stage | Named reviewer or approver; stages can include agents |
| Budget stop | Recorded spending reaches 100% with hard stops enabled | Board user decides whether to raise the cap and resume |

Choose the gate that matches the moment you need control. Plan approval binds to a specific revision; changing the plan resets that approval. A task completion stage intercepts the move to done.

Ask first requires approval for each connector call through Paperclip's tool gateway. New connection actions normally start Allowed, including writes and destructive actions, so configure the relevant actions deliberately. Some actions can't be enabled at all. The connector documentation says Gmail permanently blocks send, trash, spam, and label mutation.

A spend request has a weaker boundary. The agent's instructions tell it to ask; Paperclip doesn't detect the spending action for it. An agent that ignores those instructions can bypass the request. Pair this rule with budget controls.

## Give the approver an exact action

Paperclip describes requests containing context, diffs, and a recommendation. What the human actually sees depends on what the agent supplies. Advice: include the proposed action, its cost or scope, the reason to proceed, and the main risk. Ask the human to approve that action.

An approver can approve, reject, or request revision, and leave a note. Requesting revision doesn't automatically wake the agent.

## Walk through one agent's budget

Paperclip supports company, agent, and project caps. An agent at its own limit pauses even when the company still has room. Company and agent budgets reset at 00:00 UTC on the first of each month. Project budgets default to lifetime, though they can use a monthly reset.

Advice: start sizing with a tolerable monthly loss. Assign each agent an amount you can accept spending on its current task, within the company total. Review recorded costs after representative completed work, then adjust the allocation. Company-level starting range, from Paperclip's [cost guide](https://docs.paperclip.ing/guides/day-to-day/costs/): "Start conservative — $50–100/month is enough for experimentation with a small team."

Consider a $50 agent cap. At the default warning threshold, $40, Paperclip records a warning and work continues. The threshold is configurable. Paperclip's documentation says it pushes no outbound webhooks for incidents.

At $50, with hard stops enabled, Paperclip opens a hard-stop incident, pauses that scope, cancels queued and running runs, and refuses new ones. Tasks stay assigned. Hard stops are on by default for a budget you set. A warn-only policy keeps spending.

Only a board user can raise the cap and resume work. The new amount must exceed current observed spending. Resolution can still fail while costs await recording or blocking unpriced usage remains. Alternatively, keep the scope paused until reset. At the monthly reset, a scope paused only for budget reasons resumes automatically; pause it manually if that's unwanted.

## Handle subscription connections separately

Check turn-cap support before relying on it. A pull request merged October 24, 2026 says Paperclip Runner ignores Claude's maximum-turns setting. The adapter documentation describes a cap without that qualifier. The sources reviewed here don't settle which other runners honor it, and none gives a general rule for Codex.

The documented settings also include interval heartbeats, Timeout, Wake on demand, Cooldown, Max concurrent runs, and Continue after max-turn stop. The source names these settings without defining all their effects. With interval off, the docs say the agent wakes when it's given work or when you click run now.

## What a cap does not stop

A cap checks recorded cost as runs finish. In Paperclip's $50 example, $47 plus a $6 run becomes $53, before other runs in flight are counted. Cancellation and reporting can lag; this is an example, not typical overshoot.

A cap governs Paperclip-managed work. Provider billing falls outside it. Spend that never reaches Paperclip isn't counted. Subscription runs record $0 and usually add no per-call charge, but extra usage charges can occur. Subscription fees sit outside agent budgets. Inference: a dollar cap has nothing to count while recorded costs stay zero. Missing cost doesn't mean zero. For OpenAI Dot, an experimental Runner provider, unreported cost prevents enforcement of a provider spend ceiling; known company and agent limits still apply.

## A starter setup for two to ten agents

Advice: give every agent its own cap, as the docs advise. Let a research agent prepare notes without a human gate. Give a writing agent an agent reviewer at task completion. Require human approval for a publishing agent's supported connector calls by setting them to Ask first. Put the spending threshold in each agent's instructions and retain budget hard stops. These are suggested roles and rules.

To protect human attention, reserve human decisions for selected external actions and spending commitments. Advice: use agent review for preparation. Whether that reduces approval volume is unproven. Where a task has review and a responsible human, the default third consecutive agent change request sends it to a human. That default limits review churn. Human approval count falls outside its scope. Keep each boundary clear: task review gates completion, Ask first gates connector calls, and spend requests depend on instructions. None of them approves shell commands inside a run.

## Short FAQ

**Does a budget cap the provider's invoice?**  
No. See [the control-plane explanation](https://paperclip.ing/blog/what-is-an-ai-agent-control-plane/).

**What does task review gate?**  
Task completion. See [execution policy](https://docs.paperclip.ing/guides/power/execution-policy/).

**Where should I start with overnight work?**  
Paperclip's [overnight guide](https://paperclip.ing/blog/run-ai-agents-unattended-overnight/) covers that setup, including how low to size an overnight budget.
