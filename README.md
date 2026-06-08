# teqniqly-blazor-google-auth-identity

`dotnet new` template — Blazor Server with Google OAuth, ASP.NET Core Identity, SQL Server, and OpenTelemetry.

## Install

```bash
dotnet new install ./BlazorGoogleAuthIdentity
```

## Use

```bash
dotnet new teqniqly-blazor-google-auth --name MyApp
```

Optionally override the home page tagline:

```bash
dotnet new teqniqly-blazor-google-auth --name MyApp --AppDescription "Manage your inventory."
```

## What you get

- Blazor Server, per-page interactivity
- Google OAuth only (no local passwords)
- ASP.NET Core Identity with `Admin` and `User` roles seeded at startup
- SQL Server via EF Core
- OpenTelemetry with OTLP exporter (traces, metrics, logs)
- Bootstrap 5 + Bootstrap Icons + theming via CSS custom properties
- Docker Compose with SQL Server

See `README.md` in the generated project for setup instructions.
