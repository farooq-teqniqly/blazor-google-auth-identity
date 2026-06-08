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
- SQL Server via EF Core (connection string via user secrets)
- OpenTelemetry with OTLP exporter
- Bootstrap 5 + Bootstrap Icons + theming via CSS custom properties
- Docker Compose with SQL Server

## After instantiation

```bash
cd MyApp

dotnet user-secrets set "Authentication:Google:ClientId" "<id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<secret>"
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "<connection-string>"

# Start SQL Server (required before running migrations)
cp .env.example .env          # add MSSQL_SA_PASSWORD
docker compose up -d

dotnet ef migrations add InitialCreate
dotnet ef database update

dotnet run
```

## OpenTelemetry

Traces, metrics, and logs are exported via OTLP. The endpoint defaults to `http://localhost:4317`. Override in `appsettings.json`:

```json
{
  "OpenTelemetry": {
    "Endpoint": "http://localhost:4317"
  }
}
```

Or via environment variable:

```
OpenTelemetry__Endpoint=http://my-collector:4317
```

Instrumentation included out of the box: ASP.NET Core, HTTP client, EF Core (traces only).

The service name reported to the collector defaults to the project name. To change it, update `ServiceName` in `Extensions/TelemetryExtensions.cs`.
