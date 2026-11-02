# Tenant setup log

Decisions made while setting up the personal Microsoft 365 tenant this project runs in, recorded as they were made. Identifiers such as tenant, environment, and organization IDs are left out on purpose because this repo is public.

## Tenant and domain (Oct 2026)

- **Domain:** `plumblinedev.com`, registered through Cloudflare. Cloudflare also hosts DNS.
- **Tenant:** created by signing up for Microsoft 365 Business Basic, which adds Exchange, SharePoint, Teams, and OneDrive for patterns that depend on them. The custom domain is verified in the Microsoft 365 admin center.
- **Why a separate tenant:** everything here is built outside any employer environment, with fake data, so the work can be shown publicly.

## Identity and admin model (Oct 30, 2026)

Three accounts, each with one job:

| Account | Purpose | License |
| --- | --- | --- |
| Daily user | Building and testing; holds the Power Platform Administrator role | Licensed |
| Admin account | Global administration only, kept separate from daily work | None |
| Break-glass account | Cloud-only emergency access if the admin account is locked out | None (removed) |

- **MFA** is required on every account, with a phishing-resistant passkey on the admin account.
- Naming follows a fixed convention set before the first user was added, so new accounts and roles stay predictable.

## Email authentication (Oct 30, 2026)

- **SPF, DKIM, and DMARC** are published in Cloudflare DNS, and all three pass in Gmail's message headers.
- **DMARC** starts at `p=none` to monitor, with a planned move to `p=reject` on Nov 6, 2026 once reports show nothing legitimate failing.

## Power Platform (Nov 1, 2026)

### Licensing

- **Power Apps Premium is deferred.** The plan originally added one license up front for Dataverse and Managed Environments. In practice the developer environment, its Dataverse database, and the Managed Environments setting were all created without it. I'll buy the license when licensing actually blocks a build, and before creating a sandbox for Test.
- **Price timing:** Power Apps Premium list price rises from $20 to $22 per user per month for purchases and renewals from Jan 1, 2027, so an annual term bought in December holds the lower price.

### Environment

- **`VRT-Dev`:** a **managed developer environment** with Dataverse, in the North America geography.
- **Developer instead of sandbox.** The first plan was a managed sandbox. Developer environments don't count against tenant Dataverse capacity, which matters with little or no licensed capacity, and Microsoft now allows one managed developer environment per user.
- **Not the free Developer Plan.** Microsoft stops creating unmanaged developer environments on Nov 15, 2026 and suspends existing ones on Dec 15, 2026. The environment was created as managed from the start, so it isn't affected.
- **No sample apps or data, and no Dynamics 365 apps**, to keep solution exports clean.
- **Test environment (later):** a managed sandbox, `VRT-Test`, for pipeline deployments in Week 15. Tenant capacity needs checking before creating it.

### Solution and publisher

- **Publisher:** `Plumbline`, with prefix **`plb`**. The prefix goes on every table, column, and custom API (for example `plb_vendor`, `plb_GetOverdueVendors`) and can't be changed later, so it's the brand rather than a project code, which lets future portfolio solutions share it.
- **Solution:** `VendorReviewTracker` (display name Vendor Review Tracker), unmanaged in Dev, version 1.0.0.0, set as the **preferred solution** so new components land in it by default.

## Developer toolchain (Nov 1, 2026)

Working on an Apple Silicon Mac:

| Tool | Version | Notes |
| --- | --- | --- |
| .NET SDK | 10.0.403 | Arm64 installer |
| Power Platform CLI (`pac`) | 2.14.1 | Installed as a .NET global tool |
| Node.js | 22.10.0 | LTS |
| Git | 2.47.0 | |
| VS Code | — | C# Dev Kit and Power Platform Tools extensions |

- **PATH fix:** the .NET installer adds `~/.dotnet/tools` to the PATH in a form zsh doesn't expand, so `pac` wasn't found after install. Fixed by adding `export PATH="$PATH:$HOME/.dotnet/tools"` to `~/.zshrc`.
- **Connection check:** `pac auth create` against VRT-Dev, then `pac org who` returns the environment.
- **Windows-only tools:** Visual Studio Community and the Plug-in Registration Tool will run in a Windows VM, set up before plug-in work starts.
