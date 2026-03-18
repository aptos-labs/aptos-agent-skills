# Marketplace Indexing Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or
> superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Get aptos-agent-skills indexed on Tier 1 AI skills marketplaces by making SKILL.md metadata spec-compliant,
enriching package/marketplace manifests, updating documentation, and adding CI validation.

**Architecture:** Pure metadata and config changes -- no application code. Each task modifies frontmatter, JSON
manifests, markdown docs, or YAML workflows. The CI validation workflow uses `yq` to parse YAML frontmatter.

**Tech Stack:** YAML frontmatter (Agent Skills spec), JSON (package.json, marketplace.json), GitHub Actions, `yq`

**Spec:** `docs/superpowers/specs/2026-03-17-marketplace-indexing-design.md`

---

## File Map

| File                                                  | Action | Task | Responsibility                             |
| ----------------------------------------------------- | ------ | ---- | ------------------------------------------ |
| `skills/move/write-contracts/SKILL.md`                | Modify | 1    | Add license, author, version to frontmatter |
| `skills/move/generate-tests/SKILL.md`                 | Modify | 1    | Add license, author, version to frontmatter |
| `skills/move/security-audit/SKILL.md`                 | Modify | 1    | Add license, author, version to frontmatter |
| `skills/move/deploy-contracts/SKILL.md`               | Modify | 1    | Add license, author, version to frontmatter |
| `skills/move/search-aptos-examples/SKILL.md`          | Modify | 1    | Add license, author, version to frontmatter |
| `skills/move/analyze-gas-optimization/SKILL.md`       | Modify | 1    | Add license, author, version to frontmatter |
| `skills/move/modernize-move/SKILL.md`                 | Modify | 1    | Add license, author, version + fix allowed-tools |
| `skills/project/create-aptos-project/SKILL.md`        | Modify | 1    | Add license, author, version to frontmatter |
| `skills/sdk/typescript/use-ts-sdk/SKILL.md`           | Modify | 1    | Add license, author, version to frontmatter |
| `skills/sdk/typescript/ts-sdk-client/SKILL.md`        | Modify | 1    | Add license, author, version to frontmatter |
| `skills/sdk/typescript/ts-sdk-account/SKILL.md`       | Modify | 1    | Add license, author, version to frontmatter |
| `skills/sdk/typescript/ts-sdk-address/SKILL.md`       | Modify | 1    | Add license, author, version to frontmatter |
| `skills/sdk/typescript/ts-sdk-transactions/SKILL.md`  | Modify | 1    | Add license, author, version to frontmatter |
| `skills/sdk/typescript/ts-sdk-view-and-query/SKILL.md`| Modify | 1    | Add license, author, version to frontmatter |
| `skills/sdk/typescript/ts-sdk-types/SKILL.md`         | Modify | 1    | Add license, author, version to frontmatter |
| `skills/sdk/typescript/ts-sdk-wallet-adapter/SKILL.md`| Modify | 1    | Add license, author, version to frontmatter |
| `community-skills/smoothsend-gasless/SKILL.md`        | Modify | 2    | Add version to metadata only               |
| `package.json`                                        | Modify | 3    | Expand keywords array                      |
| `.claude-plugin/marketplace.json`                     | Modify | 3    | Add category and tags to plugin entries     |
| `README.md`                                           | Modify | 4    | Add marketplace badges and links           |
| `INSTALL.md`                                          | Modify | 4    | Fix skill name, expand table, add marketplace section |
| `.github/workflows/validate-skills.yml`               | Create | 5    | CI workflow for SKILL.md validation        |
| `.gitignore`                                          | Modify | 6    | Add .local/ entry                          |
| `.local/marketplace-submissions/claude-plugins-official.md` | Create | 6 | Local-only submission draft            |
| `.local/marketplace-submissions/clawhub-publish.sh`         | Create | 6 | Local-only ClawHub publish script      |
| `.local/marketplace-submissions/verification-checklist.md`  | Create | 6 | Local-only verification checklist      |

---

### Task 1: Update official SKILL.md frontmatter (16 files)

Add `license: MIT` after description, and `author: aptos-labs` + `version: "1.0"` to the metadata block of all 16
official skills. Also fix `modernize-move` `allowed-tools` from YAML list to space-delimited string.

**Files:**

- Modify: `skills/move/write-contracts/SKILL.md` (frontmatter only)
- Modify: `skills/move/generate-tests/SKILL.md` (frontmatter only)
- Modify: `skills/move/security-audit/SKILL.md` (frontmatter only)
- Modify: `skills/move/deploy-contracts/SKILL.md` (frontmatter only)
- Modify: `skills/move/search-aptos-examples/SKILL.md` (frontmatter only)
- Modify: `skills/move/analyze-gas-optimization/SKILL.md` (frontmatter only)
- Modify: `skills/move/modernize-move/SKILL.md` (frontmatter + allowed-tools fix)
- Modify: `skills/project/create-aptos-project/SKILL.md` (frontmatter only)
- Modify: `skills/sdk/typescript/use-ts-sdk/SKILL.md` (frontmatter only)
- Modify: `skills/sdk/typescript/ts-sdk-client/SKILL.md` (frontmatter only)
- Modify: `skills/sdk/typescript/ts-sdk-account/SKILL.md` (frontmatter only)
- Modify: `skills/sdk/typescript/ts-sdk-address/SKILL.md` (frontmatter only)
- Modify: `skills/sdk/typescript/ts-sdk-transactions/SKILL.md` (frontmatter only)
- Modify: `skills/sdk/typescript/ts-sdk-view-and-query/SKILL.md` (frontmatter only)
- Modify: `skills/sdk/typescript/ts-sdk-types/SKILL.md` (frontmatter only)
- Modify: `skills/sdk/typescript/ts-sdk-wallet-adapter/SKILL.md` (frontmatter only)

- [ ] **Step 1: Update all 7 Move skill SKILL.md frontmatters**

For each of the 7 Move skills, insert `license: MIT` after the `description` field and add `author: aptos-labs` and
`version: "1.0"` to the existing `metadata` block.

Example transformation for `skills/move/deploy-contracts/SKILL.md`:

```yaml
# BEFORE:
---
name: deploy-contracts
description:
  "Safely deploys Move contracts..."
metadata:
  category: move
  tags: ["deployment", "devnet", "testnet", "mainnet", "publishing"]
  priority: high
---

# AFTER:
---
name: deploy-contracts
description:
  "Safely deploys Move contracts..."
license: MIT
metadata:
  author: aptos-labs
  version: "1.0"
  category: move
  tags: ["deployment", "devnet", "testnet", "mainnet", "publishing"]
  priority: high
---
```

Apply this same pattern to all 7 Move skills:

1. `skills/move/write-contracts/SKILL.md`
2. `skills/move/generate-tests/SKILL.md`
3. `skills/move/security-audit/SKILL.md`
4. `skills/move/deploy-contracts/SKILL.md`
5. `skills/move/search-aptos-examples/SKILL.md`
6. `skills/move/analyze-gas-optimization/SKILL.md`
7. `skills/move/modernize-move/SKILL.md`

**Special case -- `modernize-move`:** In addition to the above, also fix `allowed-tools` from a YAML list to a
space-delimited string:

```yaml
# BEFORE:
allowed-tools:
  - Read
  - Glob
  - Grep
  - Write
  - Edit
  - Bash
  - AskUserQuestion

# AFTER:
allowed-tools: Read Glob Grep Write Edit Bash AskUserQuestion
```

- [ ] **Step 2: Update the project skill SKILL.md frontmatter**

Apply the same `license: MIT` + `metadata.author`/`metadata.version` pattern to:

1. `skills/project/create-aptos-project/SKILL.md`

Note: `create-aptos-project` has an `allowed-tools` field that is already formatted as a list. Check its format -- if
it is a YAML list, convert it to a space-delimited string like the modernize-move fix.

- [ ] **Step 3: Update all 8 TypeScript SDK skill SKILL.md frontmatters**

Apply the same pattern to all 8 SDK skills:

1. `skills/sdk/typescript/use-ts-sdk/SKILL.md`
2. `skills/sdk/typescript/ts-sdk-client/SKILL.md`
3. `skills/sdk/typescript/ts-sdk-account/SKILL.md`
4. `skills/sdk/typescript/ts-sdk-address/SKILL.md`
5. `skills/sdk/typescript/ts-sdk-transactions/SKILL.md`
6. `skills/sdk/typescript/ts-sdk-view-and-query/SKILL.md`
7. `skills/sdk/typescript/ts-sdk-types/SKILL.md`
8. `skills/sdk/typescript/ts-sdk-wallet-adapter/SKILL.md`

- [ ] **Step 4: Verify all 16 official skills have correct frontmatter**

Run this command to verify every official SKILL.md now has `license: MIT`, `author: aptos-labs`, and `version: "1.0"`:

```bash
for f in $(find skills -name "SKILL.md"); do
  echo "=== $f ==="
  head -20 "$f"
  echo
done
```

Expected: All 16 files show `license: MIT` after description and `author: aptos-labs` + `version: "1.0"` in metadata.

Also verify modernize-move has `allowed-tools: Read Glob Grep Write Edit Bash AskUserQuestion` (single string, not
list).

- [ ] **Step 5: Run formatter and commit**

```bash
bun run format
git add skills/
git commit -m "feat: add license, author, version metadata to all official skills

Standardize SKILL.md frontmatter across all 16 official skills for
Agent Skills spec compliance. Fix modernize-move allowed-tools from
YAML list to spec-compliant space-delimited string."
```

---

### Task 2: Update community skill metadata

Add `version: "1.0"` to the community skill's metadata block. Do NOT add `license` (author's responsibility).

**Files:**

- Modify: `community-skills/smoothsend-gasless/SKILL.md` (frontmatter only)

- [ ] **Step 1: Add version to smoothsend-gasless metadata**

```yaml
# BEFORE:
metadata:
  author: ivedmohan
  category: sdk
  tags: ["typescript", "sdk", "wallet", "gasless", "sponsored", "smoothsend", "transactionSubmitter"]

# AFTER:
metadata:
  author: ivedmohan
  version: "1.0"
  category: sdk
  tags: ["typescript", "sdk", "wallet", "gasless", "sponsored", "smoothsend", "transactionSubmitter"]
```

- [ ] **Step 2: Verify and commit**

```bash
head -15 community-skills/smoothsend-gasless/SKILL.md
bun run format
git add community-skills/smoothsend-gasless/SKILL.md
git commit -m "feat: add version metadata to community skill"
```

---

### Task 3: Enhance package.json and marketplace.json

Expand keywords in package.json and add category/tags to marketplace.json plugin entries.

**Files:**

- Modify: `package.json:5-14` (keywords array)
- Modify: `.claude-plugin/marketplace.json` (add category/tags to plugin entries)

- [ ] **Step 1: Expand package.json keywords**

Replace the `keywords` array in `package.json` with:

```json
"keywords": [
  "aptos",
  "move",
  "smart-contracts",
  "agent-skills",
  "ai",
  "claude",
  "cursor",
  "codex",
  "copilot",
  "web3",
  "blockchain",
  "nft",
  "defi",
  "typescript-sdk",
  "wallet-adapter",
  "move-v2",
  "digital-assets",
  "fungible-assets"
]
```

- [ ] **Step 2: Add category and tags to marketplace.json plugin entries**

In `.claude-plugin/marketplace.json`, add `"category"` and `"tags"` fields to each plugin entry object (after
`"strict": false`).

For the `"aptos-agent-skills"` plugin:

```json
"category": "blockchain",
"tags": ["aptos", "move", "smart-contracts", "web3", "defi", "nft", "typescript-sdk"],
```

For the `"community-skills"` plugin:

```json
"category": "blockchain",
"tags": ["aptos", "community", "gasless"],
```

- [ ] **Step 3: Verify JSON is valid and commit**

```bash
node -e "JSON.parse(require('fs').readFileSync('package.json','utf8')); console.log('package.json: valid')"
node -e "JSON.parse(require('fs').readFileSync('.claude-plugin/marketplace.json','utf8')); console.log('marketplace.json: valid')"
bun run format
git add package.json .claude-plugin/marketplace.json
git commit -m "feat: enhance keywords and marketplace metadata for indexing

Expand package.json keywords for broader search coverage. Add
category and tags to marketplace.json plugin entries for marketplace
crawlers."
```

---

### Task 4: Update README and INSTALL documentation

Add marketplace badges and links to README. Fix INSTALL.md skill table and add marketplace browse section.

**Files:**

- Modify: `README.md:3-6` (badges section)
- Modify: `README.md:113-119` (Learn More table)
- Modify: `INSTALL.md:53-65` (Available Skills table)
- Modify: `INSTALL.md` (add marketplace browse section at end)

- [ ] **Step 1: Add marketplace badges to README.md**

After the existing badges on lines 3-6 of README.md, add:

```markdown
[![SkillsMP](https://img.shields.io/badge/SkillsMP-Browse-purple.svg)](https://skillsmp.com/search?q=aptos)
[![Skills.sh](https://img.shields.io/badge/Skills.sh-Install-blue.svg)](https://skills.sh/search?q=aptos)
```

- [ ] **Step 2: Add marketplace links to README Learn More table**

In the "Learn More" table in README.md (around line 113-119), add two new rows before the closing row:

```markdown
| [SkillsMP](https://skillsmp.com/search?q=aptos)   | Browse Aptos skills on SkillsMP  |
| [Skills.sh](https://skills.sh/search?q=aptos)     | Browse and install via Skills.sh |
```

- [ ] **Step 3: Fix skill name typo in INSTALL.md**

In `INSTALL.md` line 64, change `use-typescript-sdk` to `use-ts-sdk`.

- [ ] **Step 4: Expand INSTALL.md Available Skills table**

Replace the current 8-row Available Skills table in INSTALL.md (lines 53-65) with the full 17-skill table. Use the
README.md skill tables as the source of truth for names and descriptions:

```markdown
## Available Skills

### Move Smart Contracts

| Skill                      | Description                                                      |
| -------------------------- | ---------------------------------------------------------------- |
| `write-contracts`          | Generate secure Aptos Move V2 smart contracts                    |
| `generate-tests`           | Generate Move unit tests targeting 100% code coverage            |
| `security-audit`           | Security audit for Move smart contracts before deployment        |
| `deploy-contracts`         | Deploy Move contracts to devnet, testnet, or mainnet             |
| `search-aptos-examples`    | Search aptos-core for reference implementations and patterns     |
| `analyze-gas-optimization` | Analyze and reduce gas costs in Move smart contracts             |
| `modernize-move`           | Migrate Move V1 resource accounts to V2 object model             |

### TypeScript SDK

| Skill                      | Description                                                                        |
| -------------------------- | ---------------------------------------------------------------------------------- |
| `use-ts-sdk`               | Aptos TypeScript SDK orchestrator for frontend integration                         |
| `ts-sdk-client`            | Set up Aptos client with AptosConfig and network selection                         |
| `ts-sdk-account`           | Create Aptos accounts and signers from private keys or derivation paths            |
| `ts-sdk-address`           | Parse and derive Aptos account addresses (AIP-40)                                  |
| `ts-sdk-transactions`      | Build, sign, and submit Aptos transactions including sponsored and multi-agent     |
| `ts-sdk-view-and-query`    | Call Move view functions and query on-chain Aptos data                              |
| `ts-sdk-types`             | Map Move types to TypeScript (u64, u128, address, vector)                          |
| `ts-sdk-wallet-adapter`    | Integrate Aptos wallets into React apps with useWallet                             |

### Project Setup

| Skill                      | Description                                                 |
| -------------------------- | ----------------------------------------------------------- |
| `create-aptos-project`     | Scaffold new Aptos dApp projects with create-aptos-dapp     |

### Community

| Skill                      | Description                                                 |
| -------------------------- | ----------------------------------------------------------- |
| `smoothsend-gasless`       | Sponsor gas fees for Aptos transactions with SmoothSend     |
```

- [ ] **Step 5: Add marketplace browse section to INSTALL.md**

Add at the end of INSTALL.md, before or after the "Manual Installation" section:

```markdown
## Browse on Marketplaces

- **SkillsMP:** [skillsmp.com/search?q=aptos](https://skillsmp.com/search?q=aptos)
- **Skills.sh:** [skills.sh/search?q=aptos](https://skills.sh/search?q=aptos)
- **SkillHub:** [skillhub.club/search?q=aptos](https://www.skillhub.club/search?q=aptos)
- **ClawHub:** [clawhub.ai/search?q=aptos](https://clawhub.ai/search?q=aptos)
```

- [ ] **Step 6: Format and commit**

```bash
bun run format
git add README.md INSTALL.md
git commit -m "docs: add marketplace badges, links, and expand INSTALL skill table

Add SkillsMP and Skills.sh badges to README. Add marketplace links to
Learn More table. Fix use-typescript-sdk typo in INSTALL.md. Expand
Available Skills table from 8 to 17 skills. Add Browse on Marketplaces
section."
```

---

### Task 5: Create CI validation workflow

Create a GitHub Action that validates SKILL.md frontmatter on every PR touching skills.

**Files:**

- Create: `.github/workflows/validate-skills.yml`

- [ ] **Step 1: Create the validation workflow**

Create `.github/workflows/validate-skills.yml`:

```yaml
name: Validate SKILL.md files

on:
  pull_request:
    paths:
      - "skills/**"
      - "community-skills/**"

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install yq
        run: |
          sudo wget -qO /usr/local/bin/yq https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64
          sudo chmod +x /usr/local/bin/yq

      - name: Validate SKILL.md frontmatter
        run: |
          errors=0

          for skill_file in $(find skills community-skills -name "SKILL.md" 2>/dev/null); do
            dir_name=$(basename "$(dirname "$skill_file")")
            echo "Validating: $skill_file"

            # Extract YAML frontmatter (between --- delimiters)
            frontmatter=$(sed -n '/^---$/,/^---$/p' "$skill_file" | sed '1d;$d')

            if [ -z "$frontmatter" ]; then
              echo "  ERROR: No YAML frontmatter found"
              errors=$((errors + 1))
              continue
            fi

            # Check required field: name
            name=$(echo "$frontmatter" | yq '.name // ""')
            if [ -z "$name" ] || [ "$name" = "null" ]; then
              echo "  ERROR: Missing required field 'name'"
              errors=$((errors + 1))
            elif [ ${#name} -gt 64 ]; then
              echo "  ERROR: name exceeds 64 characters: '$name'"
              errors=$((errors + 1))
            elif ! echo "$name" | grep -qE '^[a-z0-9]([a-z0-9-]*[a-z0-9])?$'; then
              echo "  ERROR: name must be lowercase alphanumeric + hyphens, no leading/trailing hyphens: '$name'"
              errors=$((errors + 1))
            elif echo "$name" | grep -q -- '--'; then
              echo "  ERROR: name must not contain consecutive hyphens: '$name'"
              errors=$((errors + 1))
            fi

            # Check name matches directory
            if [ "$name" != "$dir_name" ]; then
              echo "  ERROR: name '$name' does not match directory '$dir_name'"
              errors=$((errors + 1))
            fi

            # Check required field: description
            desc=$(echo "$frontmatter" | yq '.description // ""')
            if [ -z "$desc" ] || [ "$desc" = "null" ]; then
              echo "  ERROR: Missing required field 'description'"
              errors=$((errors + 1))
            elif [ ${#desc} -gt 1024 ]; then
              echo "  ERROR: description exceeds 1024 characters"
              errors=$((errors + 1))
            fi

            # Check allowed-tools is a string if present
            tools_type=$(echo "$frontmatter" | yq 'type_of(.["allowed-tools"] // null)')
            if [ "$tools_type" = "!!seq" ]; then
              echo "  ERROR: allowed-tools must be a space-delimited string, not a list"
              errors=$((errors + 1))
            fi

            echo "  OK"
          done

          if [ $errors -gt 0 ]; then
            echo ""
            echo "FAILED: $errors validation error(s) found"
            exit 1
          fi

          echo ""
          echo "All SKILL.md files passed validation"
```

- [ ] **Step 2: Test the validation locally**

Run the validation logic against the current skills to verify it works:

```bash
for skill_file in $(find skills community-skills -name "SKILL.md" 2>/dev/null); do
  dir_name=$(basename "$(dirname "$skill_file")")
  frontmatter=$(sed -n '/^---$/,/^---$/p' "$skill_file" | sed '1d;$d')
  name=$(echo "$frontmatter" | yq '.name // ""' 2>/dev/null || echo "yq-not-installed")
  echo "$skill_file -> name='$name' dir='$dir_name' match=$([ "$name" = "$dir_name" ] && echo YES || echo NO)"
done
```

Expected: All 17 skills show `match=YES`. If `yq` is not installed locally, install it first:
`brew install yq` (macOS) or skip this step (it will be tested in CI).

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/validate-skills.yml
git commit -m "ci: add SKILL.md frontmatter validation workflow

Validates name, description, name-directory match, and allowed-tools
format on every PR touching skills/ or community-skills/. Uses yq for
reliable YAML parsing."
```

---

### Task 6: Create local marketplace submission docs

Add `.local/` to `.gitignore` and create local-only submission documentation. These files are NOT committed.

**Files:**

- Modify: `.gitignore` (add `.local/` entry)
- Create: `.local/marketplace-submissions/claude-plugins-official.md` (local only)
- Create: `.local/marketplace-submissions/clawhub-publish.sh` (local only)
- Create: `.local/marketplace-submissions/verification-checklist.md` (local only)

- [ ] **Step 1: Add .local/ to .gitignore**

Add this line to the end of `.gitignore`:

```
# Local-only files (not committed)
.local/
```

- [ ] **Step 2: Commit .gitignore change**

```bash
git add .gitignore
git commit -m "chore: add .local/ to gitignore for local-only files"
```

- [ ] **Step 3: Create Claude Plugins Official submission draft**

Create `.local/marketplace-submissions/claude-plugins-official.md`:

```markdown
# Claude Plugins Official Submission

## Primary: In-App Submission

Submit via Anthropic's in-app forms:
- Claude.ai: https://claude.ai/settings/plugins/submit
- Console: https://platform.claude.com/plugins/submit

## Submission Content

**Plugin Name:** aptos-agent-skills
**Author:** Aptos Labs (aptos-labs)
**Repository:** https://github.com/aptos-labs/aptos-agent-skills
**License:** MIT
**Category:** Blockchain / Web3

**Description:**
AI skills for building fullstack Aptos dApps -- from Move smart contract development to wallet
connection and frontend integration. 17 specialized skills covering the complete dApp lifecycle.

**Skills (17 total):**

Move Smart Contracts (7):
- write-contracts: Generate secure Aptos Move V2 smart contracts
- generate-tests: Generate Move unit tests targeting 100% code coverage
- security-audit: Security audit for Move smart contracts before deployment
- deploy-contracts: Deploy Move contracts to devnet, testnet, or mainnet
- search-aptos-examples: Search aptos-core for reference implementations
- analyze-gas-optimization: Analyze and reduce gas costs in Move contracts
- modernize-move: Migrate Move V1 resource accounts to V2 object model

TypeScript SDK (8):
- use-ts-sdk: TypeScript SDK orchestrator for frontend integration
- ts-sdk-client: Aptos client setup with AptosConfig
- ts-sdk-account: Account/signer creation
- ts-sdk-address: Address parsing and derivation (AIP-40)
- ts-sdk-transactions: Build, sign, submit transactions
- ts-sdk-view-and-query: View functions and on-chain queries
- ts-sdk-types: Move-to-TypeScript type mapping
- ts-sdk-wallet-adapter: React wallet integration with useWallet

Project Setup (1):
- create-aptos-project: Scaffold new Aptos dApp projects

Community (1):
- smoothsend-gasless: Gasless transaction sponsorship (third-party)

**Quality Evidence:**
- CI: Prettier formatting checks on all PRs
- CI: SKILL.md frontmatter validation on all PRs
- Agent Skills spec compliant (agentskills.io)
- 9 pattern reference documents
- MIT licensed
- Active maintenance by Aptos Labs

## Secondary: Direct PR

If in-app submission is unavailable, submit a PR to:
https://github.com/anthropics/claude-plugins-official
```

- [ ] **Step 4: Create ClawHub publish script**

Create `.local/marketplace-submissions/clawhub-publish.sh`:

```bash
#!/usr/bin/env bash
# Publish aptos-agent-skills to ClawHub
# Prerequisites: npm i -g clawhub (from official openclaw/clawhub repo only)
#                clawhub auth login

set -euo pipefail

echo "Publishing aptos-agent-skills to ClawHub..."
clawhub publish .
echo "Done. Verify at: https://clawhub.ai/search?q=aptos"
```

- [ ] **Step 5: Create verification checklist**

Create `.local/marketplace-submissions/verification-checklist.md`:

```markdown
# Marketplace Verification Checklist

Run these checks after the PR is merged to main.

## Auto-Indexed Marketplaces

- [ ] **SkillsMP**: Search "aptos" at https://skillsmp.com -- skills should appear
  - If not found: submit manually at skillsmp.com (may need 2+ GitHub stars)
  - Check again in 24-48 hours (crawling schedule varies)

- [ ] **Skills.sh**: Run `npx skills search aptos` -- should list aptos-agent-skills
  - Also check: https://skills.sh (search for "aptos")
  - Skills appear after first install via `npx skills add aptos-labs/aptos-agent-skills`

- [ ] **SkillHub**: Search "aptos" at https://www.skillhub.club -- skills should appear
  - Check again in 24-48 hours if not found immediately

## Manual Submissions

- [ ] **Claude Plugins Official**: Submit via in-app form (see claude-plugins-official.md)
  - Expected timeline: review may take days/weeks

- [ ] **ClawHub**: Publish via CLI
  ```bash
  npm i -g clawhub  # install from official openclaw/clawhub repo only
  clawhub auth login
  clawhub publish .
  ```
  - Verify: search "aptos" at https://clawhub.ai

## Verification: Claude Code Plugin

- [ ] Run in Claude Code:
  ```
  /plugin marketplace add aptos-labs/aptos-agent-skills
  ```
  - Verify all 17 skills load correctly
  - Test a skill activation (e.g., say "write a contract")

## Verification: npx skills

- [ ] Full install test:
  ```bash
  npx skills add aptos-labs/aptos-agent-skills
  ls ~/.claude/skills/  # or ~/.cursor/skills/
  ```
  - Verify skills are copied correctly

## Notes

- Auto-indexed marketplaces may take 24-48 hours to update after merge
- GitHub stars affect SkillsMP ranking -- encourage stars on the repo
- Skills.sh ranking is based on install count -- encourage installs
```

- [ ] **Step 6: Verify local files are gitignored**

```bash
git status
```

Expected: The `.local/` directory and its contents should NOT appear as untracked files.
