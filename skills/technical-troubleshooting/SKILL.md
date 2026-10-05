---
name: technical-troubleshooting
description: Diagnose and resolve technical failures systematically.
version: 1.0.0
author: ryan berned
license: MIT
platforms:
  - linux
  - macos
  - windows
metadata:
  hermes:
    tags:
      - troubleshooting
      - debugging
      - engineering
      - deployment
    category: software-development
    related_skills:
      - software-development
---

# Technical Troubleshooting Skill

This skill provides a systematic workflow for diagnosing and resolving
technical failures. It focuses on evidence, root-cause analysis, minimal
changes, and verification rather than repeated trial-and-error.

## When to Use

Use this skill when:

- An application fails to start.
- A deployment fails.
- A build fails.
- A service returns unexpected errors.
- Configuration changes produce unexpected behavior.
- Logs contain errors that require diagnosis.
- A previously working system stops working.
- A fix has failed and an alternative approach is required.

Do not use this skill for simple informational questions that do not involve
diagnosis or remediation.

## Prerequisites

Use the available native Hermes tools as appropriate:

- `terminal` for runtime inspection and commands.
- `read_file` for inspecting configuration and source files.
- `search_files` for locating relevant files and symbols.
- `patch` for targeted code or configuration changes.
- `web_extract` for authoritative technical documentation when required.

Never assume a command, file, endpoint, environment variable, or configuration
option exists without evidence.

## How to Run

Start by identifying the observable failure.

Collect the smallest useful amount of evidence:

1. Exact error message.
2. Relevant logs.
3. Current configuration.
4. Recent change that preceded the failure.
5. Runtime or deployment state.

Then classify the failure before modifying anything.

## Quick Reference

| Situation | First action |
|---|---|
| Build failure | Inspect build output and changed files |
| Startup failure | Inspect entrypoint, command, environment, and logs |
| Health-check failure | Verify listening address, port, and health endpoint |
| Network failure | Verify endpoint, DNS, connectivity, and authentication |
| Authentication failure | Verify configuration and credential presence without exposing secrets |
| Permission failure | Inspect service/user/role permissions |
| Application error | Locate failing code path and reproduce if possible |
| Configuration regression | Compare known-good and current configuration |
| Previous fix failed | Stop repeating it and investigate an alternative cause |

## Procedure

### 1. Establish the failure

State exactly what is broken.

Do not begin changing configuration before identifying the observable
failure.

Record:

- What should happen.
- What actually happens.
- Where the failure occurs.
- Whether the failure is reproducible.

### 2. Determine the failure layer

Classify the failure into one of these layers:

1. Source code.
2. Dependency or package.
3. Build.
4. Container or runtime.
5. Process startup.
6. Configuration.
7. Networking.
8. Authentication.
9. Permissions.
10. Health checks.
11. External service.
12. User interaction.

Avoid changing components outside the failing layer until evidence requires it.

### 3. Inspect evidence

Use:

- `search_files` to locate relevant files.
- `read_file` to inspect configuration and implementation.
- `terminal` when runtime evidence is required.

Prefer logs and actual runtime behavior over assumptions.

Separate:

- Confirmed facts.
- Strong hypotheses.
- Unknowns.

### 4. Identify the smallest test

Before applying a broad fix, determine the smallest change or test that can
confirm or reject the leading hypothesis.

A useful diagnostic test should answer one question.

Do not combine unrelated fixes because doing so makes the result ambiguous.

### 5. Apply the minimal fix

When the root cause is sufficiently supported:

- Change only what is necessary.
- Preserve working configuration.
- Avoid unnecessary refactoring.
- Avoid changing multiple unrelated variables.
- Never expose secrets in output.

Use `patch` for targeted modifications when appropriate.

### 6. Verify

After the change, verify the behavior that originally failed.

Verification should preferably occur at the same layer as the failure.

Examples:

- Build failure → successful build.
- Startup failure → process remains running.
- Port failure → service listens on the expected interface and port.
- Health failure → health endpoint returns the expected response.
- API failure → authenticated request succeeds.
- Application failure → original operation succeeds.

Do not claim success based only on a changed configuration file.

### 7. Record the result

When the fix works, summarize:

- Root cause.
- Change made.
- Verification performed.
- Any remaining warnings.
- Any recommended follow-up.

If the fix fails, preserve the evidence and continue with a different
hypothesis.

### 8. Pivot after failure

Never blindly repeat a failed approach.

If a change does not work:

1. Revert or isolate the change when appropriate.
2. Record what the attempt proved.
3. Identify what assumption was wrong.
4. Select a different diagnostic path.
5. Test the new hypothesis.

A failed approach is useful evidence and should narrow the search space.

## Pitfalls

### Repeating failed fixes

Do not repeatedly apply the same change after evidence shows it does not
solve the problem.

### Changing too many things

Do not modify several unrelated settings at once.

### Treating symptoms

Do not repeatedly restart, redeploy, or reset a system without understanding
why it fails.

### Guessing configuration

Do not invent configuration keys, endpoints, ports, commands, or file paths.

### Ignoring working components

Do not modify components already proven to work unless new evidence implicates
them.

### Declaring success too early

A configuration change is not proof that the system works.

Always verify the original failure condition.

### Exposing credentials

Never print API keys, passwords, tokens, cookies, private keys, or other
credentials.

## Verification

A troubleshooting task is complete only when the original failure condition
has been verified as resolved.

The final report should contain:

**ROOT CAUSE**
The confirmed reason for the failure.

**CHANGE**
The minimal change that fixed it.

**VERIFIED**
The exact behavior or test that now succeeds.

**REMAINING**
Any known warnings, limitations, or unrelated issues.

If the issue remains unresolved, clearly state:

**NOT FIXED**

and provide the strongest evidence collected and the next diagnostic hypothesis.
