# Claude Certified Architect – Foundations Certification

**Practice Exam — Questions and Answers (60 questions)**

> Source: recovered from the original Q&A chat (uploaded `Claude_Certified_Architect_Foundations_Q_and_A.docx`).
> Note on images: only the Question 60 screenshot was still available on disk
> (each upload reused the same file path, so earlier images were overwritten and
> could not be recovered). All 60 questions and selected answers are listed below.

---

## Scenario 1: Customer Support Resolution Agent

**Q1.** Claude frequently requests `get_customer` and `lookup_order` in separate sequential turns even when both are needed upfront. How do you reduce API round-trips?
**A: B** — Create composite tools like `get_customer_with_orders` that bundle common lookup combinations into single calls.

**Q2.** Tools return inconsistent data formats (Unix timestamps, ISO 8601 dates, numeric status codes); some are third-party MCP servers you cannot modify. How do you normalize?
**A: B** — Modify tools you control to return human-readable formats; create wrapper tools for third-party tools.

**Q3.** Prevent the agent from calling order/refund tools before customer identity is verified (financial-harm reliability).
**A: D** — Add a programmatic prerequisite that blocks `lookup_order` and `process_refund` calls until `get_customer` has returned a verified customer ID.

**Q4.** A customer message contains multiple distinct concerns. How should the agent handle it?
**A: B** — Decompose the request into distinct concerns, then investigate each in parallel using shared customer context before synthesizing a resolution.

**Q5.** The agent is miscalibrated on when to escalate vs. resolve autonomously. How to fix?
**A: C** — Add explicit escalation criteria to your system prompt with few-shot examples demonstrating when to escalate versus resolve autonomously.

**Q6.** How does the application know when to continue the tool loop vs. stop?
**A: C** — Check the `stop_reason` field in Claude's response—continue when it equals `"tool_use"` and stop when it equals `"end_turn"`.

**Q7.** Improve tool selection on ambiguous cases.
**A: C** — Add 4-6 examples targeting ambiguous scenarios, each showing reasoning for why one tool was chosen over plausible alternatives.

**Q8.** First step to fix poor tool selection.
**A: C** — Expand each tool's description to include input formats it handles, example queries, edge cases, and boundaries explaining when to use it versus similar tools.

**Q9.** Why does Claude misroute "account" requests despite good tool descriptions?
**A: D** — The model's base training creates associations between "account" terminology and customer-related operations that override the tool descriptions.

**Q10.** `get_customer` returns multiple matches for an identifier. What should the agent do?
**A: A** — Instruct Claude to ask for an additional identifier (email, phone, or order number) when `get_customer` returns multiple matches, before taking any customer-specific action.

**Q11.** Responses have variable gaps in completeness. Adaptive fix?
**A: B** — Add a self-critique step where the agent evaluates its draft response for completeness—ensuring it addresses the customer's concern, includes relevant context, and is accurate.

**Q12.** Transactional facts get lost in summarized conversation history.
**A: C** — Extract transactional facts (amounts, dates, order numbers) into a persistent "case facts" block included in each prompt, outside the summarized history.

**Q13.** Handle multi-concern customer messages reliably.
**A: D** — Implement a preprocessing layer that uses a separate model call to decompose multi-concern messages into individual requests, process each independently, then combine.

**Q14.** First thing to review when tools are mis-selected.
**A: D** — Review tool descriptions to ensure they clearly distinguish each tool's purpose.

**Q15.** Customer requests a competitor price match; policy allows adjustments for price drops on your own site within 14 days but is silent on competitors.
**A: D** — Escalate: the competitor price-match is a genuine policy gap requiring human interpretation.

---

## Scenario 2: Code Generation with Claude Code

**Q16.** Apply test conventions to test files scattered across the repo (not in one directory).
**A: A** — Create rule files in `.claude/rules/` with YAML frontmatter specifying glob patterns to conditionally apply conventions based on file paths.

**Q17.** Ambiguous Slack-integration architecture decision before implementing.
**A: D** — Enter plan mode to explore the integration options and their architectural implications, then present a recommendation before implementing.

**Q18.** Bloated `CLAUDE.md` is hard to maintain.
**A: C** — Create separate markdown files in `.claude/rules/`, each covering one topic (e.g., `testing.md`, `api-conventions.md`).

**Q19.** A skill has three issues (missing required params, verbose output pollution, over-broad tool access).
**A: A** — Add `argument-hint` frontmatter to prompt for required parameters, use `context: fork` to isolate execution, and restrict `allowed-tools` to file write operations.

**Q20.** Verbose Phase 1 exploration output pollutes the main context.
**A: C** — Use the Explore subagent for Phase 1 to isolate verbose output and return a summary, then continue Phases 2-3 in the main conversation.

**Q21.** A skill's analysis output pollutes the main session.
**A: B** — Add `context: fork` to the skill's frontmatter to run the analysis in an isolated sub-agent context.

**Q22.** Personalize a team-shared skill for your own use.
**A: A** — Create a personal version in `~/.claude/skills/` with a different name like `/my-commit`.

**Q23.** Isolate exploration discussion from contaminating implementation.
**A: D** — Add `context: fork` to the skill's frontmatter.

**Q24.** Use exemplar endpoints as a pattern only when creating new endpoints.
**A: D** — Create a skill that references the exemplar endpoints and includes pattern-following instructions, invoked on-demand via slash command.

**Q25.** A coding guideline only some developers receive.
**A: D** — The guideline exists in the original developers' `~/.claude/CLAUDE.md` (user-level) instead of the project's `.claude/CLAUDE.md`. Move it to the project level.

**Q26.** Share an MCP server config with the team without committing credentials.
**A: B** — Add the server to a project-scoped `.mcp.json` with environment variable expansion (`${GITHUB_TOKEN}`) for authentication, and document the required environment in the README.

**Q27.** Organize universal standards vs. task-specific workflows.
**A: C** — Keep universal standards in `CLAUDE.md` and create Skills for task-specific workflows (PR reviews, deployments, migrations) with trigger keywords.

**Q28.** Prose instructions for a transformation are ambiguous.
**A: D** — Provide 2-3 concrete input-output examples showing the expected transformation for representative API responses.

**Q29.** Distribute slash commands to the whole team automatically.
**A: A** — Place them in the `.claude/commands/` directory in the project repository.

**Q30.** Plan a monolith-to-microservices restructuring before coding.
**A: B** — Enter plan mode to explore the codebase, understand dependencies, and design an implementation approach before making changes.

---

## Scenario 3: Multi-Agent Research System

**Q31.** Context bloat from verbose subagent outputs reaching the coordinator.
**A: B** — Modify upstream agents to return structured data (key facts, citations, relevance scores) instead of verbose content and reasoning.

**Q32.** Subagents research overlapping material.
**A: D** — Have the coordinator explicitly partition the research space before delegation, assigning distinct subtopics or source types to each agent.

**Q33.** Document analysis encounters two conflicting figures.
**A: C** — Complete the document analysis with both figures included, explicitly annotate the conflict with source attribution, and let the coordinator decide how to reconcile.

**Q34.** Error handling for web-search results (failures vs. empty).
**A: D** — Distinguish access failures (timeout) needing retry decisions from valid empty results ("0 results") representing successful queries.

**Q35.** Two sets of findings must be combined.
**A: A** — The coordinator passes both sets of findings to the synthesis agent for unified integration.

**Q36.** Reduce coordinator overload from subagent errors (transient PDF failures).
**A: B** — Have the subagent implement local recovery for transient failures and only propagate errors it cannot resolve to the coordinator, including what was attempted and partial results.

**Q37.** Fact verification routing where 85% are simple lookups.
**A: D** — Give the synthesis agent a scoped `verify_fact` tool for simple lookups, while complex verifications continue delegating to the web search agent through the coordinator.

**Q38.** Multi-agent error-handling architecture.
**A: A** — The coordinator can observe all interactions, handle errors consistently, and decide what information each subagent should receive.

**Q39.** 45% of "analyze uploaded report" requests misrouted to a mislabeled web-search tool.
**A: B** — Rename the web search tool to `extract_web_results` and update its description to "processes and returns information retrieved from web searches and URLs."

**Q40.** A corrupted PDF is encountered by a subagent.
**A: D** — Return the error with context to the coordinator agent, letting it decide how to proceed.

**Q41.** The web search subagent times out on a complex topic; design how the failure flows back to the coordinator to enable intelligent recovery.
**A: B** — Return structured error context to the coordinator including the failure type, the attempted query, any partial results, and potential alternative approaches.

**Q42.** A document analysis agent given a general-purpose `fetch_url` tool now frequently fetches search-result pages (ad-hoc web search), causing inconsistent results. Most effective fix?
**A: A** — Replace `fetch_url` with a `load_document` tool that validates URLs point to document formats.

**Q43.** Synthesis reliably cites the first 15K and final 10K tokens but omits critical findings in the middle 50K. How to restructure the aggregated input?
**A: D** — Place a key findings summary at the beginning of the aggregated input and organize detailed results with explicit section headers for easier navigation.

**Q44.** Web search returned 3 of 5 source categories (2 timed out); synthesis must produce findings from mixed-quality input. Best error-propagation strategy?
**A: A** — Structure the synthesis output with coverage annotations indicating which findings are well-supported versus which topic areas have gaps due to unavailable sources.

**Q45.** Final reports cover only visual arts; the coordinator decomposed the topic into three visual-arts subtasks. Most likely root cause?
**A: C** — The coordinator agent's task decomposition is too narrow, resulting in subagent assignments that don't cover all relevant domains of the topic.

---

## Scenario 4: Claude Code for Continuous Integration

**Q46.** A re-review after fixes still re-flags 5 already-fixed issues. Eliminate redundant feedback while keeping thorough analysis.
**A: A** — Include prior review findings in context, instructing Claude to only report new or still-unaddressed issues.

**Q47.** Reviews are valid but not actionable; detailed instructions still yield inconsistent output. Most reliable prompting technique for consistently actionable feedback?
**A: B** — Add 3-4 few-shot examples showing the exact format you want: issue identified, code location, specific fix suggestion.

**Q48.** False-positive rates vary by category (style 52%, docs 48%); developers are dismissing findings. Restore trust while improving the system.
**A: B** — Temporarily disable high false positive categories (style, naming, documentation) and run only high-precision categories while improving prompts.

**Q49.** Subtle issues are only caught by a different reviewer; Claude's generation reasoning considered but dismissed them. Address the root cause of this self-review limitation.
**A: D** — Have a second, independent Claude Code instance review the changes without seeing the generator's reasoning.

**Q50.** CI runs Claude Code in `--print` mode producing narrative paragraphs; you want to auto-post each finding as an inline PR comment (needs structured data). Most effective approach?
**A: D** — Use CLI flags `--output-format json` and `--json-schema` to enforce structured findings, then parse output to post inline comments via the GitHub API.

**Q51.** Two modes: a blocking pre-merge-commit hook and an overnight deep analysis that polls. Batches API gives 50% savings but up to 24h. Which mode should use batch?
**A: B** — Deep analysis only.

**Q52.** Test-case suggestions include 6 duplicates already covered in the existing suite. Most effective change to reduce duplicates?
**A: D** — Include the existing test file in the context so Claude can identify what scenarios are already covered.

**Q53.** `claude "..."` hangs waiting for interactive input in a pipeline. Correct approach to run Claude Code in an automated pipeline?
**A: A** — Add the `-p` flag: `claude -p "Analyze this pull request for security issues"`.

**Q54.** Three analyses: blocking PR style checks, weekly security audits, nightly test generation. Match each task to its API to optimize cost while keeping acceptable developer experience.
**A: D** — Use synchronous calls for PR style checks; use the Message Batches API for weekly security audits and nightly test generation.

**Q55.** Inconsistent severity ratings (same issue "critical" in some PRs, "medium" in others). Improve severity consistency.
**A: B** — Include explicit severity criteria in your prompt with concrete code examples for each severity level.

**Q56.** 15 findings/PR, 40% false positives; bottleneck is investigation time (clicking into each finding to read reasoning); filtering before review is rejected. Best change?
**A: B** — Require Claude to include its reasoning and confidence assessment inline with each finding.

**Q57.** A single-pass review of 14 files produces inconsistent, contradictory results. How should you restructure the review?
**A: A** — Split into focused passes: analyze each file individually for local issues, then run a separate integration-focused pass examining cross-file data flow.

**Q58.** Reduce API costs: a blocking pre-merge check plus an overnight technical-debt report. Manager proposes switching both to the Batches API. How to evaluate?
**A: B** — Use batch processing for the technical debt reports only; keep real-time calls for pre-merge checks.

**Q59.** An iterative review uses tool calling (requesting related files mid-analysis). Evaluating batch processing—what is the primary technical constraint?
**A: A** — The asynchronous model prevents executing tools mid-request and returning results for Claude to continue analysis.

**Q60.** Comment/docstring review over-flags acceptable patterns (TODO markers, plain descriptions) and misses comments describing behavior the code no longer implements. Change addressing the root cause?
**A: A** — Specify explicit criteria: flag comments only when their claimed behavior contradicts actual code behavior.

---

## Answer Key (quick reference)

| Q | A | Q | A | Q | A | Q | A | Q | A | Q | A |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | B | 11 | B | 21 | B | 31 | B | 41 | B | 51 | B |
| 2 | B | 12 | C | 22 | A | 32 | D | 42 | A | 52 | D |
| 3 | D | 13 | D | 23 | D | 33 | C | 43 | D | 53 | A |
| 4 | B | 14 | D | 24 | D | 34 | D | 44 | A | 54 | D |
| 5 | C | 15 | D | 25 | D | 35 | A | 45 | C | 55 | B |
| 6 | C | 16 | A | 26 | B | 36 | B | 46 | A | 56 | B |
| 7 | C | 17 | D | 27 | C | 37 | D | 47 | B | 57 | A |
| 8 | C | 18 | C | 28 | D | 38 | A | 48 | B | 58 | B |
| 9 | D | 19 | A | 29 | A | 39 | B | 49 | D | 59 | A |
| 10 | A | 20 | C | 30 | B | 40 | D | 50 | D | 60 | A |

**Scenario map:** Q1–Q15 Customer Support Resolution Agent · Q16–Q30 Code Generation with Claude Code · Q31–Q45 Multi-Agent Research System · Q46–Q60 Claude Code for Continuous Integration.
