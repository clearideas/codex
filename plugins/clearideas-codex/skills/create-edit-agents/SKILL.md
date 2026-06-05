---
name: create-edit-agents
description: Use when the user wants Codex to create, edit, validate, clone, schedule, test, or run Clear Ideas agents through the Clear Ideas MCP tools.
---

# Create And Edit Clear Ideas Agents

Use this skill when the user wants Codex to do agent authoring work for Clear Ideas. Treat Codex as the flexible agent editor and Clear Ideas as the schema, validation, persistence, and runtime authority.

Clear Ideas MCP tools use the word `agent` for authoring and execution. Use `agent` in user-facing text unless the user explicitly uses a different term. The saved agent JSON still has runtime schema fields such as `prompts`, variables, triggers, and schedules.

## Core Principle

Do the drafting and repair work in Codex:

- inspect the current agent JSON
- reason about the target behavior
- draft or edit the agent locally
- iterate against Clear Ideas validation
- save only the finished agent
- optionally run, inspect, and refine

Do not hand-author agent JSON from memory when the MCP schema or a saved agent can be read. Use `get_agent_definition_schema` when step fields, model aliases, skill IDs, schedule shapes, examples, deprecations, or current limits matter.

## Create An Agent

1. If the request needs source connections or governed retrieval, call `list_agent_connections`.
2. For a brand-new agent, call `start_agent_design` to get a strong initial draft and durable design session.
3. If the designer asks clarifying questions, ask the user only for the missing details, then call `continue_agent_design`.
4. When a draft exists, call `validate_agent_definition` with the draft.
5. Fix validation issues locally and repeat `validate_agent_definition` until there are no blocking issues.
6. Call `create_agent` with the final draft.
7. Return the created agent name and URL.

If the user explicitly asks Codex to build from scratch, Codex may draft the agent JSON directly, but still must use `get_agent_definition_schema` when uncertain and `validate_agent_definition` before saving.

## Edit An Existing Agent

1. If the target agent is ambiguous, call `list_agents` and ask the user to choose.
2. Call `get_agent` for the selected agent. Keep its `authoringHash`.
3. Edit the agent JSON locally. Preserve unrelated fields and existing intent unless the user asks for a redesign.
4. Call `validate_agent_definition` with the edited draft.
5. Fix blocking issues and repeat validation until clean.
6. Save with `update_agent`, passing the complete edited `agent` and `expectedHash` from `get_agent`.
7. Call `get_agent` again and summarize what changed.

Use direct `update_agent` patches for obvious metadata-only changes such as name, tags, status, simple schedules, or a single clearly specified field. Use the full draft flow for steps, variables, outputs, triggers, HITL, webhooks, sub-agent invocations, source connections, or dataflow.

## Validate And Repair

Validation is the authority. When `validate_agent_definition` returns blocking issues:

- fix the agent JSON locally
- keep changes scoped to the issue unless a broader redesign is necessary
- validate again
- show the user only the relevant problem if user input is needed

Never save a structural edit with validation disabled unless the user explicitly asks to bypass validation and understands the risk.

## Testing An Agent

When the user wants to test or debug execution:

1. Call `run_agent` with `runNow: true` and the required runtime variables.
2. Call `get_agent_run` and `get_agent_run_events` to inspect status, outputs, and runner events.
3. If the run is waiting for human input, use `control_agent_run` with `action: "respond"` after the user supplies answers or a decision.
4. If the run should be stopped or restarted, use `control_agent_run` with `action: "cancel"` or `action: "run"`.
5. Fold findings back into local agent edits, validate, and save the improved agent.

## Scheduling

For schedules and variable presets:

- inspect the current agent first with `get_agent`
- preserve existing schedules and variable presets unless the user asks to replace them
- use `schedule_agent` for schedule/variable preset updates
- validate the resulting agent shape when schedule variables or presets depend on agent variables

## Guardrails

- Use `prompts` as the canonical top-level step array.
- Use literal `{{variableName}}` references for agent variable and step-output dataflow.
- Preserve `outputContract`, `skillIds`, `autoAddSkills`, trigger mappings, schedules, and source connections unless the requested edit affects them.
- Do not invent custom agent skill IDs. Use `get_agent_definition_schema` for current built-in IDs.
- Mark reusable runtime inputs with `requiresOverride: true`.
- Keep HITL steps top-level.
- Keep webhook, sub-agent, schedule, preset, and variable mapping shapes aligned with `get_agent_definition_schema` and `validate_agent_definition`.
- Do not register or call evidence export as an MCP agent tool; evidence export is UI-only.

## Final Response

When work is complete, include:

- what changed
- whether validation passed
- saved agent URL or run URL when available
- any remaining risk, skipped test, or user input needed
