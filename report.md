# prospect-sequence — delivery report (Frantic bounty #56)

**Package:** `epistemedeus/prospect-sequence@sha-9575d8e502fb`
**Public adoption page:** https://runx.ai/x/epistemedeus/prospect-sequence
**Source / provenance:** https://github.com/epistemedeus/prospect-sequence
**PR against runxhq/runx:** https://github.com/runxhq/runx/pull/140

## What it is and why someone would use it

- **What it does.** `prospect-sequence` turns an account (`prospect{company,contact}`) + an `icp` + a `source_allowlist` into a *sourced* outreach plan: it reads only the allowlisted public sources through a **governed HTTP front** (every request and redirect hop re-checked against the allowlist and an **SSRF guard** that refuses loopback / private / link-local / ULA / CGNAT / cloud-metadata like `169.254.169.254`), synthesizes an **angle that cites every source it read**, drafts a **3-touch sequence**, and emits a **gated `send_proposal`** (`decision: proposed`, `requires_approval: true`, `performed_by: send-as`). The judgment is the research and the angle — it never sends and **refuses to fabricate** an account fact it did not read. A real operator would use it because the output is defensible line-by-line: every claim is bound to a `source_url` and a `content_digest`, and the actual send stays a gated downstream Effect.

## Runnable proof (a new user can reproduce all of this with no private context)

- **runx CLI.** `runx-cli 0.6.13` was used for publish, install, dogfood, and verify (meets the ≥0.6.13 floor).
- **Publish.** Published to the hosted registry via URL-as-publish (`POST https://api.runx.ai/v1/index`) under owner **epistemedeus**, name **prospect-sequence**, version **sha-9575d8e502fb**. `runx registry read epistemedeus/prospect-sequence@sha-9575d8e502fb --json` resolves the metadata + digests (`package_digest sha256:be3615…`, `profile_digest sha256:ce5332…`).
- **PR.** `runxhq/runx` PR [#140](https://github.com/runxhq/runx/pull/140) contains `skills/prospect-sequence/{X.yaml, SKILL.md, run.mjs, fixtures/}`; `x_yaml` and `skill_md` are raw-fetchable from the PR head commit and byte-match the published package.
- **Install.** `runx add epistemedeus/prospect-sequence@sha-9575d8e502fb`.
- **Local harness (before publish).** `runx harness ./skills/prospect-sequence` → **2/2 sealed, 0 assertion errors**: `prospect-with-public-source-yields-sourced-sequence` (sealed) and `off-allowlist-private-target-is-refused` (refused). Hosted listing declares both cases at `/v1/skills/epistemedeus/prospect-sequence/harness`.
- **Post-publish dogfood.** `runx skill epistemedeus/prospect-sequence@sha-9575d8e502fb --registry https://api.runx.ai …` resolved the **published** package from the **official source** (`registry_source: "remote https://api.runx.ai"`, `trust_state: trusted`) and sealed receipt **`runx:receipt:sha256:8f572925…`** — the registry run, not the harness fixture seal (see `evidence.json.dogfood.registry_provenance`). The run read `https://example.com/`, produced a sourced angle citing it, a 3-touch sequence, and a gated `send_proposal`.
- **Verify.** `runx verify --receipt receipt.json --json` → **`valid: true`** (digest valid, signature valid); full verdict in [`verification.json`](./verification.json).

## Artifacts (all durably hosted in this repo, commit-pinned)

- [`evidence.json`](./evidence.json) — 12 observations + the `dogfood` block (package, input, command, receipt_ref, verify_verdict, harness_cases, registry_provenance).
- [`verification.json`](./verification.json) — the `runx verify` verdict.
- [`receipt.json`](./receipt.json) — the sealed post-publish dogfood receipt.
- [`SKILL.md`](./SKILL.md), [`X.yaml`](./X.yaml), [`run.mjs`](./run.mjs), [`fixtures/`](./fixtures) — the package.

No tokens or secrets appear in any artifact. The receipt is signed with a dedicated Ed25519 key (`kid: epistemedeus-prospect-seq`); the public verification key is recorded so anyone can independently `runx verify`.
