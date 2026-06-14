# Claude Practice Exam — 60 Questions

A practice exam covering the Claude (Anthropic) Developer Platform: models, the
Messages API, thinking & effort, tool use, agents, prompt caching, structured
outputs, and operations. Multiple choice. Answer key with explanations at the end.

Generated 2026-06-14. Accurate to the Anthropic platform as of the knowledge used to build it.

---

## Section 1 — Models, Pricing & Context (Q1–Q10)

**Q1.** Which model is Anthropic's most capable *widely released* model?
- A. Claude Opus 4.8
- B. Claude Fable 5
- C. Claude Sonnet 4.6
- D. Claude Mythos Preview

**Q2.** What is the exact model ID string for Claude Opus 4.8?
- A. `claude-opus-4-8-20251114`
- B. `claude-4-8-opus`
- C. `claude-opus-4-8`
- D. `claude-opus-4.8`

**Q3.** What is the context window of Claude Opus 4.8?
- A. 200K
- B. 500K
- C. 1M
- D. 128K

**Q4.** What is the input/output price per million tokens for Claude Opus 4.8?
- A. $3 / $15
- B. $5 / $25
- C. $10 / $50
- D. $1 / $5

**Q5.** Claude Haiku 4.5's context window is:
- A. 1M
- B. 200K
- C. 100K
- D. 64K

**Q6.** A user asks for "the balanced model." Which ID do you use?
- A. `claude-haiku-4-5`
- B. `claude-opus-4-8`
- C. `claude-sonnet-4-6`
- D. `claude-fable-5`

**Q7.** Which model is available *only* through Project Glasswing?
- A. Claude Mythos 5
- B. Claude Fable 5
- C. Claude Opus 4.7
- D. Claude Haiku 4.5

**Q8.** For live capability data (context window, vision/thinking support) you should:
- A. Hard-code values from documentation
- B. Query the Models API (`client.models.retrieve` / `.list`)
- C. Use `tiktoken`
- D. Guess based on the model name

**Q9.** When counting tokens for a Claude prompt you should use:
- A. `tiktoken`
- B. `gpt-tokenizer`
- C. The `count_tokens` endpoint (`POST /v1/messages/count_tokens`)
- D. A fixed 4-chars-per-token estimate

**Q10.** Which statement about Claude Fable 5's tokenizer is correct?
- A. It is unique and unrelated to other models
- B. It is the same tokenizer as Opus 4.8 (introduced with Opus 4.7)
- C. It is the OpenAI tokenizer
- D. It uses half as many tokens as Opus 4.6

---

## Section 2 — Thinking & Effort (Q11–Q18)

**Q11.** On Opus 4.8, how do you enable extended reasoning?
- A. `thinking: {type: "enabled", budget_tokens: 8000}`
- B. `thinking: {type: "adaptive"}`
- C. `temperature: 0`
- D. `reasoning: true`

**Q12.** Sending `thinking: {type: "enabled", budget_tokens: N}` on Opus 4.8 results in:
- A. It works normally
- B. A 400 error (`budget_tokens` removed)
- C. A silent fallback to adaptive
- D. A 529 error

**Q13.** Where does the `effort` parameter go?
- A. Top-level on the request
- B. Inside `thinking`
- C. Inside `output_config`
- D. Inside `metadata`

**Q14.** Which effort level was added in Opus 4.7, between `high` and `max`?
- A. `ultra`
- B. `xhigh`
- C. `extreme`
- D. `very_high`

**Q15.** On Claude Fable 5, sending an explicit `thinking: {type: "disabled"}`:
- A. Works fine
- B. Returns a 400 — omit the `thinking` param entirely instead
- C. Enables adaptive thinking
- D. Is required

**Q16.** On Opus 4.8/4.7 and Fable 5, the default `thinking.display` value is:
- A. `summarized`
- B. `full`
- C. `omitted`
- D. `verbose`

**Q17.** To show users a readable summary of the model's reasoning, set:
- A. `thinking: {type: "adaptive", display: "summarized"}`
- B. `verbose: true`
- C. `thinking: {show: true}`
- D. `output_config: {reasoning: "visible"}`

**Q18.** Which sampling parameters are *removed* (400 on Fable 5 / Opus 4.8 / 4.7)?
- A. `max_tokens`
- B. `temperature`, `top_p`, `top_k`
- C. `stop_sequences`
- D. `system`

---

## Section 3 — Tool Use & Agents (Q19–Q30)

**Q19.** A tool definition requires which three things?
- A. name, description, input_schema
- B. name, version, handler
- C. id, type, output
- D. name, model, prompt

**Q20.** When Claude wants to call a tool, `stop_reason` is:
- A. `end_turn`
- B. `tool_use`
- C. `pause_turn`
- D. `refusal`

**Q21.** A `tool_result` block must include:
- A. The tool's source code
- B. The matching `tool_use_id`
- C. A new model ID
- D. A `cache_control` marker

**Q22.** `tool_choice` of `{"type": "any"}` means:
- A. Claude cannot use tools
- B. Claude must use at least one tool
- C. Claude must use a specific named tool
- D. Claude decides (default)

**Q23.** To force at most one tool call per response, add:
- A. `"max_tools": 1`
- B. `"disable_parallel_tool_use": true`
- C. `"single": true`
- D. `"parallel": false`

**Q24.** The code execution server-side tool runs:
- A. On your client machine
- B. In an Anthropic-hosted sandboxed container (no internet)
- C. In the user's browser
- D. On AWS Lambda you provision

**Q25.** When a server-side tool loop hits its default 10-iteration limit, `stop_reason` is:
- A. `max_tokens`
- B. `pause_turn`
- C. `tool_limit`
- D. `end_turn`

**Q26.** To resume after `pause_turn`, you should:
- A. Add a user message saying "Continue."
- B. Re-send the user message + assistant response; the server resumes automatically
- C. Start a new conversation
- D. Switch models

**Q27.** Per the agent-design guidance, you promote a bash action to a *dedicated tool* mainly to:
- A. Make it faster
- B. Gate, render, audit, or parallelize the action
- C. Reduce token cost
- D. Avoid writing code

**Q28.** Programmatic tool calling (PTC) reduces token cost because:
- A. It compresses the prompt
- B. Intermediate tool results return to the running script, not Claude's context
- C. It disables thinking
- D. It uses a cheaper model

**Q29.** The Python tool runner decorator is:
- A. `@tool`
- B. `@beta_tool`
- C. `@anthropic_tool`
- D. `@register`

**Q30.** Structured outputs constrain responses via:
- A. `output_format` (top-level, current)
- B. `output_config: {format: {...}}` (the deprecated `output_format` is replaced)
- C. `response_schema`
- D. `format_config`

---

## Section 4 — Prompt Caching (Q31–Q38)

**Q31.** The core invariant of prompt caching is:
- A. Caching is random
- B. It's a prefix match — any byte change in the prefix invalidates everything after it
- C. Only the system prompt is cached
- D. Caching ignores message order

**Q32.** The render order that determines the cache prefix is:
- A. `messages` → `system` → `tools`
- B. `tools` → `system` → `messages`
- C. `system` → `messages` → `tools`
- D. alphabetical

**Q33.** Maximum number of `cache_control` breakpoints per request:
- A. 1
- B. 2
- C. 4
- D. unlimited

**Q34.** A silent cache invalidator is:
- A. A frozen system prompt
- B. `datetime.now()` interpolated into the system prompt
- C. Deterministic tool ordering
- D. A stable model ID

**Q35.** To confirm a cache hit, check which usage field?
- A. `input_tokens`
- B. `cache_read_input_tokens`
- C. `output_tokens`
- D. `total_tokens`

**Q36.** Cache *write* cost for the default 5-minute TTL is approximately:
- A. 0.1× base input
- B. 1.25× base input
- C. 2× base input
- D. 10× base input

**Q37.** Changing the model mid-conversation:
- A. Keeps the cache valid
- B. Invalidates the entire cache (caches are model-scoped)
- C. Only invalidates messages
- D. Has no effect

**Q38.** The recommended way to inject a mid-conversation operator instruction without breaking the cached prefix is:
- A. Edit the top-level `system` prompt
- B. Append a `{"role": "system", ...}` message to `messages` (beta, supported models)
- C. Prepend it to `messages[0]`
- D. Add it to `metadata`

---

## Section 5 — API Operations & Errors (Q39–Q48)

**Q39.** HTTP 429 from the API means:
- A. Invalid API key
- B. Rate limited (retryable)
- C. Model not found
- D. Request too large

**Q40.** HTTP 401 typically means:
- A. Rate limited
- B. Invalid or missing API key
- C. Overloaded
- D. Bad model ID

**Q41.** Which error is NOT automatically retried by the SDK?
- A. 429
- B. 529
- C. 400 (invalid_request_error)
- D. 500

**Q42.** A 404 `not_found_error` is most commonly caused by:
- A. A typo in the model ID
- B. Wrong password
- C. Too many tokens
- D. A network blip

**Q43.** The Batches API processes requests at what discount?
- A. 25%
- B. 50%
- C. 90%
- D. 10%

**Q44.** Files uploaded via the Files API are referenced in messages by:
- A. `file_path`
- B. `file_id`
- C. URL only
- D. base64 only

**Q45.** For `max_tokens` larger than ~16K, you should:
- A. Always use non-streaming
- B. Stream the request (avoids SDK HTTP timeouts)
- C. Split into 16K chunks manually
- D. Switch to Haiku

**Q46.** The first message in a `messages` array must have role:
- A. `assistant`
- B. `system`
- C. `user`
- D. any

**Q47.** Which `stop_reason` indicates Claude declined for safety reasons?
- A. `end_turn`
- B. `stop_sequence`
- C. `refusal`
- D. `max_tokens`

**Q48.** On Claude Fable 5, a safety classifier decline returns:
- A. HTTP 400
- B. HTTP 200 with `stop_reason: "refusal"`
- C. HTTP 403
- D. An empty 204

---

## Section 6 — Managed Agents (Q49–Q56)

**Q49.** The mandatory Managed Agents flow is:
- A. Session first, then Agent
- B. Agent (created once) → Session (every run)
- C. Environment only
- D. Vault → Agent → Session every time

**Q50.** `model`, `system`, and `tools` are configured on:
- A. The session
- B. The agent object
- C. The environment
- D. The vault

**Q51.** What does each session provision?
- A. A new agent
- B. A container (workspace where tools execute)
- C. A new API key
- D. A vault

**Q52.** Archiving an agent is:
- A. Reversible anytime
- B. Permanent — read-only, no unarchive, new sessions can't reference it
- C. The same as deleting
- D. Automatic after 24h

**Q53.** MCP server *credentials* are stored in:
- A. The agent's `mcp_servers` array
- B. A vault, attached to sessions via `vault_ids`
- C. The session title
- D. Environment variables in the prompt

**Q54.** To receive agent output in real time you should:
- A. Poll once
- B. Stream events (`GET /v1/sessions/{id}/events/stream`)
- C. Wait for an email
- D. Read the agent object

**Q55.** When opening an SSE stream and sending a kickoff message, you should:
- A. Send first, then open the stream
- B. Open the stream first (stream-first), then send
- C. Order doesn't matter
- D. Never open a stream

**Q56.** An `Outcome` (`user.define_outcome`) is used to:
- A. Delete a session
- B. Run a rubric-graded iterate → grade → revise loop
- C. Set the model
- D. Create a vault

---

## Section 7 — Migration & Behavior (Q57–Q60)

**Q57.** Migrating Opus 4.7 → Opus 4.8 requires:
- A. A full rewrite — many breaking changes
- B. The model-ID swap plus prompt re-tuning (no new breaking changes)
- C. Switching to `budget_tokens`
- D. Re-adding `temperature`

**Q58.** Last-assistant-turn prefills on the 4.6/4.7/4.8 family and Fable 5:
- A. Are required
- B. Return a 400 — use structured outputs or system-prompt instructions instead
- C. Improve quality
- D. Are silently ignored

**Q59.** A behavioral shift in Opus 4.8 vs 4.7 is that 4.8 tends to:
- A. Narrate less and never ask questions
- B. Narrate more and ask more often on minor decisions
- C. Refuse everything
- D. Ignore the system prompt

**Q60.** Claude Fable 5 requires which data-retention setting?
- A. Zero data retention (ZDR)
- B. 30-day data retention (not available under ZDR)
- C. 7-day retention
- D. No requirement

---

# Answer Key

| Q | Ans | Note |
|---|-----|------|
| 1 | B | Fable 5 is the most capable *widely released* model. |
| 2 | C | Exact IDs carry no date suffix. |
| 3 | C | 1M context. |
| 4 | B | $5 in / $25 out per MTok. |
| 5 | B | Haiku 4.5 = 200K (others are 1M). |
| 6 | C | "balanced" → Sonnet 4.6. |
| 7 | A | Mythos 5 is Project Glasswing only. |
| 8 | B | Use the Models API for live capability data. |
| 9 | C | `count_tokens` — never `tiktoken`. |
| 10 | B | Same tokenizer as Opus 4.8. |
| 11 | B | Adaptive thinking. |
| 12 | B | `budget_tokens` removed → 400. |
| 13 | C | Inside `output_config`. |
| 14 | B | `xhigh`. |
| 15 | B | 400 — omit `thinking` entirely. |
| 16 | C | Default is `omitted`. |
| 17 | A | `display: "summarized"`. |
| 18 | B | temperature/top_p/top_k removed. |
| 19 | A | name, description, input_schema. |
| 20 | B | `tool_use`. |
| 21 | B | Matching `tool_use_id`. |
| 22 | B | Must use at least one tool. |
| 23 | B | `disable_parallel_tool_use: true`. |
| 24 | B | Anthropic-hosted sandbox, no internet. |
| 25 | B | `pause_turn`. |
| 26 | B | Re-send; server resumes automatically (no "Continue."). |
| 27 | B | Gate / render / audit / parallelize. |
| 28 | B | Intermediate results stay in the script, not context. |
| 29 | B | `@beta_tool`. |
| 30 | B | `output_config: {format: {...}}`. |
| 31 | B | Prefix match invariant. |
| 32 | B | tools → system → messages. |
| 33 | C | 4 breakpoints. |
| 34 | B | `datetime.now()` in the prefix. |
| 35 | B | `cache_read_input_tokens`. |
| 36 | B | 1.25× for 5-min TTL (2× for 1h). |
| 37 | B | Model switch invalidates the whole cache. |
| 38 | B | Append a `role: "system"` message (beta). |
| 39 | B | 429 = rate limited, retryable. |
| 40 | B | 401 = auth error. |
| 41 | C | 400 is not retried (client error). |
| 42 | A | Typo in model ID. |
| 43 | B | 50% discount. |
| 44 | B | `file_id`. |
| 45 | B | Stream to avoid timeouts. |
| 46 | C | First message must be `user`. |
| 47 | C | `refusal`. |
| 48 | B | HTTP 200 + `stop_reason: "refusal"`. |
| 49 | B | Agent once → Session every run. |
| 50 | B | On the agent object. |
| 51 | B | A container workspace. |
| 52 | B | Permanent, no unarchive. |
| 53 | B | In a vault, via `vault_ids`. |
| 54 | B | Stream events via SSE. |
| 55 | B | Stream-first, then send. |
| 56 | B | Rubric-graded iterate loop. |
| 57 | B | Model-ID swap + prompt re-tuning. |
| 58 | B | Prefills 400 — use structured outputs. |
| 59 | B | 4.8 narrates more / asks more. |
| 60 | B | 30-day retention required. |

---

## Scoring

- 54–60 correct (90%+): Excellent — strong command of the platform.
- 48–53 (80–89%): Solid; review the sections you missed.
- 42–47 (70–79%): Passing; revisit thinking/effort, caching, and managed agents.
- Below 42: Re-study the Messages API fundamentals, models table, and tool use.
