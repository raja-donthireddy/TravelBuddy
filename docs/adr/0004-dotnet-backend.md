# ADR-0004: .NET backend

- **Status:** Accepted (product owner decision, 2026-10-06)
- **Date:** 2026-10-06

## Decision
1. ASP.NET Core on the current .NET LTS release, in `backend/`, with a solution of API, domain and test projects.
2. Responsibilities: accounts and sign-in (ADR-0005), journey state, contact selection, push delivery (FCM and APNs), and the read-only web link page.
3. The web link page is served by the backend. It shows the Google map and polls the journey state about once a minute, plus on arrival.
4. Domain logic stays free of framework dependencies and is unit-tested; API tests run against a test host.
5. Checks before every PR: `dotnet format --verify-no-changes`, `dotnet build`, `dotnet test`.

## Hosting and data
- Hosted in **Azure**. Database: **Azure SQL Database** (SQL Server engine), accessed with Entity Framework Core.
- TLS only; encryption at rest through Azure SQL transparent data encryption (on by default); secrets in Azure Key Vault, not in source or app settings files; managed identity for database access where possible.
- **Test: UK South** (product owner decision). Single deployment serving all users, UK and India. Before building, confirm that every service we use (Azure SQL Database, Key Vault, the compute and push services) is available in UK South; UK West is the suggested fallback.
- **Production: two deployments.** UK South for the UK and **Central India** for India (the product owner's "India Central"; confirm the exact region name and service availability when planning production).
- Data protection for the test phase is deliberately deferred and tracked in `docs/production-readiness.md`.
- Open for production: how a UK user and an Indian user share a journey across the two deployments (where the journey is stored, how contact lookup works across regions). Design before production, not now.
- Specific Azure services (App Service or Container Apps, Notification Hubs or direct push) are chosen in the Plan for the first backend Story.
