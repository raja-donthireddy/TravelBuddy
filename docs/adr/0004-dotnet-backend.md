# ADR-0004: .NET backend

- **Status:** Accepted for language and framework; hosting and database to be decided
- **Date:** 2026-10-06

## Decision
1. ASP.NET Core on the current .NET LTS release, in `backend/`, with a solution of API, domain and test projects.
2. Responsibilities: accounts and sign-in (ADR-0005), journey state, contact selection, push delivery (FCM and APNs), and the read-only web link page.
3. The web link page is served by the backend. It shows the Google map and polls the journey state about once a minute, plus on arrival.
4. Domain logic stays free of framework dependencies and is unit-tested; API tests run against a test host.
5. Checks before every PR: `dotnet format --verify-no-changes`, `dotnet build`, `dotnet test`.

## Open
- Hosting and database. Recommendation to evaluate: Azure App Service with Azure Database for PostgreSQL or Azure SQL, with managed encryption at rest and TLS. Decide in a follow-up ADR before the sharing Feature.

## Consequences
- One language for backend and web page; separate Flutter and .NET toolchains in one repository.
