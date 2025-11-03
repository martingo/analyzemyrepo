# GitHub Copilot Instructions for analyzemyrepo.com

## Project Overview

analyzemyrepo.com is a web application that enables users to analyze any GitHub repository and get comprehensive details about its performance, contributors, community governance, geographic diversity, and organizational diversity. The primary goal is to point out repository improvement opportunities and give contributors advice on how to resolve these problems.

Additionally, the platform helps users discover cool projects through weekly updated ratings and collections.

## Architecture

This project uses the **T3 Stack** (create-t3-app), a modern, type-safe web development stack for Next.js applications.

### Technology Stack

- **Framework**: Next.js 13 (React-based)
- **Language**: TypeScript (strict mode enabled)
- **API Layer**: tRPC for end-to-end type-safe APIs
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: NextAuth.js
- **Search**: Meilisearch for repository search functionality
- **Styling**: Tailwind CSS with DaisyUI and Flowbite components
- **Data Visualization**: Nivo charts
- **Data Orchestration**: Prefect (for backend data pipeline)

### Application Structure

```
src/
├── components/     # Reusable React components
├── pages/          # Next.js pages and API routes
│   ├── analyze/    # Repository analysis pages
│   ├── collections/# Repository collections
│   ├── topics/     # Topic-based browsing
│   └── api/        # API endpoints
├── server/         # Server-side code
│   ├── router/     # tRPC routers (github, postgres, etc.)
│   └── db/         # Database client setup
├── styles/         # Global styles
├── types/          # TypeScript type definitions
└── utils/          # Utility functions

prisma/
└── schema.prisma   # Database schema
```

### Data Flow

1. **Hybrid Data Approach**: The application uses both real-time GitHub API calls and pre-fetched data from PostgreSQL
2. **GitHub API**: Direct integration for real-time repository insights
3. **Database**: Pre-processed data uploaded via Prefect for enhanced analytics
4. **Search**: Meilisearch provides fast repository name autocomplete
5. **tRPC Routers**: Type-safe API endpoints in `src/server/router/`

## Development Process

### Core Principles

1. **Easy to Review**
   - Keep pull requests small and focused on a single concern
   - Write clear, descriptive commit messages
   - Break large features into smaller, reviewable chunks
   - Each PR should be independently testable and deployable

2. **Keep Changes Small**
   - Make minimal, surgical changes to accomplish the goal
   - Avoid refactoring existing working code unless necessary
   - One logical change per commit
   - Prefer iterative improvements over large rewrites

3. **Test-Driven Development (TDD)**
   - Write tests before implementing new features when possible
   - Ensure existing tests pass before making changes
   - Add tests for bug fixes to prevent regressions
   - Follow existing test patterns in the codebase

### Code Quality Standards

- **TypeScript**: Use strict type checking, avoid `any` types
- **Linting**: Run `npm run lint` before committing
  - ESLint configuration in `.eslintrc.json`
  - TypeScript ESLint plugin enabled
  - Next.js core web vitals checks
- **Code Style**: Follow existing patterns in the codebase
- **Comments**: Add comments only when necessary to explain complex logic

### Development Workflow

1. **Setup**:
   ```bash
   npm install
   npm run dev  # Start development server
   ```

2. **Before Committing**:
   - Run linting: `npm run lint`
   - Test your changes locally
   - Review changes: `git diff`

3. **Building**:
   ```bash
   npm run build  # Production build
   npm run postbuild  # Generates sitemap
   ```

### Environment Variables

Required environment variables (see `.env.example` or README):
- `GITHUB_ACCESS_TOKEN_2`: GitHub API key (required)
- `DATABASE_URL`: PostgreSQL connection string (optional for local dev)
- `NEXT_PUBLIC_MEILI_URL`: Meilisearch instance URL (optional)
- `NEXT_PUBLIC_MEILI_SEARCH_KEY`: Meilisearch API key (optional)

### Working with Dependencies

- Use `npm install` to add new packages
- Run `npm run postinstall` (or reinstall) after Prisma schema changes
- Keep dependencies up to date but test thoroughly

### API Development

- tRPC routers are in `src/server/router/`
- Each router handles a specific domain (github, postgres, embeddings, etc.)
- Use Zod for input validation
- Follow existing patterns for error handling and response formatting

### Database Changes

- Modify `prisma/schema.prisma` for schema changes
- Run `npx prisma generate` to update the Prisma client
- Use Prisma migrations for production changes

## Best Practices

- **Performance**: Be mindful of GitHub API rate limits
- **Type Safety**: Leverage TypeScript and tRPC for end-to-end type safety
- **Accessibility**: Maintain accessible UI components
- **Responsive Design**: Ensure mobile-friendly layouts
- **Error Handling**: Provide clear error messages to users
- **Security**: Never commit API keys or sensitive data

## Security & EU CRA Compliance

- **AI prompt hygiene**: Never paste secrets, credentials, or API keys into prompts.
- **Human review**: Treat all AI output as untrusted; require human maintainer review before merge.
- **No AI for crypto/auth**: Do not generate cryptography, authentication, or authorization logic with Copilot unless explicitly approved by a security maintainer.
- **Licensing/provenance**: For long suggestions, verify license and provenance before use.
- **Dependency scanning**: Run vulnerability scanning (e.g., OSV) and fix HIGH/CRITICAL findings before merge.
- **Lockfiles**: Commit lockfiles and keep them up to date.
- **Security-only updates**: When feasible, publish security updates separately from feature changes.
- **CVD contact**: Use GitHub Security Advisories as the single point of contact for coordinated vulnerability disclosure.
- **Security testing/reviews**: Perform regular security tests and security reviews of changes.
- **Vulnerability disclosure**: Note fixed vulnerabilities in release notes when applicable.
- **SBOM**: Generate a CycloneDX SBOM for releases when possible.
- **Binary changes**: Any changes to binary files in PRs must be manually reviewed by maintainers.

## Additional Notes

- The application can work without Meilisearch by entering full repository names
- Not all sections are available when running locally without the PostgreSQL data pipeline
- The project uses Apache License 2.0
