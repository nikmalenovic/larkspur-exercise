# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: Build 1 (fixed the loop and two tool briefings), Build 2 (added next_available_day locally then moved it to MCP), Build 3 (three tone-safety hard-gate evals, all passing), Build 4 (cost lever: prompt caching on the stable system prompt).
Does: Takes a PNR and a customer's message, looks up the booking, checks live flight status, resolves what policy owes the customer (rebooking waiver, refund path, meal/hotel/goodwill), and answers "when's the first day I can fly" via MCP. Escalates for groups, partner segments, minors, refunds, legal threats, and offensive-language situations.
Number: 5/5 Stage 1 ticket types now resolve without crashing, up from 0/5 on the fresh clone.
Safety check: confirm_rebooking is irreversible and refuses to run without a confirmation_token that only the customer's own click can produce — chat text saying "yes" is not enough. hold_seat is reversible, so the agent can offer it freely.
Next: Extend evals to Stage 2 ticket types; add pax_count support to next_available_day for group-sized parties.
Still broken: next_available_day answers for a party of one — a group booking with 6 passengers may get a date where only 1 seat is open, not 6.
Lever: cost

## Priya asked

Costs: $0.024 model cost per resolved contact after caching (down from $0.067 before), ~$330/week at 13,700 chats/week. That is model inference only — loaded cost including infrastructure and evals runs roughly 40% higher, so call it ~$460/week fully loaded. Against $94,530/week for human agents at the same volume, the model cost is under 0.5% of the alternative.

Wrong: The agent can state something confidently that a tool result does not support — a fluent hallucination that reads as correct. The judge grader is designed to catch exactly this (it is shown every tool result, not just the agent's words), but it catches it in eval, not in production. The other failure mode is an edge case that falls outside the five Stage 1 shapes: a codeshare segment, a same-day connection, a partially-used ticket. Those reach escalate_to_human today, which is the right outcome, but it is worth naming.

Runs it: The MCP server (support/mcp_server.py) is a separate process that must be running alongside the agent. Today it starts per-conversation; in production it would be a long-lived service. The agent process itself is stateless — each conversation is independent. Ops owns the MCP server; product owns the agent prompt and tool list; whoever writes the eval cases owns the gate.

Left out: Refunds — the agent escalates every refund request to a human rather than processing it. Multi-passenger group bookings — next_available_day answers for a party of one and may return a date with only one open seat. Proactive outreach — the agent only responds, it does not initiate. Anything on a partner-operated segment is escalated without an answer.
