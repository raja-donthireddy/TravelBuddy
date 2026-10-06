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
- **Region for India: India South Central** (product owner decision). Before building, confirm that the region is available to our subscription and that every service we use (Azure SQL Database, Key Vault, the compute and push services) is offered there; if one is missing, the fallback is Central India, to be agreed.
- **Region for UK users:** not yet decided. See the open question below.
- Specific Azure services (App Service or Container Apps, Notification Hubs or direct push) are chosen in the Plan for the first backend Story.

## Open question
One deployment or two? A single deployment in India South Central would hold UK users' data in India, which needs a lawful transfer mechanism under UK GDPR. Two regional deployments (UK South and India South Central) keep data in-country but need a way to handle a UK traveller sharing with an Indian contact.
- Retention: scheduled job deletes journey data per ADR-0001.

## Consequences
- One language for backend and web page; separate Flutter and .NET toolchains in one repository.
