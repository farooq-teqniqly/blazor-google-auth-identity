# BlazorGoogleAuthIdentity

Blazor Server app with Google OAuth, ASP.NET Core Identity, SQL Server, and OpenTelemetry.

| Setting | Value |
|---------|-------|
| **Interactivity** | Server, per-page |
| **Authentication** | Google OAuth only |
| **Roles** | `Admin`, `User` — seeded at startup |
| **Database** | SQL Server via EF Core |
| **Theme** | CSS custom properties (`--app-theme-*`) |

## Setup

```bash
# Google OAuth credentials
dotnet user-secrets set "Authentication:Google:ClientId" "<id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<secret>"

# Database connection string
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "<connection-string>"

# Start SQL Server (required before running migrations)
cp .env.example .env          # add MSSQL_SA_PASSWORD
docker compose up -d

# Create and apply schema
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

## Authentication

Google OAuth only — no local password accounts. `ExternalLoginSignInAsync` uses `bypassTwoFactor: true`; Google is trusted to enforce MFA on the Google account.

New users receive the `User` role automatically on first login. Assign `Admin` via `UserManager.AddToRoleAsync(user, "Admin")`.

For production, supply credentials via environment variables:

```
Authentication__Google__ClientId=<id>
Authentication__Google__ClientSecret=<secret>
ConnectionStrings__DefaultConnection=<connection-string>
```

## Rendering

Pages are static SSR by default. Add `@rendermode InteractiveServer` only to components that need client-side behavior. Never add it to Identity/Account pages or to `<Routes>` in `App.razor`.

## Theming

Set `data-app-theme="<name>"` on `<body>` or any parent element. Available themes: `blue`, `indigo`, `purple`, `pink`, `red`, `orange`, `yellow`, `green`, `teal`, `cyan`, `gray`.
