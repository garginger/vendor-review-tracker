# Vendor Review Tracker

A Microsoft Power Platform solution for tracking third-party vendors, their risk tiers, and due-diligence review dates. It's the kind of vendor management work regulated financial institutions do every day, rebuilt from scratch with fake data as a pro-code portfolio project.

I'm building it over 17 weeks (November 2026 to February 2027) while preparing for **Microsoft exam AB-400: Extending Microsoft Power Platform Solutions with Code and AI** (Power Platform Developer Associate). Each phase adds a layer the exam covers.

## What it does

- Tracks vendors with a risk tier (High, Medium, Low) and calculates each one's next review date: every 12, 24, or 36 months.
- Records reviews and findings, and flags vendors that are overdue.
- Enforces business rules on the server, so the data stays consistent whether it's edited in the app, by a flow, or through the API.

## What's in it (planned)

| Layer | What gets built |
| --- | --- |
| Data | Dataverse tables for vendors, reviews, and findings; security roles; alternate keys; environment variables |
| Server logic | C# plug-ins and a custom API (`plb_GetOverdueVendors`) |
| Azure | Timer, HTTP, and Service Bus-triggered Azure Functions using managed identity |
| User experience | Model-driven app, TypeScript client scripts, a PCF code component, and a Power Apps code app |
| Integration | A custom connector, Dataverse service endpoints, and an alternate-key sync job |
| AI | A Copilot Studio agent that answers from deterministic tools, plus a Microsoft Foundry agent |
| ALM | Power Platform Pipelines from Dev to Test, and GitHub Actions with the Power Platform Build Tools |

## Repo layout

```
docs/        Setup notes and design decisions
src/         Solution source, plug-ins, Functions, and front-end code (added as the build progresses)
```

## Notes

- All data is fake. Nothing here comes from any employer's systems.
- Setup decisions for the development tenant are logged in [docs/tenant-setup.md](docs/tenant-setup.md).
