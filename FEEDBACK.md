# Overnight review: Larkspur disruption-care agent

**To:** nikmalenovic_larkspur-exercise  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-22 17:23

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Overnight, Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py's tool_list() now appends mcp_client.tools(), so next_available_day rides the MCP server rather than a local schema.**

The diff changes tool_list() from build_tools() + EXTRA_TOOLS to build_tools() + EXTRA_TOOLS + mcp_client.tools(). EXTRA_TOOLS itself is still an empty list per the static scan, and LOCAL_TOOLS has no executors, so this team's one added capability lives entirely off-repo on the MCP side. PITCH.md calls this Build 2: added our own tool, next_available_day, then moved it onto the MCP server.

Run python3 run.py --show-tools and paste the entry for next_available_day to confirm its schema and description.

**2. The last committed trace shows next_available_day actually firing, but only inside a 2-tool-call, 3-turn run.**

readout-trace.json lists tools called, in order: lookup_booking, next_available_day with 3 API turns and 2 tool calls total. That means the run never touched get_flight_status, check_policy, hold_seat, confirm_rebooking, issue_voucher, escalate_to_human, or send_confirmation in this trace. Whether the new tool composes correctly with policy and rebooking logic on a harder ticket is not shown anywhere in this material.

Run python3 run.py K7PQ2M --trace on a ticket that requires check_policy after next_available_day and paste the tool-call sequence.

**3. search_alternatives description grew from 6 to 69 characters, still the shortest of the nine tool briefings by a wide margin.**

The diff rewrites the description to "Search for other flights when the customer's original one won't work." at 69 characters. check_policy's description runs 445 characters and covers cause_code, delay_minutes, fare_family, and policy_row_id citation rules. A model choosing between tools on a live ticket has far less to go on for search_alternatives than for check_policy, and that gap is a description problem no model swap fixes.

Run python3 run.py --tool-tax and paste the token cost attributed to search_alternatives versus check_policy.

**4. TONE_ADDENDUM sits at 0 characters while PITCH.md names the abusive-message ticket R8KD3F as still broken.**

The static scan confirms TONE_ADDENDUM: still empty (0) characters. PITCH.md states plainly: R8KD3F, the abusive-message ticket, gets the same calm, policy-by-the-book answer as any other ticket today. There's no tone handling yet. The Lever line in PITCH.md is left as the literal placeholder <cost | speed | intelligence>, so no direction has been chosen for Build 4 yet.

Run python3 verify.py 4.1 once TONE_ADDENDUM is written and paste the gate result.

**5. No evals/cases.json exists, so the 5/5 Stage 1 number in PITCH.md has no case-by-case artifact behind it.**

PITCH.md's Number line claims 5/5 Stage 1 ticket types now resolve without crashing, up from 0/5 on the fresh clone. The repository listing confirms there is no evals/cases.json in this repository, and eval_harness.py would be the only way to reproduce that ratio against named cases. Banked gates in readout.html show 1.2, 1.3, 1.4, 2.1, 2.2, which covers the fixed loop and tool briefings but nothing under a Build 3 label.

Run python3 eval_harness.py and paste the totals line once cases exist.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (224 lines)`
- `PITCH.md`
- `TEAM.md (unchanged template)`
- `readout-trace.json`
- `readout.html (evidence block)`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
