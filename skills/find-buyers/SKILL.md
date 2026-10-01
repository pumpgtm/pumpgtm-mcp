---
name: find-buyers
description: Find people who match an ideal customer and start outreach to them on LinkedIn, email or X. Use when the user asks to find leads, prospects or buyers, or to start a campaign.
---

1. Call `get_workspace`. If `setup` lists a missing step (LinkedIn or billing), show its link and stop until it is done.
2. Call `find_people` with `targeting` and without `confirmed`. Show the targeting summary and candidate count and ask if it looks right.
3. Call `find_people` again with `confirmed: true`. Show every person in the order returned: name, title, company, LinkedIn URL and match reason. Do not rank or shortlist them.
4. Ask which to add ("add all", "add 2, 5 and 8") and record the answer with `review_people`.
5. If no sequence fits, write one with `save_sequence_draft` (use the `write-cold-message` skill for the copy) and confirm it with the user before it goes live.

Nothing is sent to anyone until the user approves the sequence.
