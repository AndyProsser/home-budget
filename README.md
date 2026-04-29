# Home Budget

Browser-based household budgeting and cashflow planning for mixed billing cycles, shared accounts, and debt tracking.

## Community

- Contribution guide: [CONTRIBUTING.md](CONTRIBUTING.md)
- Code of Conduct: [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

## What this project solves

Many households are paid fortnightly while bills are due on monthly, annual, or irregular schedules. This project provides a single planning workflow that normalizes those schedules and helps answer one practical question every pay cycle:

- What must be paid now?
- What should be set aside?
- What transfers should happen between accounts?

## Core capabilities

- Fortnightly pay-cycle planning with a checklist workflow
- Frequency normalization for fortnightly, monthly, annual, and ad-hoc expenses
- Shared account planning (Joint, Bills, Credit Card, Savings)
- Debt tracking and payoff horizon projections
- Lookahead visibility for upcoming heavy or high-risk pay cycles

## Documentation

Detailed documentation is available in the docs folder:

- [Documentation index](docs/README.md)
- [Project overview](docs/overview.md)
- [Domain model](docs/domain-model.md)
- [Calculation engine](docs/calculation-engine.md)
- [Architecture](docs/architecture.md)
- [Workflows](docs/workflows.md)

## Development direction

Planned stack and standards:

- TypeScript across frontend and backend
- React + Vite frontend
- Node.js backend with REST endpoints
- Radix UI primitives with a custom theme layer
- Vitest, ESLint, and Prettier
- Docker-based local and deployment workflow

## Contributing

Contributions are welcome.

- Contribution guide: [CONTRIBUTING.md](CONTRIBUTING.md)
- Code of Conduct: [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)

Suggested process:

1. Create a feature branch from main.
2. Keep changes focused and include clear commit messages.
3. Add or update tests for business logic changes.
4. Run linting, tests, and formatting before opening a pull request.
5. Open a pull request with context on what changed and why.

Contribution guidelines:

- Prioritize domain correctness for calculations and pay-cycle planning.
- Keep domain logic in pure, testable functions.
- Avoid coupling UI concerns directly to business logic.
- Prefer small, reviewable pull requests.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).