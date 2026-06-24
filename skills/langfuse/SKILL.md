# Langfuse Skill

Teaches agents how to post scores and query session history in Langfuse.

## Score schemas

Four score schemas are configured in this workspace. Set them on the trace for
the current session via `POST /api/public/scores`.

| Name | Type | Who sets it | When |
|------|------|-------------|------|
| `sopr_pass` | boolean | SOPR gate workflow | After PR diff analysis — true if all claims cited |
| `human_review_triggered` | boolean | Reviewer agent | On escalation — true if output needs human review |
| `task_success` | boolean | Conductor | On session completion — true if task completed |
| `cost_usd` | numeric | Conductor | On session completion — accumulated USD from `vreko.usage.cost_usd` |

## Posting a score

```bash
curl -s -u "$LANGFUSE_PUBLIC_KEY:$LANGFUSE_SECRET_KEY" \
  "$LANGFUSE_BASE_URL/api/public/scores" \
  -X POST -H "Content-Type: application/json" \
  -d '{"traceId":"<trace_id>","name":"task_success","value":1,"dataType":"BOOLEAN"}'
```

For `BOOLEAN` scores, use `1` for true and `0` for false.
For `NUMERIC` scores (`cost_usd`), pass the float value directly.

## Querying session history

List traces for a session (replace `<session_id>` with `langfuse.session.id`):

```bash
curl -s -u "$LANGFUSE_PUBLIC_KEY:$LANGFUSE_SECRET_KEY" \
  "$LANGFUSE_BASE_URL/api/public/traces?sessionId=<session_id>" | jq .
```

Fetch a single trace with its scores:

```bash
curl -s -u "$LANGFUSE_PUBLIC_KEY:$LANGFUSE_SECRET_KEY" \
  "$LANGFUSE_BASE_URL/api/public/traces/<trace_id>" | jq '{id,name,scores}'
```

## Trace stamp attributes

Every swarm session emits a canonical trace stamp. Key attributes available
in Langfuse trace metadata:

| Attribute | Langfuse field |
|-----------|---------------|
| `vreko.agent.role` | metadata.vreko.agent.role |
| `vreko.linear.issue_id` | metadata.vreko.linear.issue_id |
| `vreko.usage.cost_usd` | metadata.vreko.usage.cost_usd |
| `langfuse.session.id` | Sessions tab |
| `gen_ai.request.model` | metadata — drives cost lookup |

## Credentials

Credentials are in Doppler under the `vreko` project. Load with
`doppler run --` before any Langfuse API call. Never hardcode keys.

Required vars: `LANGFUSE_BASE_URL`, `LANGFUSE_PUBLIC_KEY`, `LANGFUSE_SECRET_KEY`.
