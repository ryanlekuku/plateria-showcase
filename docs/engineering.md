# Engineering approach

## Reliability and safety

- Validate input at system boundaries with explicit schemas.
- Store monetary values as integer cents.
- Apply tenant constraints in both application logic and the database.
- Fail closed when authentication or service configuration is unavailable.
- Keep production credentials and customer data outside development artifacts.

## Verification

The private implementation uses focused unit tests, React component tests, API tests, migration verification, tenant-isolation checks, builds, linting, and Git diff validation. CI checks are attached to an exact commit rather than inferred from prose or stale branch state.

## AI workflow principles

1. **Deterministic scope:** exact repository, commit, and allowed files.
2. **Structured evidence:** machine-validated findings rather than free-form approval.
3. **Separated roles:** report-only review and bounded remediation.
4. **Mechanical limits:** usage budgets, remediation-round caps, and repeated-finding stall detection.
5. **Human authority:** successful automation produces evidence for review; it does not merge or deploy.

The public companion repository, [AI Workflow Engineering](https://github.com/ryanlekuku/ai-workflow-engineering), demonstrates these ideas with original, dependency-free example code.
