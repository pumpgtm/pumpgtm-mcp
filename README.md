<p align="center"><a href="https://pumpgtm.com"><img src="assets/banner.png" alt="PumpGTM" width="100%"></a></p>

<h1 align="center">PumpGTM MCP server</h1>

<p align="center"><b>Run LinkedIn, email and X outreach from any AI agent.</b><br>
PumpGTM finds the buyers showing intent this week, drafts and sends the outreach from your own accounts inside each platform's limits, and hands every reply back to a human. This is the public reference for the hosted MCP server at <code>https://mcp.pumpgtm.com/mcp</code>.</p>

<p align="center">
<a href="https://pumpgtm.com/docs/mcp"><img alt="Docs" src="https://img.shields.io/badge/docs-pumpgtm.com%2Fdocs%2Fmcp-2f5cff"></a>
<a href="https://modelcontextprotocol.io"><img alt="MCP" src="https://img.shields.io/badge/MCP-Streamable%20HTTP%20%2B%20OAuth%202.1-000000"></a>
<a href="tools.json"><img alt="Tools" src="https://img.shields.io/badge/tools-34-1f883d"></a>
<a href="https://github.com/pumpgtm/pumpgtm-api"><img alt="REST API" src="https://img.shields.io/badge/REST%20API-OpenAPI%203.1-6f42c1"></a>
<a href="https://pumpgtm.com/yc"><img alt="Backed by Y Combinator" src="https://img.shields.io/badge/Backed%20by-Y%20Combinator-F26522"></a>
<a href="https://pumpgtm.com/blog/ai-founder-led-outbound-system"><img alt="Runs outbound for 10+ YC companies" src="https://img.shields.io/badge/runs%20outbound%20for-10%2B%20YC%20companies-F26522"></a>
<a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/license-MIT-lightgrey"></a>
</p>

<p align="center">
<a href="https://pumpgtm.com/docs/quickstart">Quickstart</a> · <a href="https://pumpgtm.com/docs/mcp">Docs</a> · <a href="tools.json">tools.json</a> · <a href="https://github.com/pumpgtm/pumpgtm-api">REST API</a> · <a href="https://pumpgtm.com/mcp/platforms">Platforms and agencies</a> · <a href="https://www.youtube.com/@pumpgtm">YouTube</a>
</p>

## What your agent is driving

The same workspace the dashboard shows. Plays find people, sequences reach them across LinkedIn, email and X, and every reply waits for a human.

<p align="center"><img src="assets/plays.png" alt="PumpGTM Plays: describe who you want, start from a list, or reach everyone who engaged with a post" width="100%"></p>
<p align="center"><img src="assets/sequences.png" alt="A running PumpGTM sequence with its funnel and message variants" width="100%"></p>

## How it fits together

```mermaid
flowchart LR
  A[Your AI agent<br/>Claude, Cursor, Codex, your own] -- MCP over Streamable HTTP --> M[mcp.pumpgtm.com]
  M --> W[Your PumpGTM workspace]
  W -- find_people --> P[Buyers showing intent]
  W -- sequences --> L[LinkedIn]
  W -- sequences --> E[Email]
  W -- sequences --> X[X]
  L & E & X -- replies --> R[Human decides]
  R -- webhook reply.received --> A
```

## Watch and read

<table>
<tr>
<td width="50%" align="center"><a href="https://pumpgtm.com/blog/what-is-pumpgtm-how-it-works"><img src="https://img.youtube.com/vi/wEb9ZDUIsc0/hqdefault.jpg" alt="What is PumpGTM and how does it work" width="100%"></a><br><sub><b>What is PumpGTM, and how does it actually work?</b><br>4 minute walkthrough</sub></td>
<td width="50%" align="center"><a href="https://x.com/NamanyayG/article/2097783972302557668"><img src="https://pumpgtm.com/blog/covers/ai-founder-led-outbound-system.png" alt="Using AI to run founder-led outbound for 10 YC companies" width="100%"></a><br><sub><b>Using AI to run founder-led outbound for 10 YC companies</b><br>Namanyay's article on X: one lead universe, shared prospects, copy measured by meetings. <a href="https://pumpgtm.com/blog/ai-founder-led-outbound-system">Read on pumpgtm.com</a></sub></td>
</tr>
</table>

More on the [PumpGTM YouTube channel](https://www.youtube.com/@pumpgtm) and in [PumpGTM in the wild](https://pumpgtm.com/customers/videos), videos founders and creators made on their own channels.

## Connect

Endpoint (Streamable HTTP): `https://mcp.pumpgtm.com/mcp`

Two ways to authenticate, both scoped to one workspace:

- **OAuth 2.1** for Claude (web and desktop), Claude Code, Codex, Cursor, ChatGPT and Grok. Add the endpoint and sign in; no key is needed. Dynamic client registration and PKCE are supported; metadata is at `https://pumpgtm.com/.well-known/oauth-authorization-server`. Access tokens last 30 days, refresh tokens 90.
- **Workspace key** for your own agent, the MCP SDKs, and the REST API. Sign in at [app.pumpgtm.com](https://app.pumpgtm.com), open **MCP** in the left nav, and copy the key. Send it as `Authorization: Bearer eve_mcp_..` on every request. The key does not expire. Rotation is not self-serve yet: write to hello@pumpgtm.com and we rotate it for you.

The key's database row fixes the workspace. It cannot read or act on any other workspace.

```ts
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StreamableHTTPClientTransport } from "@modelcontextprotocol/sdk/client/streamableHttp.js";

const client = new Client({ name: "your-agent", version: "1.0.0" });
await client.connect(
  new StreamableHTTPClientTransport(new URL("https://mcp.pumpgtm.com/mcp"), {
    requestInit: { headers: { Authorization: "Bearer YOUR_WORKSPACE_KEY" } },
  }),
);
const { tools } = await client.listTools(); // up to 38 tools, depending on the workspace's features
```

ChatGPT (until the PumpGTM listing is approved in the ChatGPT directory; Developer mode is no longer needed): open **Plugins** (chatgpt.com/plugins), click **+** and choose **Add custom MCP server**. Name it PumpGTM, paste `https://mcp.pumpgtm.com/mcp` as the Server URL, keep OAuth, click **I understand and want to continue**, then **Create as a plugin**. Open your personal plugins (chatgpt.com/plugins?view=personal), open PumpGTM and click **+** to install it, then sign in to PumpGTM and allow the connection. In a chat, type @PumpGTM and ask it to find buyers.

Claude Code: `claude mcp add --transport http pumpgtm https://mcp.pumpgtm.com/mcp`, then run `/mcp` (or `claude mcp login pumpgtm`) and sign in. Codex: `codex mcp add pumpgtm --url https://mcp.pumpgtm.com/mcp` then `codex mcp login pumpgtm`. Cursor: add a server with only the URL and sign in when it asks. Cline: add `"pumpgtm": { "type": "streamableHttp", "url": "https://mcp.pumpgtm.com/mcp" }` under `mcpServers` in `cline_mcp_settings.json` and sign in when it asks. Grok: at grok.com/connectors choose New Connector, then Custom, paste `https://mcp.pumpgtm.com/mcp`, and sign in (works on the free plan).

## Install as a plugin

The PumpGTM plugin bundles this server with five skills: write a cold message, make a message sound human, find buyers, handle replies, and look up a person. In Claude Code:

```
/plugin marketplace add pumpgtm/pumpgtm-mcp
/plugin install pumpgtm@pumpgtm
```

Then run `/mcp` and sign in to PumpGTM. The plugin's manifest, skills and server config live in [github.com/pumpgtm/pumpgtm-mcp](https://github.com/pumpgtm/pumpgtm-mcp).

In Codex:

```
codex plugin marketplace add pumpgtm/pumpgtm-mcp
codex plugin add pumpgtm@pumpgtm
codex mcp login pumpgtm
```

In Gemini CLI:

```
gemini extensions install https://github.com/pumpgtm/pumpgtm-mcp
```

Then run `/mcp auth pumpgtm` and sign in.

## How results come back

- Every tool result is one MCP `content` block of type `text` whose `text` is a JSON string, and the same object in `structuredContent`. Parse the JSON.
- Most results include `next`: `{ tool, arguments, reason, requiresConfirmation, requiresNewRequestId? }`. It is the suggested following call, not an instruction to skip the customer.
- Errors return `isError: true` and `{ status, retryable, changed, message, details }`. `changed: false` means nothing was written. A missing object returns code `resource_not_found`.
- Tools that write take a `requestId`: 8 to 120 characters of letters, digits, `. _ : -`, starting with a letter or digit. It is an idempotency key. Use a fresh one per call; reuse one only to retry the same call.
- Metered workspaces use Energy at the current prices returned by `energy`. `get_workspace` returns `energy` with the monthly allowance, used and remaining, and `find_people` refuses with `energy_exhausted` when a run would exceed it. Workspaces without Energy see no such field. See [Energy](https://pumpgtm.com/docs/api#energy).
- Nothing reaches LinkedIn, email or X until a person approves it. `find_people`, `review_people`, `save_sequence_draft` and `add_leads` never send. Sending starts with `set_sequence_status` `active` on an approved sequence, and each reply is answered only through `decide_reply`.

## Tool permissions

Every tool is classified for AI clients that ask before acting. Read tools never change anything. Write tools change workspace records but contact no one. Sensitive writes contact a third party, act in public, or spend money, so an AI client should show the details and get the user's yes each time. Hints in `tools/list` match this table (`readOnlyHint`, `destructiveHint`, `openWorldHint`).

| Tool | Class | Side effect |
|---|---|---|
| `get_cold_message_examples` | Read | none |
| `get_company` | Read | none |
| `get_lead_profile` | Read | none |
| `get_pipeline` | Read | none |
| `get_workspace` | Read | none |
| `get_x_play_review` | Read | none |
| `list_leads` | Read | none |
| `list_pending_replies` | Read | none |
| `list_sequences` | Read | none |
| `list_signal_events` | Read | none |
| `list_signal_types` | Read | none |
| `search_known_people` | Read | none |
| `add_company_contact` | Write | changes workspace records only; contacts no one |
| `record_customer_signal` | Write | changes workspace records only; contacts no one |
| `remove_leads` | Write | stops future automation for those people; never deletes history |
| `save_company` | Write | changes workspace records only; contacts no one |
| `save_opportunity` | Write | changes workspace records only; contacts no one |
| `add_leads` | Sensitive write | people enter a sequence and a live sequence will contact them |
| `add_to_sequence` | Sensitive write | people enter a sequence and a live sequence will contact them |
| `decide_reply` | Sensitive write | send, invite and booking_link message the prospect at once; opt_out is permanent |
| `energy` | Sensitive write | reading changes nothing; turning on automatic top-up authorizes future card charges |
| `enrich_person` | Sensitive write | spends workspace Energy (paid) |
| `find_people` | Sensitive write | with confirmed=true runs the requested search using existing Energy; discovery does not contact anyone; list mode with a sequence can enroll people |
| `manage_x_play` | Sensitive write | resuming or enabling enrollment can start future X outreach |
| `reach_post_engagers` | Sensitive write | LinkedIn: enrolls the post's engagers into a live sequence; X: collects them for review |
| `reach_universe` | Sensitive write | people enter a sequence or email campaign that can contact them |
| `research_competitor` | Sensitive write | starts paid background research; never contacts anyone |
| `review_people` | Sensitive write | good fits enter the chosen sequence; a live sequence will contact them |
| `review_x_play_candidates` | Sensitive write | good fits enter the Play's X sequence; an active sequence will DM them |
| `save_sequence_draft` | Write | saves an unapproved draft; explicit revision approval is required to make it live |
| `sequence_email_settings` | Write | changes which mailboxes, schedule and daily limit a sequence uses; sends nothing |
| `set_lead_input` | Sensitive write | releases a held message step for that person |
| `set_sequence_status` | Sensitive write | active or approved lets the paced engine contact enrolled people |
| `x_account` | Sensitive write | follows, likes or reposts publicly from the connected X account |

## A complete flow

1. `get_workspace` to read setup state, sequences, unfinished reviews and pending replies.
2. For a clear search request, read `get_workspace view=search_methods`, then call `find_people` with `targeting`, `confirmed: true` and a new `requestId`. Use the requested count, or five for a new search without one. Poll the returned `operationId` alone until complete. A preview or save-only request uses `confirmed: false` and does not authorize discovery.
3. Show every returned person in order and ask which to add. For a new sequence, `save_sequence_draft` saves an unapproved draft. Show its full copy and audience, then call `set_sequence_status` with `status: approved` and `expectedRevision` only after the customer approves that revision.
4. `review_people` with the customer's `decisions` adds their chosen people to the approved sequence; `finish: true` is optional. Do not ask for another yes to repeat an already-approved action.
5. `list_pending_replies`, show the draft to the customer, then `decide_reply` after their decision.

Requested discovery within existing Energy and the agreed scope and budget needs no additional confirmation. Check costs privately, report known usage briefly with results, and mention the remaining balance only when asked or it blocks the work. Ask before guessing an audience, exceeding agreed usage, making a new charge or enabling automatic top-up. Keep discovery answers focused on targeting and results; address legal or safety concerns when concrete context makes them relevant to the requested action or the customer asks.

Reporting: `get_workspace` (`view: "summary"` for the funnel and account pacing, `view: "activity"` for the ledger) and `list_leads` with `stage` or `sinceDays`. The same numbers are available over REST at [/docs/api](https://pumpgtm.com/docs/api).

## Resources and prompts

`resources/list` returns six lead-search guides: `gigacatalyst://lead-search/catalog`, `quickstart`, `fields`, `query-builder`, `patterns` and `results`. Read `fields` before writing `targeting` by hand. One prompt, `find_and_review_leads`, takes `requestedCount` ("5" to "100", default "5").

## Versioning

Tools scheduled for removal say so in their description for at least 90 days before leaving `tools/list`. There is no fixed rate limit on the MCP server today; discovery is paced by each workspace's daily provider budget.

<details>
<summary><b>Tool reference: all 38 tools, arguments and modes</b> (generated from the live <code>tools/list</code>)</summary>

## Tool reference

Generated from the live `tools/list` on 2026-11-02: 38 tools. "Read only" tools never write. Feature-gated groups return a structured error on workspaces without that feature. Every tool also takes an optional `workspaceId`: a Team owner uses it to act on one of their workspaces; omit it for your own.

## Start here

### get_workspace (read only)

Start here for account status. Read-only schemaVersion 2 summary: company, billing access, every saved LinkedIn/X/email/Instagram sender with separate connection, health and execution evidence, all Play types and sequence states, review coverage, period-labelled results, discovery allowance and excluded companies. Each section returns state, data and issues; unknown is not disconnected or zero. Lists are bounded previews with totals. Missing optional channels do not block other channels. Follow scoped attention and returned Accounts/Billing/Company links. view=activity reads the workspace audit ledger; view=x_posts separately reads recent X posts from the provider and may refresh that account's grant.

| Argument | Type | Notes |
|---|---|---|
| `view` | `summary`, `activity`, `x_posts` | default `"summary"`. |
| `sinceDays` | integer | default `7`. range 0 to 365. |
| `account` | string |  |
| `as` | string | view=x_posts only: which connected X account's posts (@handle) when the workspace has several; default the first connected. |
| `actions` | array of `resolve`, `invite`, `invite_withdrawn`, `accepted`, `like`, `message`, `followup`, `reply_detected`, `view_profile`, `endorse`, `comment`, `tool_prepare`, `enrich`, `meeting_invited`, `reply_drafted`, `reply_sent`, `reply_dismissed`, `reply_snoozed`, `sequence_started`, `sequence_stopped`, `meeting_booked`, `meeting_no_show`, `meeting_synced`, `manual_override`, `x_dm`, `x_follow`, `x_like`, `x_follow_back`, `x_unfollow`, `x_repost`, `x_lookup_cached`, `memory_search`, `x_follow_back_scan`, `x_credit_balance`, `x_reply_subscription`, `x_follow_event`, `x_auto_accept`, `instagram_dm`, `instagram_comment`, `instagram_unlock`, `track_unavailable`, `track_done`, `ad_campaign_architected`, `ad_campaign_saved`, `ad_creative_generated`, `ad_budget_revised`, `ad_approval_requested`, `ad_campaign_approved`, `ad_campaign_published_paused`, `linkedin_search`, `web_pages_read`, `email_found`, `email_campaign_created`, `email_campaign_added`, `email_sent`, `email_replied`, `email_auto_reply`, `email_skipped`, `email_bounced`, `email_unsubscribed`, `email_click`, `testimonial_submitted`, `sequence_people_added`, `lead_replenishment_enabled`, `lead_replenishment_disabled`, `recovery_eligible`, `recovery_matched`, `recovery_enrolled`, `recovery_review`, `recovery_invite_sent`, `recovery_invite_accepted`, `recovery_message_sent`, `recovery_reply_detected`, `recovery_suppressed`, `engine_run`, `linkedin_connect`, `error` |  |
| `outcome` | `ok`, `failed`, `skipped` |  |
| `limit` | integer | default `100`. range 1 to 500. |

### energy

Energy balance, costs and top-ups. Read this workspace's Energy (what is left, what each action costs, this month's use, renewal); estimate what planned work will cost against it; and, when asked, get a payment link to buy packs or switch automatic top-up on or off. Use it to check coverage before large discovery or X work, paid profile or email lookups, or standalone recent-X-post reads (lookups add to discovery; cached lookups are free). Checking prices does not require narrating the balance: show it only when asked or it blocks the work. Execute requested work within existing Energy and agreed usage without another confirmation; before exceeding agreed usage, show a concise estimate and ask. Reading is free and works at zero. When Energy runs short, say what the work needs and what is left, and offer the smallest top-up from the response's refill guidance (a pack is $10 for 75 Energy, or 100 per $10 from $50 up); mention a bigger plan only when the customer asks or refill.plan says to. Buying needs the customer to open the link and pay; recheck the balance after payment before resuming. Bring up automatic top-up only when the customer asks how to avoid running out (by default the saved card is charged $10 for 75 Energy whenever the balance falls below 50). Enabling it authorizes future charges to the saved card: explain the amount, threshold and recurring charges and get explicit consent first; a request to continue work or buy one pack is not consent. Team members ask the billing owner, and an allocation limit needs an allocation change.

| Argument | Type | Notes |
|---|---|---|
| `estimate` | object | Counts of planned work: peopleToFind, xAudiencePeople, xDms (with history checks), xDmsUnchecked (send-only, 0.6 Energy each), xFollows, xUnfollows, xLikes, xReposts (cost Energy) and linkedinInvites, linkedinMessages, emails (free, listed as zero). Returns the Energy it will cost, whether the balance covers it, and the packs that would close a shortfall. |
| `buy` | object |  |
| `autoTopUp` | boolean | true authorizes recurring $10 card charges for 75 Energy whenever the balance falls below 50; obtain explicit customer consent to those terms first. false turns it off. |

## Find and review people

### find_people

Find people who match a description: create or revise a Play from targeting and run it. A call with targeting saves a Play even without confirmed=true, so it is not the way to count people. Read get_workspace view=search_methods first for available methods and their query instructions, then choose targeting.searchMethod yourself (auto, connected_account or professional_database; the backend translates the inputs and falls back without changing targeting). Put literal facts in hardFilters and descriptive fit in semanticQuery; never drop targeting to fit a method, name a data vendor, or ask the customer to troubleshoot a provider. A hiring signal (currentCompanyHiringRolesAny, posted within currentCompanyHiringPostedWithinDays) requires an open job posting for one of those roles at the person's current company. A fundraising signal (currentCompanyRaisedWithinDays, optionally currentCompanyRoundTypesAny and currentCompanyMinRaisedUsd) requires a recently closed round; such a Play re-runs once a day so newly funded companies keep arriving for review. When the customer wants a new Play without saying how, offer a job-title search first, then hiring or fundraising signals, then everything else (post engagers, competitor followers or engagers, buying intent, lists). Preparation and confirmed discovery run as saved background operations: the first response returns operationId and status promptly; poll find_people with operationId alone until completed or failed (polling is read-only) and never start a new request to check progress. For an explicit search request with a specified audience, pass confirmed=true with targeting to prepare and search in one operation within existing Energy and the agreed scope and budget. Use the requested candidateCount, or five for a new search without a count. Without confirmed=true the operation only prepares the Play; after completion continue with confirmed=true when discovery was already requested. Ask only if you guessed the audience or would exceed agreed usage. A save-only, preview or cost question does not authorize discovery. Confirmed discovery returns candidates in retrieval order: show the targeting from plan.targetingSummary, then every candidate neutrally in that order with name, current title, company as its own field, LinkedIn URL and match reason; never rank, shortlist or silently drop anyone, and record the customer's add and skip choices with review_people. Company names resolve automatically and never block: an exclusion covers every company sharing the name and a required company includes every strong match. companyResolution reports each company's outcome (matched, matched_several, unmatched); mention matched_several or unmatched in one line. To pin one company, revise the same Play with its playId, expectedRevision, the complete targeting, a new requestId and its website domain in currentCompanyDomainsAny or LinkedIn company URL in currentCompanyLinkedInUrlsAny; never silently drop or substitute a company. Pass playId alone to read an existing Play's complete targeting, and tell the customer in plain words so they can say what to change; add confirmed=true to rerun it. Pass list (YC or a16z speedrun founders) instead of targeting: without confirmed=true it returns the matching count and filter options to show; run it with confirmed=true only after the customer confirms, and with sequenceId it also adds everyone matching to that sequence. Pass play (a type and its inputs) for lookalike companies, named target accounts, a role at one company, competitor post engagers, new-connection alerts or webhook signups: without confirmed=true it returns what would be created; with confirmed=true it creates the Play and starts finding people, who appear on the Play page for review. Pass accounts for a list of named companies. Optional sequenceId sets the Play's Sequence for new leads only; it never moves existing leads or approves messages (use preview_move_leads and move_leads for an explicit transfer). Creates no leads and contacts no one.

| Argument | Type | Notes |
|---|---|---|
| `requestId` | string | required. Stable idempotency key; reuse it only to retry this exact request. |
| `targeting` | object |  |
| `playId` | uuid |  |
| `expectedRevision` | integer | range  to 9007199254740991. Required with targeting when revising an existing playId. |
| `confirmed` | boolean | default `false`; pass `true` for a requested search within existing Energy and the agreed scope and budget. |
| `sequenceId` | uuid |  |

### review_people

Without decisions: read one review batch with its candidates and saved choices. With decisions: record the customer's explicit good_fit and not_a_fit choices; a top-level sequenceId applies to every good fit, finish=true completes the review and queues good fits into the sequence. For add all, mark every candidate good_fit with finish=true. Sends nothing in this call. Open world: people queued into a live sequence are later contacted by its paced steps.

| Argument | Type | Notes |
|---|---|---|
| `batchId` | uuid | required. |
| `requestId` | string | Required with decisions. |
| `sequenceId` | uuid |  |
| `decisions` | array of objects |  |
| `finish` | boolean | default `false`. |

### search_known_people (read only)

Search a company, professional role, or explicitly stated accelerator cohort in permitted shared professional evidence. Returns at most 20 matching identities; coverage is incomplete and includes labelled historical roles and interpretations. This is not a live provider search, does not expose other customers' lists or messages, and does not authorize outreach. Check source dates before recommending someone.

| Argument | Type | Notes |
|---|---|---|
| `query` | string | required. |

### get_lead_profile (read only)

Read source-dated public identities, available employment history, public activity and relationship observations for a lead owned by this workspace. Public profile links and AI interpretations require review. This does not fetch providers, change identities, enroll or contact anyone.

| Argument | Type | Notes |
|---|---|---|
| `leadId` | uuid | required. |

## Sequences

### list_sequences (read only)

Sequences with steps, approval state, status, and exact enrolled-lead counts. Pass sequenceId to read one sequence with its exact revision before editing.

| Argument | Type | Notes |
|---|---|---|
| `sequenceId` | uuid |  |

### save_sequence_draft

Write or edit an outreach sequence (LinkedIn, email and X messages) as a draft that sends nothing. Without sequenceId: create a complete new sequence (name plus ordered steps). With sequenceId and expectedRevision: read the full draft first, then replace its complete step list using that exact revision, preserving next, condition, sendIf and other unchanged fields; an active or paused sequence must be reopened with set_sequence_status status=draft first. The result is always an unapproved draft: show the customer the copy and its reviewUrl, and only after they approve this exact revision call set_sequence_status status=approved with expectedRevision, which makes it live; do that before adding people. Write like a person typing a DM to a colleague: concrete point first, plain words, contractions and natural rhythm. No em dashes, preambles ('Here's the thing'), formal transitions, mirrored contrasts ('not X, but Y'), rhetorical-question chains, slogans, buzzwords, tidy groups of three, or forced hooks, punchlines and takeaways; ask at most one question. Read it aloud and keep only what the sender would really say. Every new sequence and LinkedIn post-engagers Play gets three follow-ups after the first message unless the customer wants fewer: proof (+2 days: a company like the prospect's, one result with a number, one link), a question about how they do that job today or an offer of something free (+3 days), and a short last note that leaves the door open (+5 days), all in the customer's voice from their first message and company. Never invent a customer, number or link: ask for a real result before saving, and use the free offer in place of proof when they have none. LinkedIn connection requests go without a note: never suggest, offer or draft one, because it makes the request read overly specific. Only if the customer insists, put it in the connect step's messageTmpl (at most 300 characters; only {{firstName}} {{fullName}} {{company}} {{title}} {{senderFirstName}}); an account out of notes sends the request without it. Steps include connect, message, engage (LinkedIn), email (subject plus body), x_dm and x_engage; Instagram DMs run only in their own linear sequence. Linear sequences (up to 20 steps) that use LinkedIn start with connect, and linear X sequences cannot mix channels; tracks (20) run one lane per channel; graphs (30) mix LinkedIn, email and X, may start with email or X and branch with next/condition. In a graph, a LinkedIn message or endorsement must follow a connection request and an 'Accepted?' (linkedin_accepted) or 'Already connected?' (linkedin_first_degree) Yes path; view, like and comment need no connection. After any lead leaves graph step 0, only copy and delay edits are accepted. InMail steps (kind inmail) need the linkedin_inmail flag and a graph, take a subject (200 characters) and messageTmpl (2000) and no connection request; LinkedIn subscription and credit limits apply. Graph outreach first checks earlier email, LinkedIn and X conversations: a human reply stops it and unavailable evidence holds it. {{firstName}} {{fullName}} {{company}} {{title}} render across channels. {{ai_intro}} (message and email steps) is a one-sentence opener written once for each person from their role and recent LinkedIn posts (0.1 Energy each, reused in later steps); when it cannot be written it renders its |fallback, or 'Hope your week is going well.', so put it on its own line. A message, email or X DM step may hold a value only the customer can give per person (for example {{video_url}}): declare tool { kind: "customer_input", tokens: [...] } and tell the customer the step waits for each person's value, which set_lead_input stores. 'Write it myself' makes the step's whole text {{your_message}} with tokens ["your_message"]. For each person's own file, add attachment { key: "personal_file" } to that tool (an email step takes one whenever the customer asks, with no warning). On reconnectXForFiles, tell the customer to reconnect that X account on Accounts before files can send; other messages keep sending.

| Argument | Type | Notes |
|---|---|---|
| `requestId` | string | required. Stable idempotency key; reuse it only to retry this exact request. |
| `sequenceId` | uuid |  |
| `expectedRevision` | integer | range  to 9007199254740991. |
| `name` | string |  |
| `steps` | array of objects | required. |
| `executionModel` | `linear`, `tracks` | tracks: LinkedIn, email and X steps run as parallel lanes per person (each lane's delayDays count from its own previous step; a person without one channel's handle still gets the others). Needs the workspace's multichannel_tracks feature. Default linear. |
| `xThreadCheck` | boolean | Workspace-wide X setting (applies to every sequence). true: before each X DM, check the conversation so nobody the customer already talks to is messaged and the sequence stops on a reply (5 Energy per DM). false (the default): messages send without that check, cheaper. Omit to keep the current setting. |

### set_sequence_status

For a LinkedIn sequence (sequenceId): paused stops it for every enrolled lead, active resumes a paused sequence, draft reopens it for editing in the dashboard. status=approved (expectedRevision required) is for a sequence that is still a draft, including every sequence saved with save_sequence_draft: call it only after the customer has approved that exact revision; it records the approval and makes it live in the same call. For an email campaign (emailCampaignId): active launches sending inside its schedule and limits, paused stops it, draft reopens it; approved does not apply. Open world: active or approved lets PumpGTM's paced engine send connection requests, messages and emails to the enrolled people, who are third parties, inside working hours and account limits; it stops for anyone who replies. Approving a sequence and launching an email campaign are separate, explicit customer decisions; get_workspace lists connected mailboxes and a separate, feature-gated legacyEmailCampaigns inventory whose campaigns keep their own controls.

| Argument | Type | Notes |
|---|---|---|
| `sequenceId` | uuid |  |
| `emailCampaignId` | uuid |  |
| `status` | `approved`, `active`, `paused`, `draft` | required. |
| `expectedRevision` | integer | range  to 9007199254740991. |

### sequence_email_settings

Read a sequence's email sending inboxes, schedule and daily limit. Supply settings to replace them using this workspace's cold outreach mailboxes. Does not change sequence status or send immediately; an active sequence can send on its next paced tick. Paused or disconnected inboxes remain paused or disconnected. No Energy charge.

| Argument | Type | Notes |
|---|---|---|
| `sequenceId` | uuid | required. |
| `settings` | object |  |

## Leads

### list_leads (read only)

This workspace's leads with LinkedIn and sequence progress. stage is cumulative (reached this milestone or beyond); stageExact is the current position only. Count is exact even when the page is limited. Pass leadId to read one lead in full.

| Argument | Type | Notes |
|---|---|---|
| `leadId` | uuid |  |
| `stage` | `queued`, `resolving`, `invited`, `accepted`, `messaged`, `followed_up`, `replied`, `failed`, `skipped` |  |
| `stageExact` | `queued`, `resolving`, `invited`, `accepted`, `messaged`, `followed_up`, `replied`, `failed`, `skipped` |  |
| `sequenceId` | uuid |  |
| `strategyId` | uuid |  |
| `query` | string |  |
| `engagedOnly` | boolean |  |
| `bookedOnly` | boolean |  |
| `waitingForInput` | boolean | Only people the engine is holding because a message step needs a value from the customer (see leads_waiting_for_input in get_workspace). Each returned lead lists waitingFor keys and waitingSince. |
| `sinceDays` | integer | range 0 to 365. |
| `limit` | integer | range 1 to 500. |

### add_leads

Queue leads into one approved sequence: contacts accepts 1–500 supplied people with LinkedIn, email, and/or X identities plus expectedRevision. It supports X-only and email-only contacts, preserves supplied fields, performs no paid enrichment, skips existing identities across all channels, and keeps paused sequences paused. Other methods accept manual LinkedIn profiles, CSV content, or a saved Sales Navigator list URL. Manual and CSV finish immediately and return added, duplicate, and invalid counts plus stable addedLeadIds. Sales Navigator runs in the background and auto-enrolls; use method=import_status with its batchId to check. Nothing is sent in this call.

The input is one of these shapes:

- `method: "contacts"` with `requestId` (required), `sequenceId` (required), `expectedRevision` (required), `leads` (required)
- `method: "manual"` with `requestId` (required), `sequenceId` (required), `leads` (required)
- `method: "csv"` with `requestId` (required), `sequenceId` (required), `csvContent` (required), `customColumns`
- `method: "sales_navigator"` with `requestId` (required), `sequenceId` (required), `accountId`, `listUrl` (required)
- `method: "import_status"` with `batchId` (required)

### remove_leads

Only when the customer explicitly asks to undo an add or stop leads. Target lead IDs, one batchId from add_leads, or target=companies with employer names: those go on the workspace do-not-contact list (shown in get_workspace), every Play stops finding people there, no lead at those employers is ever added again, and everyone already queued there is removed. Uncontacted leads leave the queue; contacted leads keep history and get no further automated steps. Sent invitations and messages cannot be reversed.

The input is one of these shapes:

- `target: "leads"` with `leadIds` (required)
- `target: "import"` with `batchId` (required)
- `target: "companies"` with `companies` (required)

### set_lead_input

Store a value the customer supplies for one person, such as a link to a video they recorded, so a message step declared with tool customer_input can send. Identify the person by leadId or their LinkedIn profile URL. values maps the sequence's token names (for example video_url) to the customer's text; the key comes from the sequence, the customer never types it. Nothing is sent in this call: the paced engine sends on its next run if the person is at that step, or when they reach it. A value for someone who already replied, finished, or already received that message is refused with nothing changed. The result includes how many people in that sequence are still waiting. Open world: the stored value releases a held message to that person, a third party.

| Argument | Type | Notes |
|---|---|---|
| `requestId` | string | required. Stable idempotency key; reuse it only to retry this exact request. |
| `leadId` | uuid |  |
| `linkedinUrl` | string |  |
| `values` | object | required. |

## Replies

### list_pending_replies (read only)

Inbound replies frozen for human review. Every item names its channel (linkedin, instagram, x). Items with an id carry PumpGTM's draft answer: decide them with decide_reply by that draftId (allowedDecisions lists what applies; X drafts accept everything but invite and send as a direct message on X). X items with no draft and no id (an encrypted reply PumpGTM could not read, or an older reply) are decided with decide_reply using their leadId, or answered in X at replyUrl. An X item with theirMessage null and messageUnavailable x_chat_passcode_required (or x_chat_passcode_changed) is an encrypted X Chat reply PumpGTM could not read: say so plainly, never guess or invent what they wrote, and tell the customer they can unlock it from their Inbox (unlockUrl) by entering their X Chat passcode once (x_chat_passcode_changed: the passcode changed in X and must be entered again). Never ask the customer for the passcode in this chat. Read-only.

| Argument | Type | Notes |
|---|---|---|
| `limit` | integer | default `50`. range 1 to 200. |

### decide_reply

Apply one explicit human decision to a pending reply. send, invite, and booking_link contact the prospect; opt_out is permanent. Show the draft and ask the customer first. Pass the item's draftId when it has one (LinkedIn, Instagram and X drafts alike; an X draft's send goes out immediately as a direct message on X from the account that owns the conversation, uses Energy, and does not accept invite). Only for an X reply with no draft pass leadId instead: send then requires text the customer approved, shown to them word for word first, and X accepts only send, snooze, dismiss, opt_out and meeting_booked. Sending the same text again for the same reply returns alreadyDecided instead of a second message.

| Argument | Type | Notes |
|---|---|---|
| `draftId` | uuid | The pending reply's id, on any channel, when the item has one. |
| `leadId` | uuid | X replies without a draft only: the item's leadId, used when there is no draftId. |
| `decision` | `send`, `invite`, `booking_link`, `snooze`, `start_sequence`, `meeting_booked`, `dismiss`, `opt_out` | required. |
| `text` | string |  |
| `days` | integer | range 1 to 365. |
| `decidedBy` | string | default `"mcp"`. |

## Cold messages

### get_cold_message_examples (read only)

A random sample from PumpGTM's library of the most-liked public posts on X about writing cold emails and DMs, each with its link and why it was kept. Use them as references when drafting or critiquing a cold message; credit the author and do not copy them word for word. Read-only.

| Argument | Type | Notes |
|---|---|---|
| `count` | integer | default `5`. range 1 to 10. |

## Analytics, threads and undo

### get_analytics (read only)

Read the same outreach results as /analytics: totals, daily activity, Plays and their Sequences, message-step performance, channels and highest/lowest eligible results. Defaults to range=30d and scope=workspace. Team scope requires a personal OAuth or personal delegation key belonging to a current Team owner/admin, used from its own home workspace; shared workspace and operator keys cannot read team scope. Enrolled is all-time current Sequence assignment; other activity uses the selected Pacific-time window. Reply rates use a full 14-day observation window and rankings require 30 eligible people per result. Null rankings mean insufficient or tied results. Clicks are unavailable; meetings have partial coverage. Cached report timestamps are returned explicitly. No message text, lead records, paid lookups or outreach actions.

| Argument | Type | Notes |
|---|---|---|
| `range` | `7d`, `30d`, `90d` |  |
| `scope` | `workspace`, `team` |  |

### get_lead_thread (read only)

Read incoming and outgoing LinkedIn messages using the lead's original sender. No sends, reply decisions or read receipts. Pass leadId from list_leads and optionally limit (1–100, default 50). Each page is oldest first; nextCursor retrieves the next older page, so prepend it to the conversation. Reuse cursors only for the same lead and workspace within 30 minutes. available=false means the lead has no saved LinkedIn thread yet; provider failures are errors, never empty history. Text and attachment presence only; no attachment downloads. LinkedIn only for now.

| Argument | Type | Notes |
|---|---|---|
| `channel` | string |  |
| `limit` | integer |  |
| `cursor` | string |  |
| `leadId` | uuid | required. |

### undo_remove_leads

Only when the customer explicitly asks to undo a removal. Pass exact removalIds from remove_leads or list_leads removal.id. Restores the same lead in its original sequence with its progress; paused sequences stay paused. No copy is created and this call sends nothing. Replies, opt-outs, changed sequences, overlapping outreach and unconfirmed sends can block restoration; explain each returned reason. Retrying the same removal IDs is safe and cannot undo a later removal. Company exclusions and cancelled imports are not reversed. Open world: restored outreach may later contact these people through the ordinary paced engine.

| Argument | Type | Notes |
|---|---|---|
| `removalIds` | array | required. Exact removal IDs from remove_leads or the lead's removal field. Reuse these IDs for retries. |

### get_webhook_play_setup (read only)

Read setup instructions for HubSpot form signups or a generic Webhook Play. Omit playId for creation links; pass it for the setup URL, relevant OAuth or webhook instructions, test steps, troubleshooting and up to five saved contacts. Explain one step at a time in plain language. Returns OAuth setup guidance for HubSpot, without claiming an existing connection or configuring HubSpot automatically. Never returns credentials, creates a Play, activates a sequence, enrolls or sends. Use this for connecting HubSpot/forms, testing signup intake, or diagnosing webhook setup.

| Argument | Type | Notes |
|---|---|---|
| `playId` | uuid |  |

## People data (uses Energy)

Full guide with example calls and pricing: [People data MCP](https://pumpgtm.com/docs/people-mcp). The same operation is available over REST at [/docs/people-api](https://pumpgtm.com/docs/people-api).

### enrich_person

Look up one person by email or by public LinkedIn person URL (send exactly one; an email is matched to a LinkedIn profile first). Returns person: name, title, headline, location, company {name, domain}, linkedin, and connections (previously observed public follows) when there are any; person is null when nobody matches. Add find: ["x"] to get their X account, returned only when its name matches and its own bio or link points at their LinkedIn, company domain or company name (x.evidence says which). Add find: ["work_email"] for a work email; check workEmail.matchesCompanyDomain before using it. A requested extra that did not come back is listed in missing with a reason (not_found means no confident match, not that it does not exist). Uses workspace Energy; 1 Energy is $0.05; energyUsed is what this call spent. Read energy.peopleDataPricing first; cached lookups are free. Requires Energy enabled. Does not create leads or send outreach. Repeated calls can cost Energy; never automatically retry a timeout or error. Use find_people for audience discovery, not this tool.

| Argument | Type | Notes |
|---|---|---|
| `email` | email |  |
| `linkedin` | string |  |
| `find` | array of `x`, `work_email` | default `[]`. |

## Signals

Full guide: [Signals over MCP](https://pumpgtm.com/docs/signals-mcp). The same events are available over REST at [/docs/signals-api](https://pumpgtm.com/docs/signals-api).

### list_signal_types (read only)

Discover the Signals API catalog. Only available types are callable. Coming-soon entries are roadmap categories, not event names or working subscriptions.

### list_signal_events (read only)

Read source-linked hiring observations from PumpGTM's shared index. Default start=recent returns up to 30 days of observations; start=now returns a cursor for future observations. Save and reuse the returned cursor with the same filters to continue. Deduplicate downstream actions by eventId. No Energy charge or paid search; collection is demand-driven, not a scheduled company watch. Does not create leads or contact anyone.

| Argument | Type | Notes |
|---|---|---|
| `filters` | object | default `{"roles":[],"companyDomains":[]}`. |
| `cursor` | string or null | default `null`. |
| `start` | `now`, `recent` | default `"recent"`. |
| `limit` | integer | default `50`. range 1 to 100. |
| `name` | `"hiring.job_observed"` | required. |

## Post engagers and X (X tools need the X channel)

### reach_post_engagers

LinkedIn profileUrl plus message explicitly follows that profile: check the latest 10 posts from the past 30 days every six hours and automatically enroll up to 500 new engagers per collection. Only use profileUrl when the customer requests recurring outreach. LinkedIn post URL plus message: PumpGTM creates a Play with its own approved sequence (note-less connection request, then your message), fetches every reactor and commenter in the background (up to 500), and enrolls all of them; call again with only playId for progress. X post URL (x.com/…/status/…) plus message, or plus an existing X sequenceId: creates a real X Play, its audience rules and collection source, and returns its Play link. X defaults to review before enrollment; select engagements when the customer wants only likes or replies. The customer adds selected people to the X sequence (or with add_to_sequence). followers=true instead of a post URL creates a new-followers X Play. A supplied Sequence keeps its status; a message creates an approved active Sequence. Existing shared sources retain their status, Sequence and collection progress. Report polling blockers and paused Sequences exactly as returned; do not promise polling or sending while blocked. Follower Plays can still receive incoming X follow events while fallback polling is blocked. Use manage_x_play to inspect/edit the returned X Play, including enabling autopilot only when the customer requests automatic enrollment. watchId alone checks source/Sequence progress. The X call itself never enrolls or sends.

| Argument | Type | Notes |
|---|---|---|
| `postUrl` | uri |  |
| `profileUrl` | uri | LinkedIn only: explicitly follow this personal profile's latest posts and automatically enroll new reactors and commenters every six hours. Use with message instead of postUrl. |
| `followers` | boolean | X only: collect the account's new followers for review instead of a post's engagers. |
| `engagements` | array of `liked`, `replied` | X posts only: liked for likes, replied for replies, both when the customer requests all engagers. Defaults to both. |
| `message` | string |  |
| `sequenceId` | uuid | X posts only: an existing X DM sequence instead of a new one from message. |
| `name` | string |  |
| `playId` | uuid |  |
| `watchId` | uuid |  |
| `as` | string | X only: which connected account's post or followers (@handle) when the workspace has several; default the first connected. |

### x_account

One action from a connected X account: follow or unfollow a profile (target is an X profile URL or @handle), or like or repost one post (target is an X post URL). `as` picks which connected account acts when the workspace has several (get_workspace view=x_posts lists them); default is the first connected. Needs the X channel. Unfollow only when requested by the customer. It never sends a DM or writes a new post.

| Argument | Type | Notes |
|---|---|---|
| `action` | `follow`, `unfollow`, `like`, `repost` | required. |
| `target` | string | required. |
| `as` | string |  |

### manage_x_play

List X Plays, get one Play's current revision and audience, create a Buying intent Play, or update a Play's name, audience rules, sequence, or running/paused state. Each Play's audience is described in plain words (audience.sources); show those to the customer, never the plan JSON. create makes a Buying intent Play from one signal: competitor_followers (people following several competitor X accounts), competitor_post_engagers (people replying to, quoting or reposting competitors' recent posts), asks (people asking X and/or LinkedIn for a tool like the customer's), complaints (people unhappy with a competitor on X and/or LinkedIn), or reddit (threads asking for what the customer sells, with a reply drafted for review). When the customer has not named competitors, suggest a few from the company profile and confirm before creating. Phrases are short buyer words ("best bank for a startup", "account frozen"), never the company's own marketing terms. criteria says who fits in one sentence; found people are checked against it. A Buying intent Play collects before X is connected and lists people for review; it never enrolls or messages anyone by itself. sequenceId (an X-lane or LinkedIn-lane sequence) or message (creates an approved active X DM sequence named after the Play) is optional. Discover a Play first; update requires expectedRevision. Unspecified fields stay unchanged. The sender stays fixed. For Plays on the account's own posts or followers, reach_post_engagers sets up a new source. Pausing a Play stops future audience collection, not messages already queued in its sequence; use set_sequence_status to stop those too when requested. Enabling automatic enrollment or resuming may start future outreach through the paced engine. Never connects accounts or sends directly.

| Argument | Type | Notes |
|---|---|---|
| `action` | `list`, `get`, `create`, `update` | required. Required: list to discover Plays; get to read one; create for a new Buying intent Play; update to apply an explicit change. |
| `playId` | uuid | Required for get and update. Discover with action=list. |
| `expectedRevision` | integer | range  to 9007199254740991. Required for update. Copy the revision returned by list or get. |
| `name` | string | Required for create, e.g. "Founders who follow our competitors". |
| `status` | `active`, `paused` |  |
| `plan` | object |  |
| `signal` | `competitor_followers`, `competitor_post_engagers`, `asks`, `complaints`, `reddit` | Required for create. competitor_followers: people following several competitor X accounts (competitors, minShared). competitor_post_engagers: people replying to, quoting or reposting the competitors' recent X posts (competitors, postsPerCompetitor). asks: people asking X and/or LinkedIn for a tool like the customer's (phrases, networks). complaints: people unhappy with a competitor on X and/or LinkedIn (phrases, competitors for X, competitorNames for LinkedIn). reddit: Reddit threads asking for what the customer sells, with a reply drafted for review (phrases, subreddits). |
| `competitors` | array of strings | Competitor X accounts as @handle, handle or x.com profile link. 2–10 for competitor_followers, 1–10 for competitor_post_engagers, optional for complaints (anchors the X search). |
| `minShared` | integer | range 2 to 10. competitor_followers: how many of the competitors a person must follow. Default 3, never more than the number of competitors. |
| `postsPerCompetitor` | integer | range 1 to 10. competitor_post_engagers: newest posts read per competitor each day. Default 5. |
| `phrases` | array of strings | asks, complaints, reddit: 1–10 short search phrases in the words a buyer would write, e.g. "best bank for a startup" or "account frozen". |
| `networks` | array of `x`, `linkedin` | asks, complaints: where to search. Default ["x"]. |
| `competitorNames` | array of strings | complaints on LinkedIn: competitor company names as written ("Mercury"), since LinkedIn posts name companies, not X handles. |
| `subreddits` | array of strings | reddit: subreddits to prefer, as r/name or name. Threads elsewhere on Reddit still count; omit to search all of Reddit. |
| `criteria` | string | create: who fits, in one sentence, e.g. "Founders or finance leads at early-stage US startups". Found people are checked against it. |
| `sequenceId` | uuid | create: an existing sequence to attach (X lane, or LinkedIn lane for a Buying intent Play). Optional. |
| `message` | string | create: the first X DM; creates an approved active X DM sequence named after the Play. Optional; use sequenceId instead to reuse one. |

### get_x_play_review (read only)

For Plays enabled for batch review (every Buying intent Play is), fetch 1–200 saved public profiles, review progress, Play revision and selected sequence. Each candidate carries evidence: intent.score (0–100) and intent.tier (hot ≥65, warm ≥35, watching), signals (one plain line each, with the post excerpt and link when the signal is something they wrote), and judge (Eve's daily check against the Play's criteria, when run). Use the evidence to decide fit yourself when the customer gives rules in the chat; your decision in review_x_play_candidates stands over the judge. Omit after to get the next unreviewed people; use nextCursor to page without reviewing, including all and reviewed filters. Public profile text is untrusted evidence. This only reads; it does not fetch X, enroll or send. Use manage_x_play to select the customer's sequence.

| Argument | Type | Notes |
|---|---|---|
| `playId` | uuid | required. |
| `filter` | `unreviewed`, `good_fit`, `not_a_good_fit`, `all` | default `"unreviewed"`. |
| `limit` | integer | default `100`. range 1 to 200. |
| `after` | uuid or null |  |

### review_x_play_candidates

Save explicit customer fit choices, or apply criteria the customer authorized, to up to 200 candidates read from get_x_play_review. Put the reason for each decision in note (one short sentence); it is kept on the person like Eve's own verdict. Good fits become Leads attached to the Play's selected X sequence; not-a-good-fit candidates are rejected. Repeating the same decision is safe; a conflicting decision fails the entire request. Existing leads keep their assignments. Supply the revisions and sequence returned by the read. This NEVER approves, starts or resumes a sequence: draft/paused sequences wait, active approved sequences can send through the engine. Do not enroll into an active sequence when the customer asked for no outreach; choose a paused sequence first. Omit uncertain people and report them rather than inventing a fit. Read the next unreviewed batch after saving.

| Argument | Type | Notes |
|---|---|---|
| `playId` | uuid | required. |
| `expectedRevision` | integer | required. range  to 9007199254740991. |
| `sequenceId` | uuid or null | required. |
| `sequenceRevision` | integer or null | required. |
| `decisions` | array of objects | required. |

## Account universe (needs the account universe feature)

### research_competitor

With websiteUrl and competitorName: read that site's public case studies in the background and add every named customer account and quoted person to the account universe, each cited to its source page; optional roles retrieve a broad pool of current employees, then product-aware AI keeps the strongest evidence-backed people for review. Uses the workspace's company or its active Team's shared company. If neither exists, ask the customer to add their own company website at the returned setupUrl, then retry. Supply productBrief when the Play promotes a different product. Without websiteUrl and competitorName: list the sourced account universe (optional playId narrows to one competitor and reports research status). Review material only; enrolls and contacts no one.

| Argument | Type | Notes |
|---|---|---|
| `websiteUrl` | uri |  |
| `competitorName` | string |  |
| `roles` | array of strings |  |
| `productBrief` | string |  |
| `maxCaseStudies` | integer | range 1 to 80. |
| `maxPeopleTotal` | integer | default `100`. range 1 to 500. |
| `playId` | uuid |  |
| `limit` | integer | default `200`. range 1 to 500. |

### reach_universe

Pick people from the account universe (by competitor name, playId, or exact candidateIds; notYetEnrolled skips anyone already in LinkedIn outreach) and reach them on one channel. channel=email puts everyone with a known email into one of the workspace's own email campaigns, either campaignId or a new draft named campaignName, and may set the campaign's steps (plain text, {{firstName}} {{company}} {{title}} placeholders, delayDays between steps), schedule, sending mailboxIds (or allMailboxes), and dailyLimit in the same call; people without an email are skipped, and findEmails=true first looks up missing addresses, up to maxEmailLookups and a daily limit. channel=linkedin enrolls them into an active approved sequence by sequenceId through the ordinary paced engine. Nothing is launched or sent in this call.

| Argument | Type | Notes |
|---|---|---|
| `channel` | `email`, `linkedin` | required. |
| `competitor` | string |  |
| `playId` | uuid |  |
| `candidateIds` | array of uuids |  |
| `notYetEnrolled` | boolean | default `true`. |
| `campaignId` | string |  |
| `campaignName` | string |  |
| `findEmails` | boolean | default `false`. |
| `maxEmailLookups` | integer | default `50`. range 1 to 200. |
| `steps` | array of objects |  |
| `schedule` | object |  |
| `mailboxIds` | array of uuids |  |
| `allMailboxes` | boolean | default `false`. |
| `dailyLimit` | integer | range 1 to 5000. |
| `sequenceId` | uuid |  |

### add_to_sequence

Pick people from the account universe (by competitor name, playId, or exact candidateIds; notYetEnrolled skips anyone already in a sequence) and add them to one sequence: an existing non-archived sequenceId from list_sequences, or a new draft named name built from template linkedin | email | linkedin_email (then edit its steps with save_sequence_draft). Email-only sequences skip people without a known email; mixed sequences keep them and their email steps wait until an address is known; findEmails=true first looks up missing addresses, metered. Adding never approves, launches, or sends: the customer approves and starts the sequence with set_sequence_status.

| Argument | Type | Notes |
|---|---|---|
| `competitor` | string |  |
| `playId` | uuid |  |
| `candidateIds` | array of uuids |  |
| `notYetEnrolled` | boolean | default `true`. |
| `sequenceId` | uuid |  |
| `name` | string |  |
| `template` | `linkedin`, `email`, `x`, `linkedin_email`, `linkedin_email_parallel`, `all_channels` |  |
| `findEmails` | boolean | default `false`. |
| `maxEmailLookups` | integer | default `50`. range 1 to 200. |

## CRM (needs the CRM feature)

### get_pipeline (read only)

Read your workspace's CRM opportunities and company lifecycle. Money is in minor units and totals are separated by currency. Probabilities are explicit stage/team estimates, not calibrated predictions or verified revenue. Requires the CRM feature.

| Argument | Type | Notes |
|---|---|---|
| `q` | string |  |
| `currency` | string |  |
| `attention` | boolean |  |
| `scope` | `active`, `archived`, `all` | default `"active"`. Active excludes archived companies; archived and all preserve historical access. |
| `offset` | integer | default `0`. range 0 to 10000. |
| `limit` | integer | default `50`. range 1 to 100. |

### get_company (read only)

Company, linked lead contacts, opportunities, success plan, and latest 100 source-attributed events. Recorded/imported signals do not imply a live provider connection.

| Argument | Type | Notes |
|---|---|---|
| `company_id` | uuid | required. |

### save_company

Create or revise a CRM company and its customer success plan. Use a new UUID and expected_revision=0 to create; use the current revision to edit. Lifecycle is your team's assessment; active never means Stripe-verified. Do not invent customer status. Does not send, enroll, or bill anyone.

| Argument | Type | Notes |
|---|---|---|
| `id` | uuid | required. |
| `expected_revision` | integer | required. range 0 to 9007199254740991. |
| `name` | string | required. |
| `domain` | string or null | required. |
| `lifecycle` | `prospect`, `onboarding`, `active`, `at_risk`, `churned` | required. |
| `owner` | string or null | required. |
| `success_plan` | string or null | required. |
| `next_review_on` | date or null | required. |

### save_opportunity

Create or revise a deal for an existing CRM company. Ask for missing value/currency; null value means unknown. Use a new UUID/revision=0 to create, otherwise current revision. Stage does not change outreach or company lifecycle. Won means recorded won, not paid. Only set probability_override when the user supplies an estimate.

| Argument | Type | Notes |
|---|---|---|
| `id` | uuid | required. |
| `expected_revision` | integer | required. range 0 to 9007199254740991. |
| `company_id` | uuid | required. |
| `title` | string | required. |
| `stage` | `new`, `qualified`, `meeting`, `proposal`, `negotiation`, `won`, `lost` | required. |
| `amount_minor` | integer or null | required. |
| `currency` | `USD`, `EUR`, `GBP`, `CAD`, `AUD`, `INR` | required. |
| `probability_override` | integer or null | required. |
| `expected_close_on` | date or null | required. |
| `next_step` | string or null | required. |
| `next_step_due_on` | date or null | required. |

### add_company_contact

Add a company contact, optionally linked to an existing PumpGTM lead in this workspace. Never guess a lead match from a company name. Does not create or enroll outreach leads.

| Argument | Type | Notes |
|---|---|---|
| `id` | uuid | required. |
| `company_id` | uuid | required. |
| `name` | string | required. |
| `email` | email or null | required. |
| `role` | string or null | required. |
| `lead_id` | uuid or null | required. |

### record_customer_signal

Append a dated note, meeting, reply, product engagement, risk, or milestone, with its source link. Use evidence, not assumptions. Source names are attribution, not provider verification. Does not read a provider or send to Slack. A note never refreshes contact recency.

| Argument | Type | Notes |
|---|---|---|
| `id` | uuid | required. |
| `company_id` | uuid | required. |
| `kind` | `note`, `meeting`, `reply`, `product_engagement`, `risk`, `milestone` | required. |
| `source` | `manual`, `eve`, `notion`, `fathom`, `granola`, `calendar`, `slack`, `stripe` | required. |
| `title` | string | required. |
| `body` | string or null | required. |
| `occurred_at` | date-time | required. |
| `source_url` | uri or null | required. |

</details>

## Not on this server yet

- Managing several client workspaces from one connection. Today one key is one workspace. The agency layer is in progress; write to hello@pumpgtm.com.
- Webhooks are managed over REST, not MCP: `POST /api/v1/webhooks` with the same key, see [/docs/api](https://pumpgtm.com/docs/api).

Questions: hello@pumpgtm.com.


## Guides and reading

| If you want to | Read |
|---|---|
| Understand what this server automates | [What is an AI SDR? A plain explanation for founders](https://pumpgtm.com/blog/what-is-an-ai-sdr) |
| Decide your first sales hire | [Your first sales hire: a human SDR, or an AI SDR?](https://pumpgtm.com/blog/first-sales-hire-ai-sdr-vs-human-sdr) |
| Understand the closed loop behind the tools | [AI GTM engine: the system we run for YC startups](https://pumpgtm.com/blog/ai-gtm-engine-closed-loop-system) |
| Book meetings from LinkedIn at scale | [LinkedIn outbound playbook: 30+ enterprise meetings a week](https://pumpgtm.com/blog/linkedin-outbound-playbook-enterprise-meetings) |
| Write the first message | [A cold message that gets replies: 3 rules from 10+ YC startups](https://pumpgtm.com/blog/cold-message-that-gets-replies-three-rules) |
| Compare MCP-capable outreach tools | [LinkedIn outreach tools with an MCP server (2026)](https://pumpgtm.com/best/linkedin-outreach-tools-with-mcp-server) |
| Score leads before you reach out | [LinkedIn lead scoring: filter leads before outreach](https://pumpgtm.com/blog/linkedin-lead-scoring-before-outreach) |
| See a customer result | [How PumpGTM helped Supermemory find customers](https://pumpgtm.com/blog/how-pumpgtm-helped-supermemory-find-customers) |
| Give many users or clients their own workspace | [PumpGTM for platforms and agencies](https://pumpgtm.com/mcp/platforms) |
| Hand an agent everything at once | [pumpgtm.com/llms.txt](https://pumpgtm.com/llms.txt) |

---

<p align="center">
<a href="https://pumpgtm.com">pumpgtm.com</a> · <a href="https://pumpgtm.com/docs">Docs</a> · <a href="https://app.pumpgtm.com/onboarding">Start a workspace</a> · <a href="https://x.com/pumpgtm">X</a> · <a href="https://www.youtube.com/@pumpgtm">YouTube</a> · <a href="https://pumpgtm.com/privacy-policy">Privacy policy</a> · Support: <a href="mailto:hello@pumpgtm.com">hello@pumpgtm.com</a><br>
<sub>Built by <a href="https://pumpgtm.com/about">Giga Next Inc.</a>, San Francisco. Documentation and schemas in this repository are MIT licensed; the hosted service is a commercial product.</sub>
</p>
