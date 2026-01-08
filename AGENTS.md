Commit message：add AGENTS rules
Commit
# AGENTS (Codex Working Rules)

## Goal
Build DSCM MVP backend following docs in /docs, starting from batch processing skeleton.

## Tech stack (MUST)
- Node.js 20
- TypeScript
- Express
- Redis (for idempotency)
- Dockerfile + docker-compose.yml for one-command run

## Must read first
- /docs/DSCM产品开发文档6.4-plus.* (source of truth)

## Non-negotiable requirements
1) API endpoints:
- POST /api/v1/batch
- GET  /api/v1/batch/:batchId/result
- GET  /api/v1/health
- GET  /api/v1/metrics (Prometheus)

2) Idempotency:
- Use Redis atomic SET key value NX EX (or Lua). Do NOT use exists+set.
- Key MUST include batchId, agentName, storeId, logicalTime; reserve payloadHash extension.

3) Degradation:
- Implement DegradationService + DegradedSnapshot + DegradationSummary as per docs.
- Optional source with no fallback must still return safe default (avoid undefined).

4) Metrics:
- Prometheus metrics MUST avoid high-cardinality labels (NO batchId/storeId).
- Only low-cardinality labels allowed (status/source/action/agent).

5) Deliverables:
- README with exact run steps: docker compose up
- curl examples to verify endpoints
- Basic tests or a minimal verification script

## Coding style
- Prefer clear types and zod validation for request bodies.
- No hidden magic; keep modules readable.
