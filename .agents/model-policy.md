# Model management and delegation

Policy version: **1.0.0** (2026-10-08).

This policy governs model selection and delegation for Claude Code and Codex.
Keep repository specifications, approval gates, scientific provenance, frozen
artifacts and review restrictions in force. This policy supersedes older model
routing instructions only; it does not grant new product or publication authority.

## Responsibilities and provider routing

| Responsibility | Claude Code | Codex |
|---|---|---|
| Coordination, specification and final decisions | Selected parent session; Fable stays here when selected | Selected parent model and reasoning effort |
| Independent review and difficult judgment | `opus` | Separate agent inheriting the parent; optional `gpt-6-astra` for demanding or costly decisions |
| Implementation and substantial source extraction | `sonnet` | Inherit the parent; `gpt-6.1-sol` is the starting choice when explicit routing is useful |
| Narrow clerical work | Scripts first; `haiku` if a model is needed | Scripts first; optional `gpt-6-luna` if a model is needed |

These are responsibility mappings, not demonstrated capability equivalences.
Model names are availability-dependent examples, not a mandate to replace the
user's selected parent. Claude aliases apply only to Claude invocations.

## Claude Code

- Select `opus`, `sonnet` or `haiku` explicitly for delegated work, including
  nested delegation. Never spawn Fable as a worker or inherit it accidentally.
- Put the intended tier in custom-agent frontmatter and use a per-invocation
  override when the task needs another tier. Scientific design, interpretation,
  gate decisions and unresolved diagnosis require the parent or Opus.
- Keep external referee backends and their existing restrictions intact; a
  Claude wrapper's model is separate from the external model it calls.

## Codex

- Default: omit model and reasoning-effort overrides. Workers inherit both from
  their parent. A separate reviewer may use the same model as the implementer.
- Introduce explicit routing only when representative tasks show a benefit.
  Use Astra for unusually difficult judgment and Luna for bounded repeatable
  work; retain Sol or the selected parent for substantial implementation.
- A strict three-tier alternative is Opus role -> Astra, Sonnet role -> Sol,
  Haiku role -> Luna. It is optional, not the default routing policy.
- Use only models and effort settings exposed by the active runtime. Do not
  send Claude aliases to Codex or assume Claude effort values transfer.
  If a requested model is unavailable or overridden, report the actual choice
  when known and the limitation; never claim a model ran without evidence.

## Delegation and acceptance

- Delegate substantial independent execution when tools and task boundaries
  support it. Keep immediate small edits in the parent when delegation adds
  overhead. This policy explicitly permits subagents for those bounded tasks.
- Give each worker the task, relevant original sources, acceptance criteria,
  permitted files, constraints and expected output. Parallel writers own
  distinct files. Prefer a few coherent work units over tiny agent fan-outs.
- Keep the reviewer separate from the producer. Supply the actual artifact,
  original evidence and acceptance criteria in an independent review context;
  the producer's summary alone is insufficient. Respect any frozen review gate.
- Scripts report deterministic results, exit codes and logs. Models judge
  whether the evidence satisfies the requirements. Clerical workers may gather
  evidence but do not accept scientific claims or waive acceptance criteria.
- Escalate on evidence: isolate the failure, improve the brief or inputs, then
  raise the tier or return the decision to the parent. Respect repository stop
  rules; never weaken a check or blindly repeat attempts to obtain a pass.
- Return concise findings and file references. Checkpoint durable state in the
  repository's established location; avoid copying bulk sources into the parent.

## Versioning and rollback

Keep policy updates in isolated commits. Rollout v1.0.0 uses annotated tags
`agent-policy/pre-v1.0.0` and `agent-policy/v1.0.0` on remote default branches.
Local branches that contain unpublished work use corresponding `local-pre-`
and `local-` tags with the branch name appended. Revert the policy commit with
`git revert <policy-commit>`; preserve subsequent work and publish the revert
through the repository's normal workflow. Never reset a working tree to a tag.
