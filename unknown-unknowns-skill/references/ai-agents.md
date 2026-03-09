# AI Agent Systems Unknown Unknowns Checklist

This is the highest-density area of unknown unknowns because AI agent systems are new enough that most developers have never encountered these failure modes.

## Prompt Injection & Security
- Direct prompt injection — users craft inputs that override system prompts
- Indirect prompt injection — malicious content in documents/websites the agent reads
- Tool abuse — agent tricked into calling tools with malicious parameters
- Privilege escalation — agent accessing resources beyond intended scope
- Data exfiltration via prompt injection — agent tricked into sending data to attacker
- System prompt extraction — users can often get the agent to reveal its instructions
- Jailbreaking — circumventing safety guidelines through creative prompting
- Multi-step attacks — benign individual steps that combine into harmful actions

## Cost & Resource Control
- Token costs can explode with agent loops (agent retrying or thinking in circles)
- No per-user or per-request cost caps → one user can drain your budget
- Long conversations accumulate context → each message costs more than the last
- Tool calls multiply costs (each tool call is a separate API call)
- Retry loops without backoff or max attempts
- Parallel agent execution without concurrency limits
- Large document processing (PDFs, codebases) consuming massive token counts
- No monitoring of per-user or per-session cost

## Reliability & Failure Modes
- LLMs are non-deterministic — same input can produce different outputs
- Hallucinated tool calls — agent invents tools or parameters that don't exist
- Hallucinated data — agent confidently presents fabricated information
- Cascading hallucinations — one agent's hallucination feeds into another's input
- Infinite loops — agent gets stuck in a retry or reasoning cycle
- Partial failures — agent completes some steps but fails silently on others
- Context window overflow — conversation exceeds model limits, drops information
- Model degradation — API provider changes model behavior without notice
- Rate limiting from API providers during high usage

## Multi-Agent Specific
- No coordination protocol — agents working at cross purposes
- Deadlocks — agents waiting on each other
- Race conditions — multiple agents modifying the same resource
- Message ordering not guaranteed in async agent communication
- No shared state management — agents have inconsistent views of the world
- Error propagation — one agent's failure cascades through the system
- No supervisor/orchestrator — no way to detect when the system is stuck
- Trust boundaries — should Agent A trust Agent B's output?
- Observability — hard to debug what happened across multiple agents
- Testing — traditional unit tests don't cover non-deterministic behavior

## Data & Privacy
- User data sent to LLM APIs — check provider's data retention policies
- PII in prompts — sensitive data exposed to third-party AI providers
- Conversation logs containing sensitive information
- Agent memory/context containing data from other users (multi-tenant leaks)
- Training data inclusion — some providers may train on your data
- GDPR right to deletion includes AI conversation data
- Vector database containing embeddings of sensitive documents

## UX & Trust
- Users don't understand what the AI can and can't do
- No confidence indicators — users can't tell when the AI is guessing
- Users over-trust AI output (especially in domains like medical, legal, financial)
- No human-in-the-loop for high-stakes decisions
- Missing audit trail — can't explain why the AI did something
- Inconsistent behavior confuses users (different answers to same question)
- Latency expectations — AI operations are slower than traditional CRUD
- Error messages from AI failures are often cryptic to users
- No feedback mechanism — users can't report bad AI behavior

## Evaluation & Quality
- No evaluation framework — no way to measure if the AI is getting better or worse
- No regression testing for prompt changes
- A/B testing is complex with non-deterministic outputs
- Ground truth is expensive to establish for open-ended tasks
- Prompt drift — system prompt accumulates changes that interact unpredictably
- Model version changes break carefully tuned prompts
- No monitoring of output quality over time
- Bias in AI outputs not measured or mitigated

## Architecture Patterns
- Putting too much logic in prompts vs code (prompts are fragile, code is reliable)
- Not separating AI decision-making from action execution
- Missing fallback for when AI is unavailable or degraded
- No caching of common AI responses (huge cost and latency savings)
- Streaming responses not implemented (users staring at blank screen)
- No graceful degradation — if AI fails, entire feature fails
- Tight coupling to specific AI provider — no abstraction layer
- Missing structured output validation — AI returns unexpected formats
- No content filtering on AI outputs before showing to users
