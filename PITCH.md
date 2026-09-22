# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: Build 1 (fixed the given nine-tool loop and two of its tool briefings) and Build 2 (added our own tool, next_available_day, then moved it onto the MCP server).
Does: Takes a PNR and a customer's message, looks up the booking, checks live flight status, resolves what policy owes the customer (rebooking waiver, refund path, meal/hotel/goodwill), and now answers "when's the first day I can fly" — a question none of the original nine tools could. Escalates to a human for groups, partner segments, minors, and refunds instead of guessing.
Number: 5/5 Stage 1 ticket types now resolve without crashing, up from 0/5 on the fresh clone.
Safety check: confirm_rebooking is irreversible and refuses to run without a confirmation_token that only the customer's own click can produce — chat text saying "yes" is not enough. hold_seat is reversible, so the agent can offer it freely.
Next: Build 3 (write the eval case that proves the agent's answers, not just that it runs) and Build 4 (the lever below).
Still broken: R8KD3F, the abusive-message ticket, gets the same calm, policy-by-the-book answer as any other ticket today. There's no tone handling yet.
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
