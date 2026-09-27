---
name: cloudflare-release-workflow
description: Prepare, deploy, and verify scoped Cloudflare Workers or Pages releases. Use when a user asks to deploy, host, publish, or set up a Cloudflare project, or a project rule authorizes deployment after validation.
---

# Cloudflare release workflow

Use this skill for Cloudflare Workers or Pages releases. Follow repository release instructions and current Wrangler or Cloudflare guidance.

## Before release

- Confirm the target repository, Worker or Pages project, environment, branch/commit, working-tree changes, and scope.
- Read deployment instructions and release records. Identify the last known-good production version and a practical rollback path.
- Build and run checks required for the change. A local build does not prove deployment.
- Do not apply migrations, seed data, or modify formal records for a frontend-only or routing-only release. Verify target and impact before any required migration.
- Cloudflare deployment and Git commit/push are separate actions. Do not push without explicit authorization or a clear standing instruction.

## Authorization preflight

Before deploying, check Wrangler identity with the project's supported command, such as wrangler whoami, and compare it with the configured target account.

- Distinguish missing login, wrong account, insufficient permission, and network/tool failure. Never switch accounts or credentials to work around a mismatch.
- If login is missing, guide the user through the official Cloudflare OAuth flow in a visible browser or terminal. Give the next action, wait for interactive login, then check identity again. Do not claim success based only on an exit code or config file.
- Never ask for or print passwords, verification codes, API tokens, cookies, or keyring contents. Do not create plaintext credential files or run short-lived authorization URLs silently in the background.
- Reuse valid local authorization; do not log out or reauthorize just to demonstrate the flow.
- Wrangler CLI authorization, Cloudflare website login, Agent/MCP authorization, and an application's D1 binding are separate. Verify only the layer required by the task.
- For D1 projects, inspect relevant bindings and use read-only checks when needed. Login or a D1 listing does not prove the deployed app is bound correctly. Do not create, alter, migrate, seed, or delete D1 resources unless explicitly requested and the target and impact are verified.

## Deploy

- Deploy only when requested or explicitly authorized by an applicable project rule after scoped validation.
- Use the project's established command and correct account/project. Do not change credentials to bypass a mismatch.
- On failure, inspect durable provider state, version IDs, and asset hashes before retrying. Avoid repeating an operation after a timeout until its outcome is known.
- If provider/UI confirmation is required, prepare the release and stop at that confirmation.

## Verify and record

- Confirm provider success and that the active production version changed to the expected version.
- Run scoped read-only checks against the live route and assets. Verify hashes/build IDs where supported; do not claim a match when evidence is missing.
- Record deployed and rollback versions, validation evidence, incomplete checks, and Git synchronization state.
- Separate local checks from production checks; keep unresolved authenticated or real-device acceptance pending.
- Before any Git commit or push, present the change summary, target repository, and branch, then ask the user to confirm.
