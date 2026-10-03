# PumpGTM

The `pumpgtm` MCP server (https://mcp.pumpgtm.com/mcp) finds buyers showing intent, runs LinkedIn, email and X outreach from the user's own accounts, and hands every reply back to a human.

- First use: if the server reports it needs authentication, tell the user to run `/mcp auth pumpgtm` and sign in. No API key is needed.
- Start with `get_workspace` to see the connected accounts and what is set up.
- Never start a sequence (`set_sequence_status`) without the user confirming the audience and the message.
- Docs: https://pumpgtm.com/docs/mcp
