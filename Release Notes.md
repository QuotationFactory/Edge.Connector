# QF.Agent.Service release notes

This summarizes commits from 2024-06-02 (inclusive) up to HEAD.

Generated: 2026-05-05

---

## Highlights

- Added a resilience/retry pipeline for retrieving agent settings (fixes endless retry / background service stop). (IoT / Agent client)
- Improvements to command waiter logic (fix blocking and sharing-violation handling). (Agent host)
- Added a simple updater and related packaging changes. (updater)
- New Agent time-registration export feature. (Shared external/time registration)
- Several message contract updates (extended request messages, deprecations). Check "Breaking changes & notes". (Shared external/messages)
- Project / build updates: migrated/updated language level and frameworks (Net9, nullable, treat warnings as errors) and multiple third-party package upgrades.

---

## Breaking changes & important notes

- remove unused ExternalRef from ExportToErpResponse, ExportToErpAssemblyResponse, ExportToErpPartTypeResponse
  - Commit: c0dfc187e395fada5a77dd775e2a9a97e091829b
  - Author: Wibe
  - Date: 2025-03-31
  - Original message: "remove unused ExternalRef from ExportToErpResponse, ExportToErpAssemblyResponse, ExportToErpPartTypeResponse"
  - Impact: Message contracts had fields removed. Consumers that rely on ExternalRef will need to adapt. Marked as potentially breaking — please verify integrations that deserialize these message types.

- Chore Net9.0 / Implement IExternalKey / language-level bumps / Nullable enabled / TreatWarningsAsErrors
  - Commits: 91e684f5..., bcba8692..., f584bb19... (see below for details)
  - Impact: Upgrading target frameworks / enabling nullable / enabling TreatWarningsAsErrors may cause compilation failures for downstream projects or require dependency updates. Test builds and CI pipelines before upgrading production deployments.

- Deprecate WorkingStepKey from RequestManufacturabilityCheckOfPartTypeMessage (deprecated)
  - Commit: ade22183...
  - Impact: Field deprecated — consumers should migrate away; planned removal may be breaking later.


---

## Features

- Implement resilience pipeline with retry strategy for retrieving agent settings
  - Commit: f584bb19f32bec7707958abad557be8cfe28623e
  - Author: Wibe
  - Date: 2026-03-25
  - Original message: "Merged PR 6987: Implement resilience pipeline with retry strategy for retrieving agent settings"
  - Summary: Adds a retry/resilience pipeline around agent settings retrieval to avoid EndlessRetry scenarios and ensure background services stop correctly.
  - Impact: Improves reliability of IoT agent startup and settings retrieval; reduces risk of stuck background tasks.

- AgentTimeRegistrationExport added
  - Commit: 6ef9e78d2be375dba57643f0e9e832c74b83e85e (merged); 851b3f5... (author commit)
  - Author: Albert
  - Date: 2024-11-20 / 2024-07-26
  - Original message: "Merged PR 5469: AgentTimeRegistrationExport added"
  - Summary: Adds export support for agent time registration data.
  - Impact: New export capability — verify export consumers and permissions.

- Implements RequestRepeatOrderMessage and Response
  - Commit: 814f79d..., 5ee599d...
  - Author: Wibe
  - Date: 2025-07-22 / 2025-05-19
  - Summary: Adds repeat-order message type and response.
  - Impact: New message contract for repeat ordering flows.

- Implements AgentAutoInviteImportRequest in AddressBookSyncMessage and RequestAutoInviteMessageResponse
  - Commit: a22ad725..., 6ccd765f...
  - Author: Wibe
  - Date: 2025-07-29 / 2025-07-03
  - Summary: Adds support for agent auto-invite import flows in address-book sync messages.

- Added a simple updater
  - Commit: ba85e8d649ffffca83abf4b6410c6e8187f30080
  - Author: Remco
  - Date: 2024-06-20
  - Summary: Adds a simple updater component (updater/).
  - Impact: Packaging and release process may use updater utilities.

---

## Fixes

- Handle sharing violation of IOException explicitly (command waiters / ping/pingreceived)
  - Commit: 01998817288809c939880866dda2a4b868ce28d6
  - Author: Remco
  - Date: 2024-06-19
  - Original message: "handle sharing violation of IOException explicitly, only log and error after 2 mins."
  - Summary: Avoids spurious hard failures when files are locked briefly; defers error logging until the lock persists.
  - Impact: More robust handling of IOExceptions caused by sharing violations.

- Refactor WaitForCommand → RegisterCommandWaiter and fix blocking issue
  - Commit: 36ff5fad4c7e250c7655e1d209a4e1e03cc7531a
  - Author: Remco
  - Date: 2024-06-19
  - Summary: Refactors command waiter registration to avoid blocking issues (improves responsiveness of command processing).

- Fix: duplicate files when using GetAllFilters
  - Commit: a0a42768a420d84dea0923f3d61352f814520486
  - Author: Wibe
  - Date: 2025-05-19
  - Summary: Fixes duplication bug in filter-based file enumerations.

- Fix: EventLog
  - Commit: 0b37d423f9fc4f965278684b07dd10d65b2c6dd1
  - Author: Wibe
  - Date: 2025-05-20
  - Summary: Fixes EventLog handling; also related commit makes EventLogLevel required.

- Fix buildnumber formatting
  - Commit: c4add45a816dbbcd65cc9cb3187c285615d8068d
  - Author: Wibe
  - Date: 2024-09-10

---

## Refactors & Chores

- Multiple dependency / package updates
  - Various commits in late 2025 and 2026: upgrades to Mediatr 13.1.0, many third-party package updates, and general "chore: update packages" commits.
  - Example commit: 734cc906befc6c567c43d299d4e3183904b309cc — chored mediatr → v13.1.0 (2026-03-20)
  - Impact: Verify CI and run full integration tests after upgrading third-party packages.

- Chore: Net9.0 migration and language-level updates
  - Commits: 91ae4a9a6e2f6e0fa8890ffb17ae359dd4c28747 (merged PR 6251: Update to Net9), 91e684f5dacd..., bcba8692...
  - Summary: Updates the codebase for .NET 9 and related language level changes (global.json). Ensure all build agents and target runtimes are compatible.

- Enable Nullable and TreatWarningsAsErrors
  - Commits in 2026-03: 2479ab03..., b3c20412..., etc.
  - Summary: Nullable reference types enabled and TreatWarningsAsErrors set to true. This can surface new compile-time issues.

- Do not publish and pack
  - Commit: c6eff1f...
  - Summary: Prevents publishing/packing for certain projects as part of packaging changes.

- Make EventLogLevel required
  - Commit: 83f4387f145390b814b7255aa09948dc0846bc21
  - Summary: Configuration change requiring EventLogLevel setting.

- Documentation and small cleanups
  - Commits: 8197a735..., 758db130..., etc.

---

## Full commit list (chronological, newest first)

(Each entry: hash — author — date)

- f584bb19f32bec7707958abad557be8cfe28623e — Wibe — 2026-03-25
  - Merged PR 6987: Implement resilience pipeline with retry strategy for retrieving agent settings

- aead337f7c6b777e8df87c95a2a2563b0ade9d83 — Wibe — 2026-03-24
  - Implement resilience pipeline with retry strategy for retrieving agent settings to fix TryExecuteBackgroundServiceAsync to stop when EndlessRetryRetrieveAgentSettingsAsync

- 734cc906befc6c567c43d299d4e3183904b309cc — Wibe — 2026-03-20
  - Merged PR 6982: chore mediatr to version 13.1.0

- d699c62fb757d171adc3f9bdb3fe3a18b9e3b504 — Wibe — 2026-03-20
  - chore mediatr to version 13.1.0

- 2479ab0384bfe5d6403394a78b08251a4e48fa3b — Wibe — 2026-03-16
  - Merged PR 6972: Implements Nullable enabled and TreatWarningsAsErrors is true

- b3c20412392f8d65ce90e04d2f249f46f5475fec — Wibe — 2026-03-16
  - Implements Nullable enabled and TreatWarningsAsErrors is true

- 379c19a5994e23eada58a92127481dd0f11d275a — Wibe — 2025-12-23
  - Merged PR 6797: Updates 3th party packages

- 6e1f743f19ff41d57cdfe48b4038c9c25711a55b — Wibe — 2025-12-23
  - Merged PR 6796: updates 3th party packages

- d3c23498c1c23b0a127a1c63c8398ce234cc57a0 — Wibe — 2025-12-23
  - updates 3th party packages

- 36db3f28d5cb553f2333817ad35a0326a9da93d1 — Wibe — 2025-12-23
  - Merged PR 6793: Extend RequestManufacturabilityCheckOfPartTypeMessage with necessary props.

- f7748eb622178707df8639a1ec70135561d0b82e — Wibe — 2025-12-22
  - using new feed in nuget.config adds sourcemap for unitsnet package

- 7ac683aa37179c5491f29e97c7064c437310a90e — Wibe — 2025-12-22
  - updates 3th party packages

- 5573d26457bdc0534342aee9075b73db37ea2dc2 — Wibe — 2025-12-20
  - Extend RequestManufacturabilityCheckOfPartTypeMessage with necessary props.

- 5b347b62fb3a940c246dc14906c5e6d75a89b2c6 — Wibe — 2025-10-01
  - Merged PR 6608: Deprecate WorkingStepKey from RequestManufacturabilityCheckOfPartTypeMessage and update packages

- 85c42bdab16a7b50415252a9d8dfd202bf55a36f — Wibe — 2025-10-01
  - chore: update packages

- ade221838b95b4db984f5a24388cd9a3cf7d8683 — Wibe — 2025-10-01
  - Deprecate WorkingStepKey from RequestManufacturabilityCheckOfPartTypeMessage and RequestManufacturabilityCheckOfPartTypeMessageResponse

- 7d4b876a01a014f6132a403426c51ce7ffacde79 — Wibe — 2025-07-31
  - Merged PR 6460: chore: update packages

- abbaf513e4ddb288e32b5c376fb401ca0ec03d6c — Wibe — 2025-07-31
  - chore: update packages

- a22ad7256c09a2ff37a5e67cdcc46e7686181d48 — Wibe — 2025-07-29
  - Merged PR 6432: Implements AgentAutoInviteImportRequest in AddressBookSyncMessage and a new RequestAutoInviteMessageResponse

- 6ccd765f00f8743e284fb754e23ab4e6453f772e — Wibe — 2025-07-03
  - Implements AgentAutoInviteImportRequest in AddressBookSyncMessage and a new RequestAutoInviteMessageResponse

- 814f79ddb80e7b60d0b85b6abb9e5a6a35d71ea4 — Wibe — 2025-07-22
  - Merged PR 6355: Implements RequestRepeatOrderMessage and Response

- f7d0a96c5cb3b028a51ef3c1b619ca76634f3aa3 — Wibe — 2025-05-26
  - Rename: SellingBuyingPartyArticle

- a0a42768a420d84dea0923f3d61352f814520486 — Wibe — 2025-05-19
  - Fix: duplicate files when using GetAllFilters

- 5ee599dac73e327769eed4bf7a12e893617469ae — Wibe — 2025-05-19
  - Implements RequestRepeatOrderMessage and Response

- 440db159c565f42965d8e3c31ebb3e1adb4cd796 — Wibe — 2025-05-20
  - Merged PR 6356: chore: update packages

- 0b37d423f9fc4f965278684b07dd10d65b2c6dd1 — Wibe — 2025-05-20
  - Fix: EventLog

- 758db1303f099acdb0b9ea46771b20043f8a0078 — Wibe — 2025-05-20
  - chore: Code cleanup

- 8a5bb730b63438b0a96f9d8dcf36e53080c84576 — Wibe — 2025-05-19
  - chore: update packages

- 91ae4a9a6e2f6e0fa8890ffb17ae359dd4c28747 — Wibe — 2025-03-31
  - Merged PR 6251: Update to Net9

- 91e684f5dacd86712e8976c2fcdf07a71625a285 — Wibe — 2025-03-31
  - Chore Net9.0

- bcba869242e9a9044f59685ad3050db1f157041d — Wibe — 2025-03-31
  - Implement IExternalKey

- c0dfc187e395fada5a77dd775e2a9a97e091829b — Wibe — 2025-03-31
  - remove unused ExternalRef from ExportToErpResponse, ExportToErpAssemblyResponse, ExportToErpPartTypeResponse

- 6ef9e78d2be375dba57643f0e9e832c74b83e85e — Albert — 2024-11-20
  - Merged PR 5469: AgentTimeRegistrationExport added

- 8197a73516d9a3aa3d4b8c018c88ada29b2324ad — Wibe — 2024-11-20
  - Adds some documentation

- 850e0e7dbb8286c805a7adbb13d2c162b29ba94d — Wibe — 2024-10-01
  - update language level and global.json

- 83f4387f145390b814b7255aa09948dc0846bc21 — Wibe — 2024-10-01
  - Make EventLogLevel required

- c4add45a816dbbcd65cc9cb3187c285615d8068d — Wibe — 2024-09-10
  - Fix buildnumber formatting

- c6eff1fad5757f36c6e791b8d593c118f64a0aff — Wibe — 2024-08-29
  - Do not publish and pack

- 851b3f524ebdccd340f178a70ef221a69d821ff4 — Albert — 2024-07-26
  - AgentTimeRegistrationExport added

- ba85e8d649ffffca83abf4b6410c6e8187f30080 — Remco — 2024-06-20
  - Merged PR 5372: add a simple updater

- e1f0560d259523c6237129246ccb215b89d9d865 — Remco — 2024-06-20
  - Merged PR 5369: Fix command waiters (ping/pingreceived)

- 36ff5fad4c7e250c7655e1d209a4e1e03cc7531a — Remco — 2024-06-19
  - refactor WaitForCommand > RegisterCommandWaiter and fix blocking issue

- 01998817288809c939880866dda2a4b868ce28d6 — Remco — 2024-06-19
  - handle sharing violation of IOException explicitly, only log and error after 2 mins.

---





## Update 20-06-2024
* Add updater (first version)
* Fix possible lock (WaitForCommand > RegisterCommandWaiter and fix blocking issue)


## Update 07-05-2024
* Agent Build version is send to backend to make it visible in the agent settings.


## Update 23-04-2024
* updates versioned datacontract to version 0.3.51
* Updates nuget packages to latest versions


## Update 22-04-2024
* Implements KeepAlive and Waiting for response loop.
    When response not received in max 5 minutes, connector will restart and try to re-connect.
* updates versioned datacontract to version 0.3.45

## Update 05-04-2024
NEEDS update of appsettings.json
Implemented improved logging implementation (Application Insights)

## Update 27-03-2024
Improved re-connection to the cloud infrastructure
Upgrade from .net6 to .net8 incl. dependency packages
