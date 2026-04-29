# Contributing to Home Budget

Thank you for contributing to Home Budget.

This project focuses on reliable household cashflow planning and debt tracking. Contributions should prioritize correctness, clarity, and maintainability.

## Ways to contribute

- Report bugs with clear reproduction steps.
- Propose UX and workflow improvements.
- Improve calculation correctness and test coverage.
- Improve documentation and examples.

## Development principles

- Keep business logic deterministic and testable.
- Prefer pure functions for calculations.
- Keep domain logic separate from UI concerns.
- Avoid hard-coded dates and magic numbers.
- Keep pull requests small and focused.

## Project direction

The project intent and architecture are documented in:

- AI-CONTEXT.md
- ARCHITECTURE.md
- docs/

Please align contributions with those documents.

## Getting started

1. Fork and clone the repository.
2. Create a branch from main using a descriptive name.
3. Implement the change with tests where applicable.
4. Run local quality checks.
5. Open a pull request.

Example branch naming:

- feat/pay-cycle-transfer-summary
- fix/monthly-normalization-rounding
- docs/contribution-guidelines

## Pull request checklist

Before opening a PR, verify:

- The change is scoped to one concern.
- Documentation was updated if behavior changed.
- Tests were added or updated for business logic changes.
- Existing tests pass locally.
- Linting and formatting checks pass.

## Commit guidance

Use clear, imperative commit messages.

Examples:

- Add debt projection edge-case tests
- Fix annual-to-fortnightly normalization for ad-hoc estimates
- Document pay-cycle checklist statuses

## Coding conventions

- Use TypeScript types/interfaces for domain models.
- Keep files focused and reasonably small.
- Use clear naming that reflects budgeting domain terms.
- Favor composition over inheritance.

## Testing expectations

Prioritize test coverage for:

- amount normalization
- pay-cycle planning and must-pay selection
- transfer recommendation aggregation
- debt payoff projections

## Reporting bugs

Please include:

- expected behavior
- actual behavior
- steps to reproduce
- sample data (sanitized)
- environment notes (OS, Node version)

## Feature requests

Feature requests are welcome. Include:

- problem statement
- proposed behavior
- alternatives considered
- impact on pay-cycle workflow and calculation engine

## Questions

If anything is unclear, open an issue and reference the relevant docs page so discussion stays focused.

## License

By contributing, you agree that your contributions will be licensed under the MIT License in this repository.
