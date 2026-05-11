# Code Review Standards

Last Updated: May 12, 2026

## 1. Objectives

### 1.1 Quality Assurance
Ensure code is correct, maintainable, and follows Lutervyn's coding standards.

### 1.2 Knowledge Sharing
Spread knowledge of the codebase across the team and provide learning opportunities for both authors and reviewers.

### 1.3 Consistency
Maintain a consistent style and architecture across all projects.

## 2. The Review Process

### 2.1 Initiating a Review
- Submit a Pull Request (PR) with a clear title and description.
- Link the PR to the relevant issue or ticket.
- Ensure all CI checks (linting, tests) pass before requesting a review.
- Tag at least one primary reviewer.

### 2.2 Reviewer Responsibilities
- Provide constructive and actionable feedback.
- Aim for a 24-hour turnaround time for initial reviews.
- Focus on logic, architecture, and security first; style second (let linters handle style where possible).
- Use "Nit" for minor suggestions that aren't blockers.

### 2.3 Author Responsibilities
- Respond to all comments (even if just to acknowledge).
- Explain the rationale for specific implementation choices if questioned.
- Avoid becoming defensive; remember that reviews are about the code, not the person.

## 3. Approval Criteria

- At least one "Approve" vote from a qualified reviewer (two for critical systems).
- All "Blocker" comments addressed or resolved.
- All automated tests passing.
- No security vulnerabilities identified.

## 4. Conflict Resolution

- If an author and reviewer cannot agree on a change:
  - Bring in a third engineer for a fresh perspective.
  - Escalate to the Engineering Lead for a final decision.
  - Document the final decision in the PR comments.

## 5. Review Guidelines

### 5.1 What to look for
- See the [Code Review Checklist](./Code%20Review%20Checklist.md) for detailed technical points.

### 5.2 Tone and Communication
- Use "we" instead of "you" (e.g., "Could we use a more descriptive name here?").
- Ask questions rather than making demands (e.g., "What was the reasoning for choosing this library?").
- Praise good work; don't just point out flaws.

---

**Effective Date**: May 12, 2026
**Next Review**: May 12, 2027
