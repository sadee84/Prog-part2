# RaceDay – Part 2: RESTful API

_Author: Sade Mzila · Last updated: 20 October 2026_

## Overview

RaceDay is a web-based event management system for South African road running, walking and cycling events. This Part 2 delivers the ASP.NET Core Web API behind it: it handles registration and login, role-based access, events, categories, enrolments and race results, stored in SQL Server through Entity Framework Core. Every endpoint can be viewed and tested in the browser through Swagger.

---

## 1. Roles

| Role | What they can do |
|------|------------------|
| **Organiser** | Create, edit and delete their own events; add age or distance categories to those events; view everyone enrolled in their events; capture finish times and finishing positions after an event; manage their own profile. |
| **Participant** | Register and log in; browse events and their categories; enrol in an event by choosing a category; view and cancel their own enrolments; view their own race results; manage their own profile. |

Access is enforced in the API using server-side session authentication. No session returns **401 Unauthorized**; the wrong role, or touching another Organiser's event, returns **403 Forbidden**.

**Tech stack:** C# / .NET 8, ASP.NET Core Web API, Entity Framework Core (code-first), SQL Server, hashed passwords, Swagger (Swashbuckle), xUnit, GitHub Actions.

---

## 2. Setup and run

**Prerequisites:** Visual Studio 2022 (or the .NET 8 SDK), SQL Server (LocalDB or SQL Server Express), SSMS and Git.

1. **Clone the repository**

   ```bash
   git clone https://github.com/sadee84/Prog-part2.git
   cd Prog-part2/Part2
   ```

2. **Set the connection string.** Open `RaceDay.Api/appsettings.json` and change `ConnectionStrings:DefaultConnection` so `Server=` matches your SQL Server instance (the name you see in SSMS). The database name must stay `RaceDayDb`.

3. **Create the database** (choose one option):

   - **Option A – automatic (recommended):** nothing to run. When the API starts for the first time it creates `RaceDayDb` and all tables from the EF Core model (code-first).

   - **Option B – SQL script:** open `docs/RaceDay_Part2_Schema.sql` in SSMS and click **Execute** before starting the API. The API then uses the database you created.

4. **Run the API** – in Visual Studio open `RaceDay.sln` and press F5, or from the `Part2` folder:

   ```bash
   dotnet run --project RaceDay.Api
   ```

5. **Open Swagger** at **http://localhost:5080/swagger**. Use `POST /api/auth/register` and `POST /api/auth/login` first; Swagger then reuses your session cookie for the other endpoints. Register one Organiser and one Participant (use a private window for the second) to try both roles.

6. **Run the unit tests** (no SQL Server needed, they use an in-memory database):

   ```bash
   dotnet test
   ```

_Optional:_ set `Registration:OrganiserAccessCode` in `appsettings.json` to require a secret code for Organiser sign-up.

---

## 3. SQL script

The full database script is in [`docs/RaceDay_Part2_Schema.sql`](docs/RaceDay_Part2_Schema.sql). It creates `RaceDayDb` with the `Users`, `Events`, `Categories`, `Enrolments` and `Results` tables, including keys, unique indexes, check constraints and cascade rules that match the EF Core model.

---

## 4. CI/CD

GitHub Actions (`.github/workflows/part2-ci.yml`) restores, builds and runs all unit tests on every push.

![Green GitHub Actions build](docs/ci-green-build.png)

---

## 5. Unit testing

21 xUnit tests cover registration and login, authenticated vs unauthenticated requests, role rejection, event management, enrolments and result capture (success and failure cases).

![Unit tests passing](docs/unit-tests.png)

---

## 6. Video

Unlisted YouTube walkthrough of the running API, code structure, endpoints, authentication and unit tests:

**YouTube link:** _PASTE LINK HERE_

---

## 7. References and AI disclosure

### AI disclosure

Claude (Anthropic, 2026) was used in this project to understand the brief, plan the structure of the solution, generate the first draft of the code and help with debugging. The code was then reviewed and run by Sade Mzila.

### References

Anthropic (2026) *Claude* [Large language model]. Available at: https://claude.ai (Accessed: 20 October 2026).

GitHub (n.d.) *Building and testing .NET*. Available at: https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-net (Accessed: 20 October 2026).

Microsoft (n.d.) *Cascade Delete – EF Core*. Available at: https://learn.microsoft.com/en-us/ef/core/saving/cascade-delete (Accessed: 20 October 2026).

Microsoft (n.d.) *Create web APIs with ASP.NET Core*. Available at: https://learn.microsoft.com/en-us/aspnet/core/web-api/ (Accessed: 20 October 2026).

Microsoft (n.d.) *Filters in ASP.NET Core*. Available at: https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/filters (Accessed: 20 October 2026).

Microsoft (n.d.) *Get started with Swashbuckle and ASP.NET Core*. Available at: https://learn.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle (Accessed: 20 October 2026).

Microsoft (n.d.) *Integration tests in ASP.NET Core*. Available at: https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests (Accessed: 20 October 2026).

Microsoft (n.d.) *Model validation in ASP.NET Core MVC and Razor Pages*. Available at: https://learn.microsoft.com/en-us/aspnet/core/mvc/models/validation (Accessed: 20 October 2026).

Microsoft (n.d.) *PasswordHasher<TUser> Class*. Available at: https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.identity.passwordhasher-1 (Accessed: 20 October 2026).

Microsoft (n.d.) *Relationships – EF Core*. Available at: https://learn.microsoft.com/en-us/ef/core/modeling/relationships (Accessed: 20 October 2026).

Microsoft (n.d.) *Session and state management in ASP.NET Core*. Available at: https://learn.microsoft.com/en-us/aspnet/core/fundamentals/app-state (Accessed: 20 October 2026).

xUnit.net (n.d.) *Getting started with xUnit.net*. Available at: https://xunit.net/docs/getting-started/v2/getting-started (Accessed: 20 October 2026).
