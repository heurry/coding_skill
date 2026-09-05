---
name: execute-readme-task
description: Convert a repository README and the user's request into a requirements matrix, a dependency-aware implementation plan, small verified changes, and an evidence-based completion report. Use when asked to understand a README-driven project, finish a multi-step repository task, implement several related requirements, or continue work whose scope and acceptance criteria are primarily documented in README.md.
---

# Execute README Task

Use a low-freedom workflow so that every conclusion, change, and completion claim is traceable to repository evidence.

## Apply the operating rules

1. Treat the current user request as the goal, the README as requirements, and the repository as implementation truth.
2. Obey higher-priority instructions and every applicable repository instruction file.
3. Read before editing. Search for existing implementations before creating new ones.
4. Make the smallest coherent change that satisfies the current requirement.
5. Preserve unrelated user changes. Never include them in a commit or rewrite them.
6. Separate facts, inferences, and unknowns. Never present an inference as a README requirement.
7. Require executed verification evidence before marking any item complete.
8. Perform one bounded implementation step at a time. Verify it before starting the next step.

## Maintain task state

Use a plan tool when available. Otherwise maintain an internal checklist with exactly one item marked `in_progress`.

Track these states:

- `DISCOVER`: read instructions, README, repository structure, and current state.
- `PLAN`: build the requirements matrix and ordered implementation steps.
- `EXECUTE`: implement one bounded step.
- `VERIFY`: run the narrow check, then the broader checks.
- `DONE`: every required item has evidence.
- `BLOCKED`: required information, authority, credentials, or environment capability is unavailable.

Never move directly from `DISCOVER` to `EXECUTE` for a multi-file or multi-requirement task.

## Phase 1: Discover

Before editing:

1. Read the user request and the complete README.
2. Locate and read applicable instruction files such as `AGENTS.md`.
3. Inspect the repository status without modifying it.
4. Identify manifests, build files, test configuration, entry points, and relevant source files.
5. Search for implementations related to every named feature, command, type, or file.
6. Record pre-existing modified and untracked files so they remain untouched.

If the README contains no actionable goal, report the missing information and stop instead of inventing a project.

## Phase 2: Build a requirements matrix

Create one row per independently verifiable requirement:

| ID | Requirement | Source | Classification | Acceptance check | Status |
|---|---|---|---|---|---|
| R1 | Concrete observable behavior | README section or user request | explicit, inferred, or unknown | Command or inspection | pending |

Apply these rules:

- Quote or closely paraphrase the source; include its section or file location.
- Split compound bullets into separate requirements.
- Express requirements as observable outcomes, not implementation guesses.
- Mark README/user statements as `explicit`.
- Mark conclusions derived from code as `inferred`.
- Mark missing decisions as `unknown`.
- Include non-functional constraints, compatibility requirements, and forbidden changes.

Resolve ambiguity conservatively:

- Ask the user only when a choice changes a public interface, persistent data, security behavior, destructive action, cost, or the main product behavior.
- Otherwise choose the smallest reversible interpretation and record it as an assumption.
- Never silently weaken an explicit acceptance criterion.

## Phase 3: Analyze the gap

For every requirement, determine:

1. What already exists.
2. What is missing or incorrect.
3. Which files and interfaces are involved.
4. Which earlier requirement it depends on.
5. Which existing test or command can validate it.

Do not assume a README command works. Confirm the command and its configuration in the repository when possible.

## Phase 4: Create an atomic plan

Order steps by dependency: shared contracts and infrastructure first, core behavior second, integration third, documentation and broad validation last.

Define every step with this schema:

| Field | Required content |
|---|---|
| Goal | One observable result |
| Requirements | IDs satisfied by the step |
| Files | Expected files to inspect or change |
| Action | One small implementation operation |
| Narrow verification | Fastest relevant check |
| Completion condition | Exact evidence required |

Reject and split a step if it:

- has multiple unrelated goals;
- cannot be verified independently;
- mixes broad refactoring with behavior changes;
- requires editing files that have not been inspected;
- uses vague completion language such as "works correctly".

## Phase 5: Execute one step

For the current step only:

1. Re-read the relevant requirement and source files.
2. Confirm assumptions against code.
3. Edit only the files needed for that step.
4. Add or update a focused test when behavior changes.
5. Inspect the diff for accidental or unrelated changes.
6. Run the narrow verification specified in the plan.
7. Mark the step complete only when the check passes or direct inspection conclusively verifies a non-executable artifact.

After a successful step, update affected requirement rows and select the next dependency-ready step.

## Handle failures deliberately

When verification fails:

1. Preserve the exact command, exit status, and relevant error output.
2. Classify the failure as implementation, test expectation, environment, dependency, permission, or ambiguous requirement.
3. Form one root-cause hypothesis supported by evidence.
4. Make one targeted correction.
5. Re-run the smallest reproducing check.

After two failed corrections for the same symptom, stop patching. Re-read the relevant call path, configuration, and assumptions before attempting another change.

Never hide a failing check, delete a legitimate test, relax assertions without requirement evidence, or label an unrun check as passing.

## Phase 6: Verify progressively

Run checks in this order when available:

1. Focused test for the changed behavior.
2. Tests for the affected module or package.
3. Static analysis, formatting, or type checks.
4. Repository build.
5. Broader regression suite recommended by the project.
6. Final diff and repository-status inspection.

If a broad check is too expensive or unavailable, run the strongest practical subset and state exactly what remains unverified.

## Apply the completion gate

Enter `DONE` only if all conditions hold:

- Every explicit requirement is `pass`, `not applicable` with justification, or `blocked` with evidence.
- Every `pass` row cites an implementation location and an executed verification result.
- No known failure is omitted.
- The final diff contains no unrelated user files or accidental generated artifacts.
- Documentation matches the implemented behavior and commands.

Do not equate code written, a successful build, or one passing test with task completion unless it covers every requirement.

## Report the result

Lead with the outcome. Use this compact format:

### Outcome

State whether the task is complete, partially complete, or blocked.

### Requirement acceptance

| ID | Status | Implementation | Evidence |
|---|---|---|---|
| R1 | pass, fail, or blocked | File/module | Executed command or direct check |

### Verification

List only commands actually executed and their results.

### Assumptions and limits

List only assumptions or unverified items that materially affect confidence or future work.
