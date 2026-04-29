### Example of an Ideal Claude Prompt Structure

```markdown
## Role
You are an expert Python developer.

## Task
Review the code below and identify any security vulnerabilities.

## Rules
- **Do not** rewrite the entire script.
- Explain each vulnerability in plain English.
- Provide a short snippet showing how to fix it.
- If uncertain, say so rather than guess.

## Context
<context>
This script handles user uploads for a web application.
</context>

## Code
<code>
[Insert code here]
</code>

## Thinking
Reason step-by-step inside `<thinking>` tags, then answer inside `<answer>` tags.
Before finishing, verify your answer against [test criteria].
```

---

### How to use the Thinking section

Claude has **two distinct mechanisms** for reasoning. Use the right one for your context.

**1. Manual chain-of-thought (any Claude model, in the prompt itself)** The `<thinking>` / `<answer>` pattern shown above. This is what you write inside a normal prompt — no API flag required, works in claude.ai, the API, and Claude Code. It cleanly separates scratchwork from the final output so downstream parsers can grab just `<answer>`.

```markdown
## Thinking
Work through the problem inside `<thinking>` tags. Inside, you should:
1. Restate what is being asked in your own words.
2. List the relevant facts from the `<context>` and `<code>` blocks.
3. Consider at least two approaches and pick the stronger one with a one-line justification.
4. Identify any assumptions you are making and flag them.

Then place your final response inside `<answer>` tags. Do not repeat the thinking in the answer.

Before closing `<answer>`, verify your output against:
- All rules in the `## Rules` section
- Edge cases: empty input, malformed input, oversized input
```

**2. Extended / adaptive thinking (API parameter on Claude 4.x models)** A model-level reasoning mode enabled via the `thinking` parameter in the API. Claude generates internal reasoning tokens that don't appear in the response (or appear summarized). On Opus 4.6 and Sonnet 4.6+, prefer **adaptive thinking** (`thinking: {type: "adaptive"}`), where Claude decides when and how much to think based on query complexity and your `effort` setting.

When extended/adaptive thinking is **enabled**:

- **Do not** add "think step by step" instructions — the model manages its own reasoning budget, and prescriptive CoT is redundant or counterproductive.
- **Do** prefer high-level guidance ("think thoroughly", "consider multiple approaches") over hand-written step-by-step plans. Claude's reasoning often exceeds what a human would prescribe.
- **Do** include `<thinking>` patterns inside few-shot examples — Claude generalizes the style into its extended thinking blocks.
- **Do** ask Claude to self-verify ("Before you finish, verify your solution with test cases for n=0, n=1, n=5").

When extended/adaptive thinking is **disabled** (the default in claude.ai chat and most API calls):

- The manual `<thinking>` / `<answer>` pattern above is the right tool.
- Use it for any task a human would need to think through: multi-step math, code review, decisions with several factors. Skip it for lookups or simple formatting.

### Common pitfalls

- **Don't repeat thinking in the answer.** Explicitly instruct: _"Do not repeat your thinking in the final answer."_Otherwise Claude often echoes the reasoning into the visible output.
- **Don't combine prescriptive CoT with extended thinking.** Pick one. Mixing them wastes tokens and can degrade quality.
- **Don't over-prescribe steps.** "Think about this thoroughly, considering multiple approaches" outperforms a rigid 7-step recipe in most tasks.

---

**Claude-specific notes:**

- Use XML tags (`<context>`, `<code>`, `<answer>`) for distinct content blocks — Claude parses them natively.
- Put long inputs near the top, the actual question near the bottom.
- Be explicit; Claude 4.x follows instructions literally and won't infer "above and beyond" behavior.

**Sources:** Anthropic [Prompting best practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-prompting-best-practices) · [Extended thinking tips](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/extended-thinking-tips) · [Adaptive thinking](https://platform.claude.com/docs/en/build-with-claude/adaptive-thinking) · [Use XML tags](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags) · [Interactive Prompt Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial)