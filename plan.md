# Afterbells — 100-Step Functional Implementation Plan

## Project status

- **Project:** Afterbells
- **Inspected revision:** `main` at `0a21cb7e497b54e9318acc5d86845884d6b1c474` (`origin/main`); baseline date 2026-10-11.
- **Implementation status:** Planning only. Every roadmap item is `NOT_STARTED`; no application change is part of this deliverable.
- **Maturity:** Verified analysis MVP and execution-blocked prototype, not a secure multi-user or production system.
- **Verified functionality:** Decimal BSC representation normalization; constrained BUY parsing; deterministic policy comparison; recorded/live-labelled snapshots; canonical SHA-256 receipts and replay; REST/CLI/read-only MCP surfaces; fail-closed wallet simulation; fixture demo; frontend unit suite.
- **Highest-priority blockers:** global unauthenticated SQLite state, no owned/versioned strategies, inline jobs, incomplete evidence timestamps/depth, no quote-to-calldata binding, Boolean confirmation, no isolated signer/idempotent submission/reconciliation, and a failing production web build.
- **Deferred work:** visual redesign is a separate workstream. This plan includes only minimal UI wiring needed for functional, accessible workflows.
- **Local reference:** `implementation.md` was read in full and remains ignored/untracked. It predates current verification; its safety direction is retained, while this plan follows inspected code.
- **Reference screenshots:** no image files accompanied the supplied attachment; the attachment's enumerated URLs were categorized as technical, ecosystem, and delivery resources. Delivery forms/community links are not runtime dependencies.
- **Official resources inspected successfully:** Binance Web3 authentication, RWA Data, Trading API, Agentic Wallet stock trading and skills reference, and BNB Chain documentation. The repository's recorded `llms.txt`/`llms-full.txt` and checked-in Binance skills snapshot were also inspected. The remaining listed portal pages/forms/community sites were categorized but not needed to assert runtime behavior.
- **Verified provider finding:** current `BinancePublicClient` calls unauthenticated `/bapi/...` surfaces. Current official Web3 REST documentation instead specifies API-key authentication, HMAC-SHA256 over `timestamp + method + /build/requestPath + rawBody`, nonce/replay controls, and signed RWA/trading endpoints. Migration must be additive and fixture-backed until credentials safely verify parity.
- **Assumptions requiring decisions:** production identity provider, PostgreSQL hosting, durable-worker deployment, data retention, legal/geographic eligibility, authoritative market freshness/depth source, and whether controlled execution is a release requirement. No implementation step may infer these choices silently.

## Baseline evidence

The worktree already contained user changes to `.gitignore`, `dx/raw/api-events.jsonl`, and untracked `dx/events.jsonl`; they are out of scope and must be preserved. `./scripts/ci-local.sh --skip-contracts` failed because the script lacks executable permission. Running it with `bash` proved web typecheck, ESLint, Prettier, and 16 Vitest tests pass, then hit a read-only global uv cache. With `UV_CACHE_DIR=/tmp/afterbells-uv-cache`, Ruff and MyPy passed; pytest did not complete during the audit and therefore is not claimed passing. `pnpm build` compiled but failed collecting `/constitution`. Keeper tests could not start because its standalone dependencies were not installed (`tsc` absent). Foundry was unavailable (`forge: command not found`). No live credentialed request, transaction, deployment, or remote mutation was performed.

Architecture evidence: `services/decisions.py` is a self-contained analysis receipt flow; `pipeline.py` is the separate execution-oriented flow. `api/main.py` exposes unauthenticated constitution, task, receipt, and intent routes. `persistence.py` is synchronous local SQLite with global active constitution and no migrations/ownership. `keeper.py` executes inline. `mcp_server.py` is correctly read-only. `baw.py` wraps CLI quote and swap commands but cannot prove an identical simulated/submitted envelope. `ExecState` lacks the complete required lifecycle. CI covers four stacks and a simple secret grep, but not migrations, supply-chain provenance, or release deployment.

## Delivery protocol and scope

Each numbered item below is one independently verifiable change and, only after its acceptance criteria pass, exactly one dedicated commit using the stated message. Before every item: verify branch/worktree, preserve unrelated changes, stage only listed files, inspect the diff for secrets/generated files, run the stated commands, and verify the commit. On failure, do not commit or begin a dependent item. External-credential steps use deterministic fixtures first and remain `BLOCKED` for live verification until credentials/access exist. `EXECUTION_ENABLED=false` remains the default throughout; Phase 8 cannot enable mainnet.

First production scope is the authenticated, durable analysis product through Gate C. Controlled execution is an optional gated capability; analysis production must not depend on it. DeFi, portfolio, b402 settlement, PancakeSwap-specific routing, and unrelated Binance modules are excluded unless a later approved requirement needs them. Selected provider modules are RWA discovery/underlying data, size-specific trading quotes/builds, transaction simulation/status, and wallet balance/reconciliation because each supports a named workflow.

## Progress ledger

All rows begin `NOT_STARTED`. Verification is the step's `Verify` field; dependencies use step IDs.

| Phase | Step | Capability | Status | Dependencies | Verification | Commit message |
|---:|---:|---|---|---|---|---|
| 1 | 1.1 | Pin and validate toolchain versions | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.2 | Repair the local CI entry point | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.3 | Fix the production web build baseline | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.4 | Reproduce backend quality checks | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.5 | Restore standalone keeper verification | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.6 | Make contract verification self-describing | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.7 | Inventory configuration and side effects | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.8 | Document executable architecture truth | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.9 | Add API contract snapshots | NOT_STARTED | See step | See step Verify | See step Commit |
| 1 | 1.10 | Publish the verified baseline report | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.1 | Define a versioned strategy schema | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.2 | Model typed strategy constraints | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.3 | Version the constrained intent parser | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.4 | Separate system ceilings from user policy | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.5 | Define the complete lifecycle vocabulary | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.6 | Enforce lifecycle transitions | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.7 | Add strategy compiler diagnostics | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.8 | Add strategy compatibility adapters | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.9 | Add shared financial boundary vectors | NOT_STARTED | See step | See step Verify | See step Commit |
| 2 | 2.10 | Expose minimal strategy API contracts | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.1 | Define normalized provider observation contracts | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.2 | Introduce a provider transport interface | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.3 | Implement Binance Web3 request signing | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.4 | Add official RWA discovery adapter | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.5 | Preserve xStocks with explicit provenance | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.6 | Normalize freshness and market sessions | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.7 | Add size-specific quote contracts | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.8 | Add quote/build and simulation adapters | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.9 | Add provider circuit breaking and cache semantics | NOT_STARTED | See step | See step Verify | See step Commit |
| 3 | 3.10 | Establish credentialed integration verification | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.1 | Define the evaluation input aggregate | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.2 | Consolidate duplicate policy evaluation | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.3 | Add typed rule outcomes | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.4 | Harden normalization and identity checks | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.5 | Implement freshness and data-quality rules | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.6 | Implement executable-liquidity rules | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.7 | Make route scoring deterministic | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.8 | Add policy conflict analysis | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.9 | Produce structured decision explanations | NOT_STARTED | See step | See step Verify | See step Commit |
| 4 | 4.10 | Route all analysis interfaces through one engine | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.1 | Version immutable market snapshots | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.2 | Scope recorded-fixture provenance | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.3 | Define typed receipt envelopes | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.4 | Publish canonical JSON specification | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.5 | Add shared canonicalization vectors | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.6 | Bind decisions to strategy and engine versions | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.7 | Harden offline replay | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.8 | Add receipt authenticity vocabulary | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.9 | Harden receipt registry semantics | NOT_STARTED | See step | See step Verify | See step Commit |
| 5 | 5.10 | Complete analysis receipt workflow | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.1 | Define job and event schemas | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.2 | Add persisted enqueue semantics | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.3 | Implement worker leases | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.4 | Classify bounded retries | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.5 | Add durable idempotency keys | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.6 | Add cancellation and deadlines | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.7 | Persist auditable tool invocations | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.8 | Constrain keeper task capabilities | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.9 | Recover interrupted jobs at startup | NOT_STARTED | See step | See step Verify | See step Commit |
| 6 | 6.10 | Expose job status and event APIs | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.1 | Select and document the identity boundary | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.2 | Add request authentication middleware | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.3 | Define tenant and ownership models | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.4 | Introduce migrations and repository interfaces | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.5 | Implement transactional production persistence | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.6 | Persist owned strategy revisions | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.7 | Enforce object-level authorization | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.8 | Add CSRF, CORS, and rate-limit controls | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.9 | Protect diagnostics and redact errors | NOT_STARTED | See step | See step Verify | See step Commit |
| 7 | 7.10 | Wire authenticated functional frontend flows | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.1 | Create canonical immutable execution plans | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.2 | Bind exact-envelope simulation | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.3 | Replace Boolean confirmation with approvals | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.4 | Isolate the signing service boundary | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.5 | Persist idempotent submission attempts | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.6 | Handle uncertain submission outcomes | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.7 | Verify authoritative chain receipts | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.8 | Reconcile balances and positions | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.9 | Add global and scoped kill switches | NOT_STARTED | See step | See step Verify | See step Commit |
| 8 | 8.10 | Validate controlled testnet lifecycle | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.1 | Build an isolated integration environment | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.2 | Test strategy-to-decision success | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.3 | Test compare, export, replay, and tamper | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.4 | Test unsupported and conflicting intents | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.5 | Test provider degradation and schema drift | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.6 | Test stale, unknown, and insufficient evidence | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.7 | Test tenant isolation and abuse controls | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.8 | Test worker restart and duplicate delivery | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.9 | Prove execution remains inaccessible | NOT_STARTED | See step | See step Verify | See step Commit |
| 9 | 9.10 | Run and publish the functional acceptance suite | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.1 | Harden CI branch gates | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.2 | Add supply-chain and secret scanning | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.3 | Separate runtime environments and secrets | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.4 | Add structured observability | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.5 | Define health, readiness, and alerts | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.6 | Prove backup, restore, and retention | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.7 | Validate performance and capacity | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.8 | Exercise deployment and rollback | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.9 | Complete security, privacy, and legal review gates | NOT_STARTED | See step | See step Verify | See step Commit |
| 10 | 10.10 | Execute the production release gate | NOT_STARTED | See step | See step Verify | See step Commit |

Each row delegates its full dependency expression, exact command, and exact professional commit message to the corresponding step below; those fields are part of this ledger by reference and must be updated here only for status tracking.

---

# Phase 1 — Verified Baseline and Development Reliability

**Objective:** make every future result reproducible without changing product behavior. **Problems:** non-executable CI script, sandbox-sensitive uv cache, failed web build, unavailable Foundry, missing keeper install, historical validation. **Prerequisites:** current checkout and documented runtimes. **Outcome:** one repeatable, side-effect-free baseline with explicit skips. **Risks:** dependency downloads and live probes; keep execution/network probes opt-in. **Validation:** fresh installs and all available checks produce recorded exit codes. **Exit gate / Gate A:** setup is reproducible, failures are classified, demo stays analysis-only.

### 1.1 Pin and validate toolchain versions
**Purpose:** remove runtime ambiguity. **Problem:** only broad engine constraints exist. **Targets/files:** `package.json`, `.nvmrc` (new), `.python-version` (new), `docs/validation.md`. **Implement:** pin supported Node/Python/pnpm/uv ranges and add a preflight that prints actionable mismatches. **Depends:** none. **Tests:** preflight success/failure fixtures. **Verify:** `pnpm run preflight && uv --version`. **Accept:** clean supported host passes; mismatches fail before installs. **Rollback:** remove pins/preflight without touching app state. **Commit:** `build: pin and validate development toolchains`. **Done:** checks pass and commit exists.

### 1.2 Repair the local CI entry point
**Purpose:** make the documented command executable. **Problem:** `ci-local.sh` mode is not executable and cache paths are fragile. **Targets/files:** `scripts/ci-local.sh`, `README.md`. **Implement:** set executable mode, use a writable task cache, preserve strict failure handling, and support named stages. **Depends:** 1.1. **Tests:** shell syntax and dry stage selection. **Verify:** `bash -n scripts/ci-local.sh && ./scripts/ci-local.sh --help`. **Accept:** documented invocation starts reliably without global-cache writes. **Rollback:** restore script mode/content. **Commit:** `build: make local validation runner reproducible`. **Done:** acceptance passes and commit exists.

### 1.3 Fix the production web build baseline
**Purpose:** restore deployable frontend output. **Problem:** page-data collection fails for `/constitution`. **Targets/files:** `apps/web/app/constitution/page.tsx`, related `apps/web/lib/*`, build regression test. **Implement:** isolate build-time API access and render honest unavailable state without hard dependency on a running API. **Depends:** 1.2. **Tests:** static/build-time page test. **Verify:** `pnpm build`. **Accept:** standalone build completes with no API and runtime behavior remains typed. **Rollback:** revert page boundary only. **Commit:** `fix: make constitution page safe for production builds`. **Done:** build passes and commit exists.

### 1.4 Reproduce backend quality checks
**Purpose:** establish current backend truth. **Problem:** pytest was not completed during audit. **Targets/files:** `services/api/tests/*`, `docs/validation.md`; production files only for confirmed baseline defects. **Implement:** run lint/type/test with isolated cache, classify hangs, and repair only reproducible defects. **Depends:** 1.2. **Tests:** existing suite plus timeout guard for MCP subprocess. **Verify:** `cd services/api && UV_CACHE_DIR=/tmp/afterbells-uv-cache uv run ruff check . && UV_CACHE_DIR=/tmp/afterbells-uv-cache uv run mypy -p afterbells && UV_CACHE_DIR=/tmp/afterbells-uv-cache uv run pytest -q`. **Accept:** suite terminates and result is current. **Rollback:** revert scoped fixes. **Commit:** `test: establish a reproducible backend baseline`. **Done:** evidence recorded and commit exists.

### 1.5 Restore standalone keeper verification
**Purpose:** prove the Agent Studio adapter compiles independently. **Problem:** local dependencies are absent and root install does not cover it. **Targets/files:** `services/keeper/agent/package.json`, lockfile, `README.md`, tests. **Implement:** validate standalone install/build/test instructions and fix only dependency/config drift. **Depends:** 1.1. **Tests:** existing Node tests. **Verify:** `cd services/keeper/agent && pnpm install --frozen-lockfile && pnpm test`. **Accept:** clean standalone install compiles and tests. **Rollback:** revert manifest/lock changes. **Commit:** `build: restore standalone keeper verification`. **Done:** tests pass and commit exists.

### 1.6 Make contract verification self-describing
**Purpose:** separate missing tools/submodules from Solidity failures. **Problem:** Foundry is unavailable locally. **Targets/files:** `scripts/ci-local.sh`, `packages/contracts/README.md` (new), `.github/workflows/ci.yml`. **Implement:** preflight `forge` and submodule state, emit explicit skip/fail rules, retain CI test. **Depends:** 1.2. **Tests:** shell preflight paths. **Verify:** `git submodule status && bash scripts/ci-local.sh --contracts-preflight`. **Accept:** missing Foundry is reported, never mislabeled success. **Rollback:** revert wrapper/docs. **Commit:** `build: clarify contract test prerequisites`. **Done:** preflight is deterministic and committed.

### 1.7 Inventory configuration and side effects
**Purpose:** make safe defaults machine-checkable. **Problem:** route/config behavior is distributed. **Targets/files:** `.env.example`, `services/api/afterbells/config.py`, `services/api/tests/test_config.py` (new), `docs/deployment-guide.md`. **Implement:** classify required/secret/live/write settings and reject unsafe combinations at startup. **Depends:** 1.4. **Tests:** default, recorded, staging, invalid execution combinations. **Verify:** `pnpm test:api -k config`. **Accept:** defaults cannot sign, broadcast, or use recorded evidence for execution. **Rollback:** revert validation while retaining false default. **Commit:** `security: validate safe runtime configuration`. **Done:** negative tests pass and commit exists.

### 1.8 Document executable architecture truth
**Purpose:** align docs with traced code. **Problem:** README collapses two decision pipelines and historical claims. **Targets/files:** `docs/system-architecture.md`, `docs/codebase-summary.md`, `README.md`. **Implement:** map analysis versus execution flows, state/persistence boundaries, and confirmed discrepancies. **Depends:** 1.4. **Tests:** documentation links. **Verify:** `python3 scripts/check-doc-links.py`. **Accept:** every claimed component maps to inspected code; blocked capabilities are explicit. **Rollback:** revert docs. **Commit:** `docs: align architecture with verified runtime behavior`. **Done:** links pass and commit exists.

### 1.9 Add API contract snapshots
**Purpose:** preserve compatibility during refactors. **Problem:** route shapes are scattered across Python and TS. **Targets/files:** `services/api/tests/test_openapi_contract.py` (new), `apps/web/lib/api.ts`, `docs/api-contract.json` (new). **Implement:** normalize OpenAPI, assert critical route schemas, and document compatibility policy. **Depends:** 1.4. **Tests:** snapshot and TS fixture decoding. **Verify:** `pnpm test:api -k openapi && pnpm test:web`. **Accept:** incompatible schema drift fails visibly. **Rollback:** regenerate only for approved changes. **Commit:** `test: lock critical API contracts`. **Done:** cross-client contract checks pass.

### 1.10 Publish the verified baseline report
**Purpose:** close Gate A with current evidence. **Problem:** `docs/validation.md` is historical. **Targets/files:** `docs/validation.md`, `docs/project-roadmap.md`. **Implement:** record commands, environment, exit codes, skipped checks, known failures, and safe-mode proof. **Depends:** 1.1–1.9. **Tests:** link check and full available CI. **Verify:** `./scripts/ci-local.sh --skip-contracts`. **Accept:** results are dated, reproducible, and never overclaim unavailable checks. **Rollback:** revert report only. **Commit:** `docs: publish verified development baseline`. **Done:** Gate A evidence is committed.

# Phase 2 — Versioned Strategy and Lifecycle Domain

**Objective:** create authoritative, owned, immutable domain contracts. **Problems:** `EquityIntent` and global constitution are not a versioned strategy; lifecycle is incomplete. **Prerequisites:** Gate A. **Outcome:** strict strategy revisions and a tested analysis/execution state model. **Risks:** schema compatibility and unsafe default coercion. **Validation:** domain/property/API contract tests. **Exit gate:** every decision names exact versions; invalid/ambiguous instructions and transitions fail closed.

### 2.1 Define a versioned strategy schema
**Purpose:** make strategy identity stable. **Problem:** intent and constitution are disconnected mutable inputs. **Targets/files:** `domain/strategy.py` (new), `domain/models.py`, tests. **Implement:** schema version, strategy/revision IDs, owner ID, hashes, timestamps, DRAFT/APPROVED/SUPERSEDED/REVOKED states. **Depends:** 1.9. **Tests:** validation, canonical hash, immutability. **Verify:** `pnpm test:api -k strategy`. **Accept:** mutation creates a revision and changes its hash. **Rollback:** schema is additive. **Commit:** `feat: define versioned strategy specifications`. **Done:** model tests pass and commit exists.

### 2.2 Model typed strategy constraints
**Purpose:** replace opaque constraint dictionaries. **Problem:** critical limits can disappear into `dict[str, Any]`. **Targets/files:** `domain/strategy.py`, `services/intent.py`, tests. **Implement:** typed assets, platforms, chains, notional, concentration, premium, freshness, depth, slippage, schedule, approval, expiry. **Depends:** 2.1. **Tests:** bounds, decimals, contradictions, unknown fields. **Verify:** `pnpm test:api -k 'strategy and constraint'`. **Accept:** unsupported critical fields reject; system ceilings remain stricter. **Rollback:** retain legacy adapter temporarily. **Commit:** `feat: type strategy risk constraints`. **Done:** strict models and tests pass.

### 2.3 Version the constrained intent parser
**Purpose:** make interpretation reproducible. **Problem:** parser behavior has no explicit version. **Targets/files:** `services/intent.py`, `domain/strategy.py`, tests. **Implement:** parser version, structured diagnostics, confidence-free deterministic outcomes, unsupported/ambiguous instruction list. **Depends:** 2.2. **Tests:** BUY grammar, SELL rejection, multiple tickers, contradictions, prompt-like junk. **Verify:** `pnpm test:api -k intent`. **Accept:** no critical text is silently ignored and identical text/version is stable. **Rollback:** keep v1 decoder. **Commit:** `feat: version deterministic intent interpretation`. **Done:** golden cases pass.

### 2.4 Separate system ceilings from user policy
**Purpose:** enforce non-overridable limits. **Problem:** constraints are merged in multiple services. **Targets/files:** `policy/schema.py`, `policy/compiler.py`, `config.py`, tests. **Implement:** explicit `SystemCeilings`, monotonic merge, provenance per effective rule. **Depends:** 2.2. **Tests:** attempts to loosen every ceiling. **Verify:** `pnpm test:api -k 'ceiling or compiler'`. **Accept:** effective values can only become stricter. **Rollback:** preserve existing hard settings. **Commit:** `security: enforce monotonic system ceilings`. **Done:** bypass tests fail closed.

### 2.5 Define the complete lifecycle vocabulary
**Purpose:** prevent optimistic state labels. **Problem:** `ExecState` lacks required states and mixes analysis/execution. **Targets/files:** `domain/models.py`, `services/state_machine.py`, docs. **Implement:** OBSERVED, EVALUATED, SIMULATED, AWAITING_APPROVAL, APPROVED, SUBMITTED, CONFIRMED plus BLOCKED, REJECTED, FAILED, CANCELLED, EXPIRED, UNKNOWN_SUBMISSION_OUTCOME, RECONCILING. **Depends:** 2.1. **Tests:** serialization and terminal-state classification. **Verify:** `pnpm test:api -k state_machine`. **Accept:** each state has semantics/actor/evidence requirements. **Rollback:** compatibility mapping for old receipts. **Commit:** `feat: define authoritative workflow lifecycle`. **Done:** model/docs/tests agree.

### 2.6 Enforce lifecycle transitions
**Purpose:** reject impossible jumps. **Problem:** transition enforcement covers only old states. **Targets/files:** `services/state_machine.py`, `services/pipeline.py`, tests. **Implement:** explicit transition table, reason/evidence/timestamp, compare-and-set hook, analysis terminal paths. **Depends:** 2.5. **Tests:** every allowed edge and representative forbidden edges. **Verify:** `pnpm test:api -k state_machine`. **Accept:** CONFIRMED cannot follow quote/submit without required evidence. **Rollback:** adapter maps legacy states read-only. **Commit:** `feat: enforce evidence-backed state transitions`. **Done:** transition suite passes.

### 2.7 Add strategy compiler diagnostics
**Purpose:** give actionable blocking explanations. **Problem:** constitution compiler reports do not cover strategy revision semantics. **Targets/files:** `policy/compiler.py`, `domain/strategy.py`, API models, tests. **Implement:** errors/warnings with source spans, rule IDs, severity, unsupported constructs. **Depends:** 2.3–2.4. **Tests:** malformed and contradictory inputs. **Verify:** `pnpm test:api -k compiler`. **Accept:** approval is impossible with blocking diagnostics. **Rollback:** retain raw text and compiler version. **Commit:** `feat: report strategy compilation diagnostics`. **Done:** diagnostics are stable and tested.

### 2.8 Add strategy compatibility adapters
**Purpose:** avoid breaking current decision clients. **Problem:** existing requests send text plus `DecisionPolicy`. **Targets/files:** `services/decisions.py`, `api/main.py`, `apps/web/lib/decisions.ts`, tests. **Implement:** translate legacy requests to an ephemeral v1 revision and return deprecation metadata. **Depends:** 2.1–2.7. **Tests:** old/new payload equivalence. **Verify:** `pnpm test:api -k decision && pnpm test:web`. **Accept:** current UI works while new responses expose revision/version. **Rollback:** remove new route, not old contract. **Commit:** `feat: bridge legacy decisions to strategy revisions`. **Done:** contract tests pass.

### 2.9 Add shared financial boundary vectors
**Purpose:** prove decimal and units safety. **Problem:** precision cases are local. **Targets/files:** `fixtures/contracts/financial-vectors.json` (new), `domain/mathx.py`, Python/TS tests. **Implement:** vectors for decimals, multipliers, rounding, min/max, invalid/non-finite inputs. **Depends:** 2.2. **Tests:** consume identical vectors in both stacks. **Verify:** `pnpm test:api -k math && pnpm test:web`. **Accept:** no binary-float financial path and results match exactly. **Rollback:** additive fixtures. **Commit:** `test: share financial precision boundary vectors`. **Done:** both implementations pass.

### 2.10 Expose minimal strategy API contracts
**Purpose:** make the domain usable before persistence. **Problem:** only constitution endpoints exist. **Targets/files:** `api/main.py`, `domain/strategy.py`, `apps/web/lib/api.ts`, tests/OpenAPI. **Implement:** validate/compile endpoints with no durable mutation yet; return exact revisions and diagnostics. **Depends:** 2.1–2.9. **Tests:** success/invalid/version negotiation. **Verify:** `pnpm test:api -k strategy && pnpm test:web`. **Accept:** clients can preview a complete immutable strategy contract. **Rollback:** remove additive endpoints. **Commit:** `feat: expose strategy validation contracts`. **Done:** Phase 2 gate passes.

# Phase 3 — Binance Evidence and Provider Reliability

**Objective:** make data adapters authenticated where documented, resilient, provenance-rich, and honest. **Problems:** current public `bapi` differs from official signed REST; timestamps/depth are insufficient. **Prerequisites:** typed strategy. **Outcome:** selected provider surfaces behind stable interfaces with fixture/live separation. **Risks:** credentials, rate limits, schema drift, jurisdiction. **Validation:** contract fixtures, fault injection, optional credentialed smoke. **Exit gate:** every observation carries provenance/freshness semantics; unsupported evidence stays unknown.

### 3.1 Define normalized provider observation contracts
**Purpose:** stop raw payload leakage. **Problem:** provenance is mostly string labels. **Targets/files:** `providers/contracts.py` (new), `domain/models.py`, tests. **Implement:** provider, endpoint/version, observed/retrieved times, direct/peer/inferred class, digest, chain/asset identity. **Depends:** 2.2. **Tests:** strict parsing and unknown timestamps. **Verify:** `pnpm test:api -k provider_contract`. **Accept:** retrieval time cannot masquerade as observation time. **Rollback:** additive adapter fields. **Commit:** `feat: define normalized provider evidence contracts`. **Done:** contracts pass.

### 3.2 Introduce a provider transport interface
**Purpose:** isolate HTTP/auth/retry behavior. **Problem:** synchronous global `httpx.get` is embedded in one client. **Targets/files:** `providers/transport.py` (new), `providers/binance_public.py`, tests. **Implement:** injected client, deadlines, cancellation, bounded retries with jitter, Retry-After, correlation IDs. **Depends:** 3.1. **Tests:** timeout, 429, 5xx, malformed JSON. **Verify:** `pnpm test:api -k provider`. **Accept:** retries are bounded and POST safety is explicit. **Rollback:** retain public adapter behind interface. **Commit:** `refactor: isolate provider transport behavior`. **Done:** fault tests pass.

### 3.3 Implement Binance Web3 request signing
**Purpose:** support documented APIs safely. **Problem:** no `/build` HMAC client exists. **Targets/files:** `providers/binance_web3.py` (new), `config.py`, `.env.example`, tests. **Implement:** exact raw path/body signing, ISO timestamp, nonce, receive window, secret redaction. **Depends:** 3.2. **Tests:** official-style golden signatures and replay/time-window errors. **Verify:** `pnpm test:api -k binance_web3_signing`. **Accept:** `/build` is signed exactly; secrets never enter logs. **Rollback:** feature flag off, remove credentials. **Commit:** `feat: sign authenticated Binance Web3 requests`. **Done:** offline vectors pass.

### 3.4 Add official RWA discovery adapter
**Purpose:** consume documented issuance/token/underlying endpoints. **Problem:** discovery depends on public `bapi` shapes. **Targets/files:** `providers/binance_web3.py`, `services/graph.py`, fixtures/tests. **Implement:** pagination, BSC filtering, platform/address/multiplier mapping, business-code validation. **Depends:** 3.3. **Tests:** Ondo/bStock, empty pages, duplicates, schema drift. **Verify:** `pnpm test:api -k rwa`. **Accept:** normalized results are equivalent or discrepancies are explicit. **Rollback:** choose legacy adapter through config. **Commit:** `feat: add official Binance RWA discovery adapter`. **Done:** fixture contracts pass; live remains credential-gated.

### 3.5 Preserve xStocks with explicit provenance
**Purpose:** avoid losing supported coverage where official catalog differs. **Problem:** current type mapping includes xStocks while current official platform examples may not. **Targets/files:** `services/graph.py`, `providers/binance_public.py`, integration matrix/tests. **Implement:** provider capability registry and source-specific xStocks path; never claim official signed coverage without proof. **Depends:** 3.4. **Tests:** mixed adapter dedupe/provenance. **Verify:** `pnpm test:api -k graph`. **Accept:** xStocks is either sourced/labeled or unavailable, never fabricated. **Rollback:** disable capability flag. **Commit:** `fix: preserve source-specific xStocks provenance`. **Done:** coverage behavior is explicit.

### 3.6 Normalize freshness and market sessions
**Purpose:** support defensible stale-data policy. **Problem:** reference timestamp is forcibly unknown and session data varies. **Targets/files:** `domain/models.py`, `services/graph.py`, providers/tests. **Implement:** source-observed versus retrieval timestamps, clock skew, freshness status, market timezone/session. **Depends:** 3.1,3.4. **Tests:** stale, future, absent, conflicting timestamps. **Verify:** `pnpm test:api -k freshness`. **Accept:** freshness rules fail closed on unknown/conflict. **Rollback:** retain UNKNOWN. **Commit:** `feat: normalize market evidence freshness`. **Done:** timestamp tests pass.

### 3.7 Add size-specific quote contracts
**Purpose:** distinguish executable depth from volume. **Problem:** liquidity policy always fails and quote shape is incomplete. **Targets/files:** `domain/models.py`, `providers/contracts.py`, fixtures/tests. **Implement:** route, amount/units, output/min output, fees, impact, expiry, target, calldata hash availability. **Depends:** 3.1. **Tests:** size mismatch, stale quote, missing fee/impact. **Verify:** `pnpm test:api -k executable_quote`. **Accept:** cap/volume cannot satisfy depth. **Rollback:** quote stays optional. **Commit:** `feat: model size-specific executable quotes`. **Done:** contract tests pass.

### 3.8 Add quote/build and simulation adapters
**Purpose:** use only relevant Trading/Transaction APIs. **Problem:** Agentic Wallet quote is not bound to a transaction. **Targets/files:** `providers/binance_web3.py`, `providers/baw.py`, `providers/bsc_rpc.py`, tests. **Implement:** capability-based quote/build/simulate methods returning immutable envelope identity; do not submit. **Depends:** 3.3,3.7. **Tests:** envelope mutation, unavailable build, revert, chain mismatch. **Verify:** `pnpm test:api -k 'quote or simulation'`. **Accept:** unbound adapters report unsupported and block. **Rollback:** disable adapter. **Commit:** `feat: add bounded quote and simulation adapters`. **Done:** no broadcast path is exercised.

### 3.9 Add provider circuit breaking and cache semantics
**Purpose:** degrade safely during upstream failures. **Problem:** in-memory TTL cache has no stale/error semantics. **Targets/files:** `providers/transport.py`, `binance_public.py`, metrics/tests. **Implement:** per-operation circuit, bounded cache, explicit stale-if-error policy only for analysis, no cache for signed mutable calls. **Depends:** 3.2. **Tests:** stampede, open/half-open, stale labels. **Verify:** `pnpm test:api -k 'cache or circuit'`. **Accept:** outages cannot silently relabel stale data live. **Rollback:** disable circuit/cache. **Commit:** `feat: enforce provider cache and circuit semantics`. **Done:** concurrency/fault tests pass.

### 3.10 Establish credentialed integration verification
**Purpose:** separate fixture confidence from live proof. **Problem:** prior live claims use a different surface. **Targets/files:** `verify_live.py`, `.github/workflows/provider-smoke.yml` (new), docs. **Implement:** opt-in read-only smoke, permission precheck, redaction, no PR secrets, recorded schema diff artifacts. **Depends:** 3.3–3.9. **Tests:** dry run without credentials. **Verify:** `pnpm verify:live -- --dry-run`. **Accept:** missing credentials skip explicitly; successful run proves selected endpoints only. **Rollback:** remove optional workflow. **Commit:** `test: add credential-gated provider contract verification`. **Done:** Phase 3 fixture gate passes; live status is honest.

# Phase 4 — Deterministic Decisions and Route Analysis

**Objective:** unify policy and routing around versioned inputs. **Problems:** duplicate decision/pipeline evaluation, partial rules, unclear quote/reference distinction. **Prerequisites:** Phases 2–3. **Outcome:** one deterministic engine with explanations and stable ordering. **Risks:** behavior drift. **Validation:** golden/property/differential tests. **Exit gate:** identical versioned inputs produce equivalent results and missing evidence blocks dependent rules.

### 4.1 Define the evaluation input aggregate
**Purpose:** bind strategy, snapshot, policy, engine version. **Problem:** services accept loose objects. **Targets/files:** `services/evaluation.py` (new), `domain/models.py`, tests. **Implement:** immutable aggregate and hash. **Depends:** 2.10,3.6. **Tests:** mutation/hash and validation. **Verify:** `pnpm test:api -k evaluation`. **Accept:** every result cites exact inputs. **Rollback:** additive wrapper. **Commit:** `feat: bind deterministic evaluation inputs`. **Done:** tests pass and commit exists.

### 4.2 Consolidate duplicate policy evaluation
**Purpose:** remove divergent rule implementations. **Problem:** `decisions.py` and `policy/engine.py` evaluate overlapping checks. **Targets/files:** both modules, `services/evaluation.py`, tests. **Implement:** shared pure rule kernel with legacy adapters. **Depends:** 4.1. **Tests:** differential old/new fixtures. **Verify:** `pnpm test:api -k policy`. **Accept:** one rule ID has one implementation. **Rollback:** feature flag old path. **Commit:** `refactor: unify deterministic policy evaluation`. **Done:** equivalence tests pass.

### 4.3 Add typed rule outcomes
**Purpose:** make explanations machine-readable. **Problem:** PASS/FAIL strings omit evidence/unknown semantics. **Targets/files:** `domain/models.py`, `policy/engine.py`, TS types/tests. **Implement:** PASS/FAIL/UNKNOWN/NOT_APPLICABLE with rule version, observed/limit values, evidence IDs. **Depends:** 4.2. **Tests:** serialization and UI decoding. **Verify:** `pnpm test:api -k rule && pnpm test:web`. **Accept:** unknown is distinct and fails required rules. **Rollback:** compatibility serializer. **Commit:** `feat: type policy outcomes and evidence links`. **Done:** contracts pass.

### 4.4 Harden normalization and identity checks
**Purpose:** prevent cross-asset/unit mistakes. **Problem:** validation is incomplete across flows. **Targets/files:** `services/graph.py`, `domain/mathx.py`, evaluation tests. **Implement:** checksummed chain/address identity, decimals/multiplier bounds, ticker aliases, duplicate conflict handling. **Depends:** 3.4,4.1. **Tests:** zero/negative/huge multiplier, duplicates, wrong chain/ticker. **Verify:** `pnpm test:api -k 'graph or math'`. **Accept:** invalid representation cannot enter ranking. **Rollback:** reject rather than coerce. **Commit:** `fix: reject invalid representation identities and units`. **Done:** negative tests pass.

### 4.5 Implement freshness and data-quality rules
**Purpose:** operationalize evidence semantics. **Problem:** freshness is only a limitation string. **Targets/files:** `policy/schema.py`, `policy/engine.py`, tests. **Implement:** max age, unknown-source, conflict, direct-source requirements. **Depends:** 3.6,4.3. **Tests:** stale/unknown/future/peer cases. **Verify:** `pnpm test:api -k freshness`. **Accept:** required freshness never passes unknown data. **Rollback:** defaults preserve current permissive analysis with visible unknown. **Commit:** `feat: enforce evidence freshness policies`. **Done:** tests pass.

### 4.6 Implement executable-liquidity rules
**Purpose:** evaluate requested size honestly. **Problem:** market volume is displayed but not depth. **Targets/files:** `policy/schema.py`, `policy/engine.py`, `services/tournament.py`, tests. **Implement:** quote-size, expiry, min output, impact/fee limits. **Depends:** 3.7,4.3. **Tests:** volume-only, undersized, expired, expensive quotes. **Verify:** `pnpm test:api -k liquidity`. **Accept:** only matching size-specific evidence can pass. **Rollback:** leave rule UNKNOWN. **Commit:** `feat: evaluate executable liquidity evidence`. **Done:** rule tests pass.

### 4.7 Make route scoring deterministic
**Purpose:** ensure stable selection. **Problem:** score inputs and tie-breaking are incomplete. **Targets/files:** `services/tournament.py`, tests. **Implement:** versioned scoring, Decimal weights, total ordering, excluded-route separation. **Depends:** 4.3–4.6. **Tests:** permutation/property/tie vectors. **Verify:** `pnpm test:api -k tournament`. **Accept:** input order never changes outcome. **Rollback:** retain prior version for replay. **Commit:** `feat: version deterministic route scoring`. **Done:** property tests pass.

### 4.8 Add policy conflict analysis
**Purpose:** reject impossible strategies early. **Problem:** individually valid constraints can conflict. **Targets/files:** `policy/compiler.py`, `services/evaluation.py`, tests. **Implement:** empty issuer/chain intersection, incompatible session/depth/freshness, expiry/schedule conflicts. **Depends:** 2.7,4.5–4.6. **Tests:** conflict matrix. **Verify:** `pnpm test:api -k conflict`. **Accept:** contradictions block before provider calls where possible. **Rollback:** diagnostics only, no unsafe fallback. **Commit:** `feat: detect contradictory strategy policies`. **Done:** conflict tests pass.

### 4.9 Produce structured decision explanations
**Purpose:** make outcomes auditable without generated prose. **Problem:** free-form explanation can drift from checks. **Targets/files:** `services/evaluation.py`, `services/decisions.py`, web types/tests. **Implement:** selected/rejected reason graph derived from rules and evidence. **Depends:** 4.3,4.7. **Tests:** golden explanations and missing evidence. **Verify:** `pnpm test:api -k explanation && pnpm test:web`. **Accept:** every claim traces to rule/evidence. **Rollback:** retain compatibility summary. **Commit:** `feat: derive explainable decision reason graphs`. **Done:** golden tests pass.

### 4.10 Route all analysis interfaces through one engine
**Purpose:** guarantee REST/CLI/MCP/keeper parity. **Problem:** keeper and pipeline use separate paths. **Targets/files:** `api/main.py`, `cli.py`, `mcp_server.py`, `services/keeper.py`, tests. **Implement:** call evaluation service; keep MCP read-only. **Depends:** 4.1–4.9. **Tests:** same request equality across interfaces. **Verify:** `pnpm test:api -k 'mcp or interface_parity' && pnpm demo`. **Accept:** canonical outputs match aside from transport metadata. **Rollback:** adapters preserve endpoints. **Commit:** `refactor: unify analysis interfaces on one engine`. **Done:** Phase 4 gate passes.

# Phase 5 — Immutable Snapshots, Replay, and Receipts

**Objective:** make evidence portable, versioned, and tamper-evident. **Problems:** receipt schema is untyped, source digest is fixture-wide, compatibility is narrow. **Prerequisites:** Phase 4. **Outcome:** versioned snapshots/receipts with cross-language verification. **Risks:** hash breakage. **Validation:** shared golden vectors and tamper suites. **Exit gate / Gate B:** strategy-to-replay analysis works end to end with honest proof limits.

### 5.1 Version immutable market snapshots
**Purpose:** bind only used inputs. **Problem:** snapshot provenance is broad and mutable dictionaries remain. **Targets/files:** `domain/snapshots.py` (new), `services/decisions.py`, tests. **Implement:** schema/provider versions, capture ID/time, per-observation digest and stable ordering. **Depends:** 4.1. **Tests:** mutation, reorder, schema validation. **Verify:** `pnpm test:api -k snapshot`. **Accept:** same evidence canonicalizes identically. **Rollback:** v1 reader remains. **Commit:** `feat: version immutable market snapshots`. **Done:** snapshot tests pass.

### 5.2 Scope recorded-fixture provenance
**Purpose:** prove exactly which files contributed. **Problem:** current digest hashes every RWA fixture. **Targets/files:** `services/decisions.py`, `providers/binance_public.py`, fixtures/tests. **Implement:** collect used fixture names/digests and dataset manifest. **Depends:** 5.1. **Tests:** unrelated fixture changes do not alter receipt. **Verify:** `pnpm test:api -k provenance`. **Accept:** digest changes iff consumed evidence changes. **Rollback:** retain legacy digest reader. **Commit:** `fix: scope fixture provenance to consumed evidence`. **Done:** provenance tests pass.

### 5.3 Define typed receipt envelopes
**Purpose:** stop schema drift. **Problem:** receipts are intentionally untyped dictionaries. **Targets/files:** `domain/receipts.py` (new), `services/receipts.py`, tests. **Implement:** envelope version/kind, subject IDs, hashes, evidence, transitions, proof claims. **Depends:** 5.1. **Tests:** strict version/kind and round trip. **Verify:** `pnpm test:api -k receipt`. **Accept:** unknown versions fail explicitly; old v1 reads. **Rollback:** dual serializer. **Commit:** `feat: define versioned receipt envelopes`. **Done:** compatibility tests pass.

### 5.4 Publish canonical JSON specification
**Purpose:** keep Python/TS/contract commitments aligned. **Problem:** edge semantics are implicit. **Targets/files:** `docs/canonical-json.md` (new), `policy/schema.py`, `apps/web/lib/canonical.ts`. **Implement:** key order, Unicode ASCII escaping, decimal representation, absent/null, excluded hash field. **Depends:** 5.3. **Tests:** normative vectors. **Verify:** `pnpm test:api -k canonical && pnpm test:web`. **Accept:** both stacks emit identical bytes/digests. **Rollback:** version algorithm, never mutate v1. **Commit:** `docs: specify canonical receipt serialization`. **Done:** vectors pass.

### 5.5 Add shared canonicalization vectors
**Purpose:** catch cross-language regressions. **Problem:** tests are separate. **Targets/files:** `fixtures/contracts/canonical-vectors.json` (new), Python/TS tests. **Implement:** Unicode, nested ordering, decimals-as-strings, null, tampering cases. **Depends:** 5.4. **Tests:** shared consumer suites. **Verify:** `pnpm test:api -k canonical && pnpm test:web`. **Accept:** all digests match expected values. **Rollback:** additive vectors. **Commit:** `test: share canonical receipt hash vectors`. **Done:** both suites pass.

### 5.6 Bind decisions to strategy and engine versions
**Purpose:** make replay historically meaningful. **Problem:** receipt contains policy but not full strategy/compiler/engine identity. **Targets/files:** `services/evaluation.py`, `domain/receipts.py`, tests. **Implement:** include revision/hash and parser/compiler/policy/scoring versions. **Depends:** 2.1,4.7,5.3. **Tests:** version mutation and missing version. **Verify:** `pnpm test:api -k replay`. **Accept:** replay selects exact compatible engine or reports unsupported. **Rollback:** v1 compatibility path. **Commit:** `feat: bind receipts to strategy and engine versions`. **Done:** version tests pass.

### 5.7 Harden offline replay
**Purpose:** guarantee no provider/wallet side effects. **Problem:** replay behavior is pure today but not structurally isolated. **Targets/files:** `services/decisions.py`, `services/replay.py` (new), tests. **Implement:** dependency-free replay package, resource limits, strict snapshot validation. **Depends:** 5.6. **Tests:** network-call trap, oversized/malformed receipts. **Verify:** `pnpm test:api -k replay`. **Accept:** replay performs zero I/O and fails boundedly. **Rollback:** keep old entry adapter. **Commit:** `security: isolate bounded offline receipt replay`. **Done:** isolation tests pass.

### 5.8 Add receipt authenticity vocabulary
**Purpose:** prevent hash overclaims. **Problem:** integrity/authenticity/execution proof can be conflated. **Targets/files:** `domain/receipts.py`, API/web components, docs/tests. **Implement:** separate integrity, issuer/source attestation, notarization, transaction evidence statuses. **Depends:** 5.3. **Tests:** rendering/serialization combinations. **Verify:** `pnpm test:api -k authenticity && pnpm test:web`. **Accept:** a valid hash never implies trade/source truth. **Rollback:** compatibility note remains. **Commit:** `feat: separate receipt integrity from authenticity`. **Done:** tests and labels agree.

### 5.9 Harden receipt registry semantics
**Purpose:** document what on-chain notarization proves. **Problem:** contract stores digest but lifecycle/duplicate semantics need alignment. **Targets/files:** Solidity contract/tests, `services/receipts.py`, docs. **Implement:** verify duplicate/zero hash/event/access behavior; change contract only if evidence requires. **Depends:** 5.3–5.5. **Tests:** Foundry plus Python commitment vectors. **Verify:** `pnpm test:contracts && pnpm test:api -k receipt`. **Accept:** registry is non-custodial integrity commitment only. **Rollback:** no deployment; revert source. **Commit:** `test: harden receipt registry proof semantics`. **Done:** tests pass where Foundry is available.

### 5.10 Complete analysis receipt workflow
**Purpose:** prove Gate B across supported entry points. **Problem:** new strategy/snapshot schemas are not yet wired end to end. **Targets/files:** API/CLI/MCP/web minimal wiring and E2E tests. **Implement:** create, compare same snapshot, export, replay, tamper failure. **Depends:** 5.1–5.9. **Tests:** browser/API/CLI/MCP journey. **Verify:** `pnpm demo && pnpm test:api && pnpm test:web && pnpm test:e2e`. **Accept:** supported workflow passes; unsupported depth/freshness remains blocked. **Rollback:** legacy routes remain during migration. **Commit:** `test: verify the complete analysis receipt workflow`. **Done:** Gate B evidence exists.

# Phase 6 — Durable Agent Orchestration and Jobs

**Objective:** replace inline tasks with recoverable, bounded work. **Problems:** tasks acknowledge only after execution; no leases/retries/cancellation/idempotency. **Prerequisites:** receipt workflow. **Outcome:** persistent jobs and auditable tool actions. **Risks:** duplicate side effects and stuck leases. **Validation:** crash/concurrency/clock tests. **Exit gate:** jobs survive restart and cannot bypass analysis authority.

### 6.1 Define job and event schemas
**Purpose:** establish durable orchestration contracts. **Problem:** SQLite task rows are minimal. **Targets/files:** `domain/jobs.py` (new), persistence/tests. **Implement:** states, attempts, deadlines, correlation, actor/tenant, input/output digest, cancellation. **Depends:** 2.5,5.3. **Tests:** transitions and serialization. **Verify:** `pnpm test:api -k job`. **Accept:** terminal and retryable states are explicit. **Rollback:** additive tables. **Commit:** `feat: define durable job and event contracts`. **Done:** schema tests pass.

### 6.2 Add persisted enqueue semantics
**Purpose:** acknowledge only durable work. **Problem:** endpoint runs tasks inline. **Targets/files:** `services/jobs.py` (new), `api/main.py`, persistence/tests. **Implement:** transactional enqueue before 202 response. **Depends:** 6.1. **Tests:** DB failure and acknowledged-job existence. **Verify:** `pnpm test:api -k enqueue`. **Accept:** every accepted ID is queryable after restart. **Rollback:** disable async route, preserve records. **Commit:** `feat: persist jobs before acceptance`. **Done:** tests pass.

### 6.3 Implement worker leases
**Purpose:** support safe concurrency/recovery. **Problem:** no atomic claims. **Targets/files:** `services/worker.py` (new), persistence/tests. **Implement:** atomic claim, owner, heartbeat, expiry, fenced completion. **Depends:** 6.2. **Tests:** competing workers and expired lease. **Verify:** `pnpm test:api -k lease`. **Accept:** one active owner; stale worker cannot commit. **Rollback:** run one worker. **Commit:** `feat: add recoverable worker leases`. **Done:** concurrency tests pass.

### 6.4 Classify bounded retries
**Purpose:** retry only safe failures. **Problem:** provider/task exceptions lack orchestration policy. **Targets/files:** `services/jobs.py`, `providers/errors.py`, tests. **Implement:** retryable taxonomy, exponential backoff/jitter, max attempts, dead letter. **Depends:** 6.3. **Tests:** 429/timeout/schema/user error. **Verify:** `pnpm test:api -k retry`. **Accept:** validation errors never retry; exhaustion is visible. **Rollback:** max attempts one. **Commit:** `feat: bound and classify job retries`. **Done:** deterministic clock tests pass.

### 6.5 Add durable idempotency keys
**Purpose:** prevent duplicate logical work. **Problem:** random task IDs make retries duplicate. **Targets/files:** job schema/persistence/API/tests. **Implement:** tenant+operation+key uniqueness, request digest conflict, stored response replay. **Depends:** 6.2. **Tests:** sequential/concurrent duplicates and key misuse. **Verify:** `pnpm test:api -k idempotency`. **Accept:** duplicates yield one job; different payload conflicts. **Rollback:** retain keys/data. **Commit:** `feat: enforce durable job idempotency`. **Done:** race tests pass.

### 6.6 Add cancellation and deadlines
**Purpose:** bound resource use and user control. **Problem:** no cancellation semantics. **Targets/files:** `services/jobs.py`, `worker.py`, API/tests. **Implement:** pending/in-flight cancel, cooperative token, provider deadline, terminal reason. **Depends:** 6.3. **Tests:** pre-start/in-flight/race/expired. **Verify:** `pnpm test:api -k 'cancel or deadline'`. **Accept:** cancelled jobs cannot later succeed. **Rollback:** disable cancel endpoint. **Commit:** `feat: enforce job cancellation and deadlines`. **Done:** race tests pass.

### 6.7 Persist auditable tool invocations
**Purpose:** trace agent actions safely. **Problem:** DX JSONL is not authoritative audit state. **Targets/files:** `domain/jobs.py`, `services/audit.py` (new), MCP/keeper adapters/tests. **Implement:** tool/version/input digest/output digest/timing/result with redaction. **Depends:** 6.1. **Tests:** redaction and append-only ordering. **Verify:** `pnpm test:api -k audit`. **Accept:** secrets/raw credentials never persist. **Rollback:** disable display, preserve audit records. **Commit:** `feat: audit bounded agent tool invocations`. **Done:** tests pass.

### 6.8 Constrain keeper task capabilities
**Purpose:** keep automation analysis-only. **Problem:** task kinds include prepare operations and global policy reads. **Targets/files:** `services/keeper.py`, keeper TS tools, tests. **Implement:** capability allowlist, typed schemas, notional/rate limits, explicit no signing/trading. **Depends:** 4.10,6.7. **Tests:** unsupported/tool-confusion/oversized payloads. **Verify:** `pnpm test:api -k keeper && cd services/keeper/agent && pnpm test`. **Accept:** keeper cannot reach wallet mutation. **Rollback:** reduce allowlist. **Commit:** `security: constrain keeper task capabilities`. **Done:** boundary tests pass.

### 6.9 Recover interrupted jobs at startup
**Purpose:** survive worker crashes. **Problem:** no restart reconciliation. **Targets/files:** `services/worker.py`, lifespan wiring, tests. **Implement:** reclaim expired leases, expire deadlines, preserve unknown side-effect outcomes. **Depends:** 6.3–6.6. **Tests:** kill/restart fixtures. **Verify:** `pnpm test:api -k recovery`. **Accept:** analysis resumes safely; side-effect jobs never blindly repeat. **Rollback:** quarantine interrupted jobs. **Commit:** `fix: recover interrupted orchestration jobs safely`. **Done:** restart tests pass.

### 6.10 Expose job status and event APIs
**Purpose:** connect orchestration functionally. **Problem:** current list endpoint lacks ownership/events/control. **Targets/files:** `api/main.py`, `apps/web/lib/api.ts`, minimal agent page, tests. **Implement:** create/get/list/events/cancel with polling semantics. **Depends:** 6.2–6.9. **Tests:** contract and minimal E2E. **Verify:** `pnpm test:api -k job && pnpm test:web`. **Accept:** state derives from persisted events and errors are visible. **Rollback:** hide new routes. **Commit:** `feat: expose durable orchestration workflows`. **Done:** Phase 6 gate passes.

# Phase 7 — Persistence, Authentication, Authorization, and API Completeness

**Objective:** deliver a secure multi-user analysis beta. **Problems:** global SQLite, no migrations/identity/ownership/rate limits. **Prerequisites:** Phases 2 and 6. **Outcome:** transactional PostgreSQL-ready storage and protected owned resources. **Risks:** lockout, IDOR, migration/data loss. **Validation:** migration, concurrency, permitted/forbidden API tests. **Exit gate / Gate C:** identity and tenancy are enforced, durable workflows survive restart, execution stays disabled.

### 7.1 Select and document the identity boundary
**Purpose:** make an explicit deployment-compatible auth choice. **Problem:** no identity model exists. **Targets/files:** `docs/adr/identity.md` (new), `config.py`, threat model. **Implement:** OIDC/OAuth2 session/token flows, issuer/audience, service identities, CSRF choice, local test identity. **Depends:** 1.7. **Tests:** configuration validation plan. **Verify:** `python3 scripts/check-doc-links.py`. **Accept:** browser/API/worker trust boundaries and revocation are specified. **Rollback:** documentation-only. **Commit:** `docs: define authentication and identity boundaries`. **Done:** ADR approved before auth code.

### 7.2 Add request authentication middleware
**Purpose:** establish verified actors. **Problem:** all routes are anonymous. **Targets/files:** `api/auth.py` (new), `api/main.py`, config/tests. **Implement:** validate signature/issuer/audience/expiry, principal context, secure error handling; retain explicit public health/read-only demo routes. **Depends:** 7.1. **Tests:** valid, missing, expired, wrong issuer/audience. **Verify:** `pnpm test:api -k auth`. **Accept:** protected mutations reject anonymous callers. **Rollback:** analysis demo flag only; never expose mutations. **Commit:** `security: authenticate protected API requests`. **Done:** auth matrix passes.

### 7.3 Define tenant and ownership models
**Purpose:** isolate all user data. **Problem:** constitutions/tasks/receipts are global. **Targets/files:** `domain/identity.py` (new), strategy/job/receipt models, persistence/tests. **Implement:** tenant, user, roles, resource owner, service actor, admin scope. **Depends:** 7.2. **Tests:** ownership validation. **Verify:** `pnpm test:api -k ownership`. **Accept:** every durable resource has non-null tenant/owner. **Rollback:** migration remains reversible before production. **Commit:** `feat: model tenant-owned application resources`. **Done:** schema tests pass.

### 7.4 Introduce migrations and repository interfaces
**Purpose:** replace implicit `CREATE TABLE` startup logic. **Problem:** SQLite schema has no migration history. **Targets/files:** `persistence/` package, migration config/files, pyproject/tests. **Implement:** repository/UoW interfaces, numbered migrations, SQLite test and PostgreSQL production dialect. **Depends:** 7.3. **Tests:** clean install and upgrade snapshot. **Verify:** `pnpm test:api -k migration`. **Accept:** upgrade preserves existing records and ownership backfill is explicit. **Rollback:** tested downgrade or restore procedure. **Commit:** `feat: add versioned database migrations`. **Done:** migration tests pass.

### 7.5 Implement transactional production persistence
**Purpose:** support concurrency/durability. **Problem:** synchronous local SQLite is insufficient for multi-user service. **Targets/files:** repositories/config/compose/tests. **Implement:** PostgreSQL connection management, FK/unique/check constraints, indexes, atomic state updates; keep SQLite for local tests. **Depends:** 7.4. **Tests:** both dialects and concurrent updates. **Verify:** `pnpm test:api -k persistence && docker compose config`. **Accept:** production rejects ephemeral SQLite and lifecycle writes are atomic. **Rollback:** local SQLite remains supported. **Commit:** `feat: add transactional production persistence`. **Done:** dialect tests pass.

### 7.6 Persist owned strategy revisions
**Purpose:** make Strategy Studio functional. **Problem:** Phase 2 only validates contracts. **Targets/files:** strategy repository/API routes/TS client/tests. **Implement:** create/list/get/revise/approve/revoke with optimistic concurrency and immutable approved revisions. **Depends:** 2.10,7.5. **Tests:** lifecycle, stale update, ownership. **Verify:** `pnpm test:api -k strategy_api && pnpm test:web`. **Accept:** old decisions retain exact revision; edits cannot mutate it. **Rollback:** disable routes, preserve data. **Commit:** `feat: persist owned strategy revisions`. **Done:** API tests pass.

### 7.7 Enforce object-level authorization
**Purpose:** prevent cross-user access. **Problem:** receipt/task IDs are enumerable to any caller. **Targets/files:** `api/authz.py` (new), all resource routes/repositories/tests. **Implement:** centralized action/resource policy and tenant filters. **Depends:** 7.3,7.6. **Tests:** IDOR matrix for strategies/jobs/receipts/approvals. **Verify:** `pnpm test:api -k authorization`. **Accept:** forbidden reads/mutations return non-leaking responses. **Rollback:** fail closed. **Commit:** `security: enforce object-level authorization`. **Done:** negative matrix passes.

### 7.8 Add CSRF, CORS, and rate-limit controls
**Purpose:** protect browser and abuse surfaces. **Problem:** broad methods/headers and no throttling. **Targets/files:** `api/main.py`, auth/config, middleware/tests. **Implement:** exact origins/methods, CSRF for cookie mode, actor/IP endpoint budgets, trusted proxies. **Depends:** 7.2. **Tests:** hostile origin, CSRF missing, 429/retry metadata. **Verify:** `pnpm test:api -k 'csrf or cors or rate'`. **Accept:** legitimate clients work; bypass attempts fail. **Rollback:** stricter defaults remain. **Commit:** `security: enforce API browser and abuse controls`. **Done:** security tests pass.

### 7.9 Protect diagnostics and redact errors
**Purpose:** avoid sensitive operational leakage. **Problem:** config/DX/task errors can expose internals. **Targets/files:** `api/main.py`, `dx/recorder.py`, logging/tests. **Implement:** public minimal health, privileged diagnostics, stable problem details, recursive redaction. **Depends:** 7.2,6.7. **Tests:** secret/path/traceback leakage. **Verify:** `pnpm test:api -k redaction`. **Accept:** external errors are actionable but contain no secret/internal path. **Rollback:** reduce public fields. **Commit:** `security: protect diagnostics and redact API errors`. **Done:** leakage tests pass.

### 7.10 Wire authenticated functional frontend flows
**Purpose:** prove secure beta usability without redesign. **Problem:** current pages assume anonymous/global endpoints. **Targets/files:** `apps/web/app/providers.tsx`, API client, strategy/receipt/agent pages, E2E tests. **Implement:** session propagation, owned lists, accessible auth/error/expiry states, no visual overhaul. **Depends:** 7.2–7.9. **Tests:** login, ownership, expiry, forbidden action. **Verify:** `pnpm test:web && pnpm test:e2e`. **Accept:** critical analysis workflows use protected APIs end to end. **Rollback:** disable authenticated deployment, not authorization. **Commit:** `feat: connect authenticated analysis workflows`. **Done:** Gate C passes.

# Phase 8 — Controlled Execution Boundary (Optional and Strictly Gated)

**Objective:** design and verify transaction safety without enabling mainnet. **Problems:** unbound wallet quote/swap, Boolean confirmation, no signer isolation/idempotency/reconciliation. **Prerequisites:** Phases 3,6,7 plus explicit product authorization. **Outcome:** controlled-environment execution lifecycle. **Risks:** financial loss, replay, chain reorg, regulatory limits. **Validation:** adversarial tests and testnet-only evidence. **Exit gate / Gate D:** all ten steps plus independent security/legal review; otherwise `EXECUTION_ENABLED=false` and status `BLOCKED`.

### 8.1 Create canonical immutable execution plans
**Purpose:** establish one transaction identity. **Problem:** quote and swap inputs can diverge. **Targets/files:** `domain/execution.py` (new), canonical vectors/tests. **Implement:** chain, actor/wallet, tokens, atomic amount, min output, slippage, target, calldata/envelope hash, value, deadline, quote/strategy/policy hashes, nonce. **Depends:** 3.8,5.4. **Tests:** mutate every field. **Verify:** `pnpm test:api -k execution_plan`. **Accept:** any critical mutation changes plan hash. **Rollback:** plans remain inert. **Commit:** `feat: define immutable execution plans`. **Done:** plan vectors pass.

### 8.2 Bind exact-envelope simulation
**Purpose:** prove submitted bytes were simulated. **Problem:** current simulation always blocks due to absent calldata. **Targets/files:** `services/pipeline.py`, provider adapters, execution models/tests. **Implement:** simulate exact chain/to/data/value/from/block context; persist envelope hash/result/expiry. **Depends:** 8.1. **Tests:** changed byte/recipient/value/chain, revert/unavailable. **Verify:** `pnpm test:api -k bound_simulation`. **Accept:** unavailable or mismatched simulation blocks. **Rollback:** blocker remains. **Commit:** `security: bind simulation to exact transaction envelopes`. **Done:** adversarial tests pass.

### 8.3 Replace Boolean confirmation with approvals
**Purpose:** require action-specific authorization. **Problem:** `confirm=true` is unauthenticated. **Targets/files:** execution/approval models, API/repository/tests. **Implement:** actor/wallet/plan/policy/chain-bound approval, expiry, one-time nonce, revocation, audit. **Depends:** 7.7,8.1. **Tests:** wrong actor/wallet/plan, expired/revoked/replay. **Verify:** `pnpm test:api -k approval`. **Accept:** Boolean alone has no authority. **Rollback:** execution remains blocked. **Commit:** `security: require plan-specific execution approvals`. **Done:** bypass suite passes.

### 8.4 Isolate the signing service boundary
**Purpose:** keep keys from API/agents/browser. **Problem:** general pipeline can invoke wallet CLI. **Targets/files:** `services/signer/` (new), deployment config, keeper/MCP tests/docs. **Implement:** narrow authenticated signer accepts only validated approved plans; KMS/HSM interface; key rotation/access logs. **Depends:** 8.3. **Tests:** unauthorized caller/tool, mutated plan, key unavailable. **Verify:** `pnpm test:api -k signer`. **Accept:** analysis processes possess no signing credential. **Rollback:** signer undeployed and kill switch off. **Commit:** `security: isolate transaction signing authority`. **Done:** trust-boundary tests pass.

### 8.5 Persist idempotent submission attempts
**Purpose:** prevent duplicate financial actions. **Problem:** no durable submission record. **Targets/files:** execution repository/worker/tests. **Implement:** plan+nonce uniqueness, pre-submit record, signed hash, provider/order/tx IDs, atomic claims. **Depends:** 6.5,8.4. **Tests:** concurrent duplicate and crash before/after broadcast. **Verify:** `pnpm test:api -k submission_idempotency`. **Accept:** one plan produces at most one accepted envelope. **Rollback:** quarantine attempts, never resubmit. **Commit:** `security: make transaction submission idempotent`. **Done:** concurrency tests pass.

### 8.6 Handle uncertain submission outcomes
**Purpose:** avoid dangerous retry after timeout. **Problem:** wallet errors cannot distinguish rejected from broadcast. **Targets/files:** execution service/state machine/providers/tests. **Implement:** UNKNOWN_SUBMISSION_OUTCOME, lookup by signed hash/nonce/order, reconciliation job before retry. **Depends:** 8.5. **Tests:** timeout after broadcast, provider loss, nonce conflict. **Verify:** `pnpm test:api -k unknown_submission`. **Accept:** unknown never becomes failed/retried optimistically. **Rollback:** manual quarantine. **Commit:** `fix: reconcile uncertain transaction submissions`. **Done:** fault tests pass.

### 8.7 Verify authoritative chain receipts
**Purpose:** distinguish submitted from confirmed. **Problem:** order response may be treated as sufficient. **Targets/files:** `providers/bsc_rpc.py`, execution verifier/state machine/tests. **Implement:** chain/status/target/input/recipient/log/output checks, confirmations and reorg tracking. **Depends:** 8.6. **Tests:** revert, wrong chain/token/recipient, missing log, reorg. **Verify:** `pnpm test:api -k chain_receipt`. **Accept:** CONFIRMED requires all evidence. **Rollback:** remain SUBMITTED/RECONCILING. **Commit:** `security: verify authoritative transaction receipts`. **Done:** chain fixtures pass.

### 8.8 Reconcile balances and positions
**Purpose:** verify intended economic result. **Problem:** receipt success does not prove received position. **Targets/files:** reconciliation service, wallet/RPC adapters, receipts/tests. **Implement:** before/after block-pinned balances, transfer/output amount, tolerance, RECONCILING/FAILED states. **Depends:** 8.7. **Tests:** delayed indexer, fee token, short output, mismatch. **Verify:** `pnpm test:api -k reconciliation`. **Accept:** position evidence precedes final completion claim. **Rollback:** explicit unresolved status. **Commit:** `feat: reconcile confirmed execution positions`. **Done:** scenarios pass.

### 8.9 Add global and scoped kill switches
**Purpose:** halt financial side effects immediately. **Problem:** only startup Boolean exists. **Targets/files:** config/execution repository/admin API/worker/tests. **Implement:** global, tenant, strategy, wallet switches; workers recheck before sign and broadcast; audited activation. **Depends:** 7.7,8.5. **Tests:** activation at each race point. **Verify:** `pnpm test:api -k kill_switch`. **Accept:** no new signing/submission after halt; reconciliation continues read-only. **Rollback:** only authorized explicit release. **Commit:** `security: enforce layered execution kill switches`. **Done:** race tests pass.

### 8.10 Validate controlled testnet lifecycle
**Purpose:** produce evidence before any mainnet discussion. **Problem:** no safe full-lifecycle proof. **Targets/files:** testnet harness/fixtures, `verify_execution.py`, runbook/report. **Implement:** isolated throwaway assets/keys, crash/retry/reorg/kill tests, independent review checklist; never enable default. **Depends:** 8.1–8.9 and explicit authorization. **Tests:** full testnet suite. **Verify:** `pnpm verify:mainnet-execution` must still refuse absent explicit gated configuration; run documented testnet command separately. **Accept:** all evidence/reviews pass or Gate D remains BLOCKED. **Rollback:** revoke test key and disable deployment. **Commit:** `test: verify controlled execution lifecycle`. **Done:** Gate D status is truthfully recorded.

# Phase 9 — End-to-End Integration and Failure Recovery

**Objective:** prove complete workflows and negative paths across real boundaries. **Problems:** current tests are component-heavy and build/E2E baseline is incomplete. **Prerequisites:** Phases 1–7; Phase 8 tests only if authorized. **Outcome:** deterministic system-level evidence. **Risks:** flaky network/time tests. **Validation:** isolated services, fixed clocks, recorded providers. **Exit gate:** critical success/failure/recovery journeys pass with no fake success.

### 9.1 Build an isolated integration environment
**Purpose:** make E2E hermetic. **Problem:** tests rely on process ports/local state. **Targets/files:** compose test profile, Playwright config, test helpers. **Implement:** ephemeral DB, fixed fixtures/clock, unique ports, cleanup, execution off. **Depends:** 7.5. **Tests:** environment self-test. **Verify:** `docker compose --profile test config && pnpm test:e2e`. **Accept:** parallel/repeated runs do not share state. **Rollback:** existing local runner remains. **Commit:** `test: isolate end-to-end test infrastructure`. **Done:** self-test passes.

### 9.2 Test strategy-to-decision success
**Purpose:** verify primary user journey. **Problem:** no owned strategy E2E. **Targets/files:** API/browser E2E tests and fixtures. **Implement:** authenticate, create/approve revision, analyze recorded ticker, inspect provenance/rules. **Depends:** 9.1,7.10. **Tests:** described journey. **Verify:** `pnpm test:e2e --grep 'strategy decision'`. **Accept:** result references exact owner/revision/snapshot. **Rollback:** test-only. **Commit:** `test: cover authenticated strategy decisions end to end`. **Done:** journey passes.

### 9.3 Test compare, export, replay, and tamper
**Purpose:** prove evidence lifecycle. **Problem:** individual functions exist but secured workflow is unproven. **Targets/files:** browser/API E2E. **Implement:** same-snapshot policy comparison, download, offline replay, single-field tamper. **Depends:** 9.2. **Tests:** workflow. **Verify:** `pnpm test:e2e --grep 'receipt replay'`. **Accept:** comparison preserves snapshot; tamper fails. **Rollback:** test-only. **Commit:** `test: cover receipt comparison and tamper detection`. **Done:** journey passes.

### 9.4 Test unsupported and conflicting intents
**Purpose:** prove fail-closed interpretation. **Problem:** UI/API could accept partial meaning. **Targets/files:** parser/API/browser tests. **Implement:** SELL, multiple tickers, missing amount, contradictory limits, unsupported critical clauses. **Depends:** 2.7,9.1. **Tests:** negative matrix. **Verify:** `pnpm test:api -k intent && pnpm test:e2e --grep 'invalid strategy'`. **Accept:** no decision/job is created for blocked input. **Rollback:** test-only. **Commit:** `test: reject ambiguous and conflicting strategies end to end`. **Done:** negative paths pass.

### 9.5 Test provider degradation and schema drift
**Purpose:** verify honest partial failure. **Problem:** provider resilience isn't system-tested. **Targets/files:** provider stub/E2E tests. **Implement:** timeout, 429, malformed payload, one-source failure, total outage. **Depends:** 3.9,9.1. **Tests:** fault matrix. **Verify:** `pnpm test:api -k provider_failure`. **Accept:** independent evidence survives; unknowns/retries are visible; no fallback relabeling. **Rollback:** test-only. **Commit:** `test: verify provider failure degradation`. **Done:** fault matrix passes.

### 9.6 Test stale, unknown, and insufficient evidence
**Purpose:** validate core financial honesty. **Problem:** missing freshness/depth is a release risk. **Targets/files:** fixtures and E2E tests. **Implement:** stale/future/unknown timestamp, peer-only reference, volume-only/undersized quote. **Depends:** 4.5–4.6,9.1. **Tests:** policy matrix. **Verify:** `pnpm test:api -k 'freshness or liquidity'`. **Accept:** dependent policies reject/UNKNOWN with exact reason. **Rollback:** test-only. **Commit:** `test: enforce unknown market evidence end to end`. **Done:** matrix passes.

### 9.7 Test tenant isolation and abuse controls
**Purpose:** prove forbidden paths. **Problem:** authorization success tests are insufficient. **Targets/files:** security integration tests. **Implement:** cross-tenant ID substitution, list leakage, role escalation, CSRF/CORS/rate abuse. **Depends:** 7.7–7.9,9.1. **Tests:** attacker matrix. **Verify:** `pnpm test:api -k security`. **Accept:** no object existence/data leaks; audit records attempts. **Rollback:** fail closed. **Commit:** `test: verify tenant isolation and API abuse controls`. **Done:** security matrix passes.

### 9.8 Test worker restart and duplicate delivery
**Purpose:** prove durability. **Problem:** lease logic needs process-level evidence. **Targets/files:** worker integration harness/tests. **Implement:** terminate worker mid-stage, duplicate enqueue, lease expiry, retry exhaustion, cancel race. **Depends:** 6.9,9.1. **Tests:** process failure matrix. **Verify:** `pnpm test:api -k job_recovery`. **Accept:** one logical result and coherent event history survive restart. **Rollback:** quarantine jobs. **Commit:** `test: verify job restart and duplicate recovery`. **Done:** recovery matrix passes.

### 9.9 Prove execution remains inaccessible
**Purpose:** guard analysis release. **Problem:** new integration could accidentally expose mutation. **Targets/files:** API/MCP/keeper/browser security tests. **Implement:** bypass UI, forged state/approval, recorded input, agent tool enumeration, config toggles. **Depends:** 7.10; 8 optional. **Tests:** negative execution surface. **Verify:** `pnpm test:api -k execution_boundaries && cd services/keeper/agent && pnpm test`. **Accept:** no sign/broadcast with default config or read-only agents. **Rollback:** disable suspect route. **Commit:** `test: prove analysis cannot trigger execution`. **Done:** boundary suite passes.

### 9.10 Run and publish the functional acceptance suite
**Purpose:** close integrated quality gate. **Problem:** results otherwise remain fragmented. **Targets/files:** CI/local script, `docs/validation.md`. **Implement:** aggregate deterministic backend/web/keeper/contracts/build/E2E/demo results with environment metadata. **Depends:** 9.1–9.9. **Tests:** full suite. **Verify:** `./scripts/ci-local.sh` (or documented contract skip with reason). **Accept:** failures block; unavailable live/testnet checks are explicit. **Rollback:** report only. **Commit:** `test: publish functional acceptance evidence`. **Done:** Phase 9 gate passes.

# Phase 10 — Production Engineering and Release Readiness

**Objective:** make the verified analysis product operable, recoverable, and releasable. **Problems:** limited supply-chain checks, environment separation, observability, backup/restore, release gates. **Prerequisites:** Phase 9. **Outcome:** production runbooks and enforced release evidence. **Risks:** secret leakage, migration failure, silent outages, misleading claims. **Validation:** CI, staging drills, scans, load/recovery tests. **Exit gate / Gate E:** all applicable controls pass; Gate D remains independent.

### 10.1 Harden CI branch gates
**Purpose:** enforce all required checks. **Problem:** CI lacks migration/contract snapshot/recovery gates. **Targets/files:** `.github/workflows/ci.yml`, scripts. **Implement:** least-privilege permissions, concurrency cancellation, pinned actions, matrix stages, required artifacts. **Depends:** 9.10. **Tests:** workflow lint and representative PR run. **Verify:** `python3 scripts/check-doc-links.py && ./scripts/ci-local.sh`. **Accept:** failed required checks block merge without production secrets. **Rollback:** revert workflow while retaining prior CI. **Commit:** `ci: enforce complete functional quality gates`. **Done:** green protected PR evidence exists.

### 10.2 Add supply-chain and secret scanning
**Purpose:** detect dependency/image/credential risk. **Problem:** CI only greps a few key patterns. **Targets/files:** workflows, dependency policies, ignore file/docs. **Implement:** lockfile audit, secret scanner, SBOM, container scan, license policy with triage. **Depends:** 10.1. **Tests:** seeded safe detection fixtures. **Verify:** documented local scanners and CI dry run. **Accept:** high-severity/unreviewed secrets block; exceptions expire. **Rollback:** disable faulty rule, not whole gate. **Commit:** `security: enforce supply-chain and secret scanning`. **Done:** scan artifacts are produced.

### 10.3 Separate runtime environments and secrets
**Purpose:** prevent test/production crossover. **Problem:** one settings class covers all modes. **Targets/files:** `config.py`, deployment manifests, `.env.example`, tests. **Implement:** local/test/staging/prod profiles, secret references, production startup invariants, no secret values in images. **Depends:** 7.5,10.2. **Tests:** configuration matrix. **Verify:** `pnpm test:api -k config && docker compose config`. **Accept:** test cannot target production; unsafe prod fails startup. **Rollback:** execution remains off. **Commit:** `build: enforce environment and secret separation`. **Done:** matrix passes.

### 10.4 Add structured observability
**Purpose:** diagnose end-to-end actions. **Problem:** logs/events lack unified metrics/traces. **Targets/files:** logging/telemetry modules, API/worker/provider instrumentation, dashboards docs. **Implement:** correlation IDs, redacted structured logs, latency/error/queue/provider metrics, trace propagation. **Depends:** 6.7,7.9. **Tests:** correlation/redaction/exporter failure. **Verify:** `pnpm test:api -k observability`. **Accept:** one request traces to job/decision/receipt without secrets. **Rollback:** no-op exporter. **Commit:** `feat: add redacted end-to-end observability`. **Done:** telemetry tests pass.

### 10.5 Define health, readiness, and alerts
**Purpose:** support safe orchestration. **Problem:** `/health` probes dependencies synchronously and conflates states. **Targets/files:** API health routes, deployment probes, alert rules/tests. **Implement:** liveness, readiness, dependency status, queue/stuck/provider/error-budget alerts. **Depends:** 10.4. **Tests:** degraded DB/provider/worker. **Verify:** `pnpm test:api -k health && docker compose config`. **Accept:** liveness stays cheap; readiness blocks unsafe traffic; alerts are actionable. **Rollback:** conservative not-ready. **Commit:** `feat: separate health readiness and operational alerts`. **Done:** probe tests pass.

### 10.6 Prove backup, restore, and retention
**Purpose:** prevent irrecoverable user/evidence loss. **Problem:** no tested production recovery. **Targets/files:** scripts/runbooks, migration/repository tests, deployment config. **Implement:** encrypted backups, retention/deletion, point-in-time assumptions, isolated restore drill and validation. **Depends:** 7.5,10.3. **Tests:** restore owned strategies/jobs/receipts and hashes. **Verify:** documented disposable-environment restore command. **Accept:** restored data preserves integrity/ownership within RPO/RTO. **Rollback:** never test against production target. **Commit:** `docs: prove database backup and restore procedures`. **Done:** drill evidence recorded.

### 10.7 Validate performance and capacity
**Purpose:** establish safe operating bounds. **Problem:** synchronous provider/database paths have no SLOs. **Targets/files:** load tests, performance config/docs. **Implement:** representative read/write/job loads, concurrency, payload limits, provider budgets, DB pool sizing. **Depends:** 10.4–10.5. **Tests:** baseline and overload. **Verify:** documented load command against isolated environment. **Accept:** SLO/capacity and graceful overload are measured, not guessed. **Rollback:** reduce limits/scale. **Commit:** `perf: establish service capacity and overload behavior`. **Done:** report is reproducible.

### 10.8 Exercise deployment and rollback
**Purpose:** make releases reversible. **Problem:** compose/demo exists but production rollout is unproven. **Targets/files:** deployment manifests, migration/release/rollback runbooks, smoke tests. **Implement:** immutable images, provenance, preflight, expand/contract migrations, canary/rollback and compatibility window. **Depends:** 10.1–10.7. **Tests:** staging deploy and rollback drill. **Verify:** documented staging release command plus `scripts/check-host.mjs`. **Accept:** old app/new schema compatibility and rollback are demonstrated. **Rollback:** procedure is itself tested. **Commit:** `build: verify deployment and rollback workflows`. **Done:** staging evidence exists.

### 10.9 Complete security, privacy, and legal review gates
**Purpose:** prevent technical readiness from overstating launch readiness. **Problem:** financial/geographic/data obligations are undecided. **Targets/files:** `SECURITY.md`, threat model, privacy/retention and release checklist. **Implement:** abuse cases, dependency findings, disclosure, data map, jurisdiction/issuer claims, independent review requirements. **Depends:** 10.2,10.6,10.8. **Tests:** tabletop incident and access review. **Verify:** `python3 scripts/check-doc-links.py`. **Accept:** owners/evidence/blockers are named; execution review is separate. **Rollback:** block release. **Commit:** `security: document production review and incident gates`. **Done:** reviews are signed or explicitly blocking.

### 10.10 Execute the production release gate
**Purpose:** decide release from evidence. **Problem:** passing tests alone does not prove operability. **Targets/files:** `docs/validation.md`, deployment/incident/operations docs, release checklist. **Implement:** collect Gates A–E, verify claims/config/secrets/migrations/restore/alerts/rollback, list deferred execution/provider checks. **Depends:** 10.1–10.9. **Tests:** full CI, staging smoke, restore/rollback/tabletop evidence. **Verify:** `./scripts/ci-local.sh && python3 scripts/check-doc-links.py`. **Accept:** Gate E passes only with evidence for every enabled feature; Gate D does not inherit approval. **Rollback:** do not release or roll back per runbook. **Commit:** `docs: record production release readiness evidence`. **Done:** release decision and blockers are auditable.

## Release-gate checklist

- **Gate A — Reliable development baseline:** BLOCKED until Phase 1 proves fresh API tests, production build, keeper, and available contract checks.
- **Gate B — Functional analysis MVP:** BLOCKED until Phases 2–5 prove versioned strategies, normalized evidence, unified decisions, replay, and tamper detection.
- **Gate C — Secure internal beta:** BLOCKED until Phases 6–7 and Phase 9 prove authentication, tenancy, durable jobs, persistence, and failure recovery.
- **Gate D — Controlled execution readiness:** BLOCKED by design until Phase 8, explicit product authorization, controlled testnet evidence, independent security review, and applicable legal review. It never enables mainnet automatically.
- **Gate E — Production release readiness:** BLOCKED until Phase 10 proves CI, environment separation, security scans, observability, backup/restore, performance, deployment/rollback, and operational reviews.

## Dependency graph and completion definition

`Baseline → Strategy/lifecycle → Provider evidence → Decision engine → Receipts → Durable jobs → Auth/persistence → Integrated analysis beta → Production release`. Optional execution branches only after provider, jobs, and auth/persistence, then rejoins integration through its own Gate D evidence. This order contains no dependency on a later phase for an earlier exit gate.

A capability is complete only when its entry point, strict input schema, deterministic business rules, provider/persistence dependencies, typed output/error/timeout behavior, authorization, redacted audit evidence, unit/integration/E2E tests, and recovery behavior all satisfy its step criteria on the current commit. A fixture test proves deterministic compatibility, not live provider availability. A receipt hash proves integrity, not authenticity or execution. No enabled feature may present `SUBMITTED` or `CONFIRMED` without the authoritative evidence defined above.
