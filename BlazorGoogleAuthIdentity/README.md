# BlazorGoogleAuthIdentity

Blazor Server app with Google OAuth, ASP.NET Core Identity, SQL Server, and OpenTelemetry.

| Setting | Value |
|---------|-------|
| **Interactivity** | Server, per-page |
| **Authentication** | Google OAuth only |
| **Roles** | `Admin`, `User` — seeded at startup |
| **Database** | SQL Server via EF Core |
| **Theme** | CSS custom properties (`--cf-theme-*`) |

## Setup

```bash
# Google OAuth credentials
dotnet user-secrets set "Authentication:Google:ClientId" "<id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<secret>"

# Database
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "<connection-string>"

# Create schema
dotnet ef migrations add InitialCreate
dotnet ef database update
```

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

Set `data-cf-theme="<name>"` on `<body>` or any parent element. Available themes: `blue`, `indigo`, `purple`, `green`, `teal`, `gray`, `yellow`, `code-magic`, `pink`, `red`, `orange`, `cyan`.
