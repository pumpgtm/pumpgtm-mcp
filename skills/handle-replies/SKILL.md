---
name: handle-replies
description: Review replies from prospects and answer them. Use when the user asks who replied, what is waiting on them, or to answer a reply.
---

1. Call `list_pending_replies`. For each reply show the person, what they said, and PumpGTM's draft answer.
2. Ask the user what to do with each one. Call `decide_reply` only with the decision they chose, and pass their edited `text` if they changed the draft.
3. `send`, `invite` and `booking_link` contact the person and `opt_out` is permanent, so never pick those without the user's explicit choice.
