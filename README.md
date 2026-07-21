# OpenG7 Mini Code Lab

Evaluation, training and specialization laboratory for North Mini Code within the OpenG7 sovereign AI ecosystem.

## Workspace architecture

Target workspace architecture:

- `apps/lab-api`: experiment, dataset, adapter and artifact metadata API.
- `apps/lab-dashboard`: experiment tracking and model comparison interface.
- `packages/lab-domain`: experiments, datasets, trajectories, checkpoints and releases.
- `packages/lab-datasets`: dataset validation, transformation and provenance tooling.
- `packages/lab-training`: supervised fine-tuning and preference-training workflows.
- `packages/lab-rl`: verifiable reward environments and agent-training adapters.
- `packages/lab-evaluation`: integration with `openg7-ai-evals`.
- `packages/lab-publishing`: model card, adapter and artifact publication tooling.
- `packages/lab-sdk`: typed experiment and training client.

## Evidence-first approach

The lab exists to turn verified OpenG7 engineering work into measurable learning.

Preferred training trajectory:

```text
issue or task
→ repository snapshot
→ attempted solution
→ tool and test results
→ review feedback
→ accepted patch
→ verified training example
```

Do **not** train directly on unreviewed repository content and assume that exposure equals skill improvement.

## Dataset guidance

Every training example must include:

- source and license
- repository and base commit
- task statement
- allowed and forbidden operations
- tool trajectory, when available
- verification results
- reviewer status
- personal and secret-data scan
- generated-data and teacher-model lineage
- intended training use

Prefer hundreds of high-quality verified trajectories over large volumes of weak synthetic examples.

## Training guidance

Recommended progression:

1. baseline evaluation without OpenG7 training
2. supervised fine-tuning on verified trajectories
3. preference training on accepted versus rejected solutions
4. reinforcement learning with verifiable code and policy rewards
5. full regression evaluation
6. human review and controlled publication

The base model should remain immutable. Publish adapters and checkpoints with explicit lineage.

## Reward guidance

Use verifiable rewards such as:

- build success
- lint success
- existing test success
- new regression-test success
- behavioral task completion
- patch-scope compliance
- policy compliance
- secret-handling compliance
- recovery after failed commands

Avoid optimizing primarily for a model judge's stylistic preference.

## Reuse in other projects

Shared packages:

- `@openg7/mini-code-lab-domain`
- `@openg7/mini-code-lab-datasets`
- `@openg7/mini-code-lab-evaluation`
- `@openg7/mini-code-lab-sdk`

Training workflows may run on separate GPU infrastructure while experiment metadata remains portable.

## OpenG7 example experiment

Initial experiment profile:

- Base model: North Mini Code 1.0-compatible checkpoint
- Experiment: `funding-repository-agent-sft-v1`
- Domain: TypeScript, Node.js, Angular, PostgreSQL, Stripe and Docker
- Training method: adapter-based supervised fine-tuning
- Dataset: verified OpenG7 Funding trajectories
- Baseline suite: `code.patch-and-test`
- Production permissions: none
- Publication: adapter only after evaluation and review

## Commands

The initial workspace is expected to expose the following commands:

```bash
corepack enable
yarn install
yarn lint
yarn format
yarn format:check
yarn test
yarn build
yarn docs
```

Commands may evolve with the implementation, but CI should preserve equivalent lint, test, build, and documentation gates.


## Production launch

Use `docs/experiment-release-checklist.md` before publishing an adapter or merged checkpoint.

The first public release should be an adapter accompanied by a model card, dataset statement, evaluation report and clear upstream attribution. It should not be described as superior without reproducible evidence.

## Mini Code Lab module (V1)

### Environment variables

- `MINI_CODE_LAB_ENV` — `development`, `test`, or `production`.
- `MINI_CODE_LAB_DATABASE_URL` — private experiment metadata database.
- `MINI_CODE_LAB_ARTIFACT_STORAGE_DRIVER` — local or approved private object storage.
- `MINI_CODE_LAB_BASE_MODEL_PATH` — local mounted base model path or approved registry reference.
- `MINI_CODE_LAB_MODEL_GATEWAY_URL` — optional inference gateway for baselines and teachers.
- `MINI_CODE_LAB_EVALS_URL` — OpenG7 AI Evals endpoint.
- `MINI_CODE_LAB_AGENT_RUNTIME_URL` — optional trajectory-generation runtime.
- `MINI_CODE_LAB_IDENTITY_ISSUER` — trusted OpenG7 Identity issuer.
- `MINI_CODE_LAB_AUDIT_ENDPOINT` — experiment and release audit sink.
- `MINI_CODE_LAB_MAX_GPU_HOURS` — experiment budget ceiling.
- `MINI_CODE_LAB_DEFAULT_SEED` — reproducibility seed.
- `MINI_CODE_LAB_DATASET_ENCRYPTION_KEY` — protects controlled datasets.
- `MINI_CODE_LAB_ALLOW_EXTERNAL_TEACHERS` — must default to `false`.
- `MINI_CODE_LAB_PUBLICATION_ENABLED` — must default to `false`.

Example values belong in `.env.example`. Model, provider and repository credentials must use mounted secrets or a secret manager.

### Local metadata launch

```bash
docker compose --profile database up -d postgres
corepack yarn dev
```

Validate a dataset:

```bash
corepack yarn dataset:validate --dataset datasets/sft/funding-v1
```

Run the baseline evaluation:

```bash
corepack yarn evals:baseline --route north-mini-code/default
```

### Experiment directory

Recommended structure:

```text
datasets/
├── sft/
├── preferences/
└── rl-tasks/

evaluations/
├── funding/
├── identity/
├── infrastructure/
└── cross-repository/

environments/
├── docker-sandbox/
└── rewards/

training/
├── sft/
├── preference/
└── rlvr/

adapters/
reports/
schemas/
scripts/
```

Large model artifacts and controlled datasets should not be committed directly to Git.

### Experiment API

```text
POST /api/lab/experiments
GET  /api/lab/experiments
GET  /api/lab/experiments/:experimentId
POST /api/lab/experiments/:experimentId/start
POST /api/lab/experiments/:experimentId/cancel
GET  /api/lab/experiments/:experimentId/runs
GET  /api/lab/experiments/:experimentId/artifacts
```

### Dataset API

```text
POST /api/lab/datasets
GET  /api/lab/datasets
GET  /api/lab/datasets/:datasetId
POST /api/lab/datasets/:datasetId/validate
POST /api/lab/datasets/:datasetId/freeze
GET  /api/lab/datasets/:datasetId/statement
```

A frozen dataset version must be immutable and checksum-addressed.

### Dataset statement

Each dataset release should document:

- purpose
- source repositories and commits
- collection period
- licenses
- consent and privacy handling
- automated and human review
- teacher models used
- known biases and exclusions
- contamination risks
- approved uses
- prohibited or unsupported uses

### Training run contract

A training run should capture:

- base model digest
- tokenizer and chat template
- code revision
- dataset version
- hyperparameters
- seed
- hardware topology
- framework and dependency versions
- checkpoints
- resource usage
- failure and resumption events

### Adapter strategy

Prefer adapter-based development initially:

```text
North Mini Code base
+ OpenG7 general adapter
+ optional domain adapter
```

Potential domain adapters:

- `openg7-funding`
- `openg7-identity`
- `openg7-infrastructure`
- `openg7-documentation`

Validate adapter composition rather than assuming multiple adapters combine safely.

### Teacher-model guidance

A stronger model may generate candidate trajectories only when:

- policy permits the source data to be shared
- provider terms are documented
- generated output is reviewed
- deterministic verification passes
- teacher identity and version are recorded
- resulting data is clearly marked synthetic

Teacher output must not become accepted training data automatically.

### Evaluation gates

Before release, compare the candidate with the unchanged baseline on:

- repository task completion
- regression rate
- tool use
- policy compliance
- secret handling
- French and English instructions
- latency and resource usage
- failure recovery

A candidate must not be promoted if it improves average score while introducing critical safety regressions.

### Release artifacts

A release package should contain:

- adapter or checkpoint
- configuration
- tokenizer or chat-template changes
- model card
- upstream attribution
- license files and notices
- dataset statement
- evaluation report
- checksums
- known limitations
- recommended runtime settings

### Upstream and naming

OpenG7 may maintain an independent derived model or adapter while clearly identifying the upstream base.

Do not imply official endorsement by the upstream creator. Preserve required copyright, license and notice files, and obtain legal review before redistributing merged weights or applying a new license to OpenG7 modifications.

### Publication API

```text
GET  /api/admin/lab/releases
POST /api/admin/lab/releases
POST /api/admin/lab/releases/:releaseId/validate
POST /api/admin/lab/releases/:releaseId/approve
POST /api/admin/lab/releases/:releaseId/publish
POST /api/admin/lab/releases/:releaseId/withdraw
```

Publication requires human approval and passing evaluation gates.

## Security principles

- immutable upstream model reference
- controlled dataset access
- no secrets or personal data in training examples
- explicit external-teacher policy
- isolated training and evaluation environments
- reproducible runs
- resource budgets
- signed and checksummed releases
- human approval before publication
- transparent limitations and attribution

## Integration with OpenG7

- `openg7-ai-evals` — baseline, regression and release evaluation.
- `openg7-agent-runtime` — trajectory generation in isolated repositories.
- `openg7-model-gateway` — stable model routes and teacher access.
- `openg7-knowledge-core` — approved repository and architecture context.
- `openg7-ai-policy-engine` — dataset, provider and publication authorization.

## Initial roadmap

### V1

- dataset schemas and validation
- experiment tracking
- baseline evaluation
- adapter SFT workflow
- model cards and dataset statements
- controlled artifact publication

### V2

- preference datasets
- verifiable reward environments
- automated trajectory review pipeline
- domain adapter comparison
- GPU job orchestration

### V3

- RL with verifiable rewards
- federated training experiments
- privacy-preserving data collaboration
- independently reviewed OpenG7 Mini Code release

## License and governance

The lab code should remain open. Models, adapters and datasets may have different licenses based on upstream obligations and source data. Every published artifact must include a clear license, provenance statement and third-party notices.
