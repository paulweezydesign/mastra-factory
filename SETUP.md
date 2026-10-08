# Local Factory setup

This repository installs the **Factory application**. It is not automatically the repository that coding agents should modify. Select that separate codebase during onboarding.

## Selected configuration

- Official scaffolder: `create-factory@0.2.6` (not `create-mastra`).
- Local Factory server; Mastra platform authentication, PostgreSQL storage, and sandboxes.
- Platform organization: Paul's Organization.
- Platform region: US; Neon database region: AWS US West (Oregon).
- Platform resources may incur charges. Local setup does not authorize a subscription purchase, hosted application deployment, or agent execution.
- Node must satisfy `package.json`'s `engines.node`; setup was checked with Node 24.21.0 and npm 11.19.0.

## Verified setup status

- Official Factory application installed at this repository's root; original Git metadata preserved.
- Dependencies installed; root `npm run check` passed.
- Platform CLI login verified. The official installer reported successful creation of project `mastra-factory` (slug `mastra-factory-ac44`), its API key, production environment, and Neon PostgreSQL database in the selected organization and region.
- Installer credentials are stored only in the ignored `.env`; they have not been inspected or printed by the setup agent.
- User confirmed the encryption key was configured privately; the key has not been read or exposed by the setup agent.
- Local server started successfully at **http://localhost:4111**. Its sign-in page returned HTTP 200 and rendered the Mastra Platform sign-in button; the listener was verified on `127.0.0.1:4111` only.
- Startup completed storage initialization with no missing-encryption warning. Authenticated application/database operations remain untested until browser sign-in.
- The development server is running in the **Factory official installer** terminal canvas. Leave that terminal running while using the app; Ctrl+C stops it. It will not automatically restart after a reboot.
- **Pending:** sign in to the local UI, select a target repository, authorize only necessary integration permissions, configure a model provider, and verify Auto-start runs and Auto-approve plans remain off before enabling intake.
- No hosted application deployment, issue intake, automated approvals, or agent runs were started.

## Windows installer compatibility

On this machine, `npm create factory@0.2.6` failed during its nested dependency install with npm 11's `EALLOWSCRIPTS`: the npm launcher passed an `allow-scripts` option that is not valid for project-scoped installs. Running the same dependency command directly succeeded. The recovery runs the already-downloaded, unmodified `create-factory@0.2.6` executable with Node, outside the npm launcher; it does not globally approve dependency scripts or change npm's script policy.

The installer requires a new directory. Scaffold outside the repository root, then move the completed application into the existing checkout without replacing its `.git` metadata. A failed provisioning run may leave credentials and platform resources behind: inspect its reported status before retrying to avoid duplicate resources. Never inspect secret files as part of troubleshooting.

## Credentials and encryption

The official installer writes platform credentials to `.env`. Never commit that file, print it in agent conversations, or copy credentials into this document. The repository ignores `.env` and its local variants, while retaining the public `.env.example` and `.env.schema` templates.

Before connecting a model provider:

1. Open `.env` yourself in a trusted local editor. Preserve all installer-generated settings.
2. Check whether `FACTORY_CREDENTIAL_ENCRYPTION_KEY` already has a value. **Do not replace an existing key.**
3. If missing, generate a Base64-encoded 32-byte key in a **private terminal**, not an agent-visible terminal:

   ```sh
   node -p "require('node:crypto').randomBytes(32).toString('base64')"
   ```

4. Save that value as `FACTORY_CREDENTIAL_ENCRYPTION_KEY` in `.env`. Do not paste the value into chat.
5. Keep a protected backup with the database's recovery information. Losing or replacing the key can make saved credentials unreadable.
6. Restart the server after configuration. Verify there is no missing-encryption warning before saving any provider or integration credentials.

See the official [credential encryption reference](https://factory.mastra.ai/reference/environment-variables#stored-credential-encryption).

## Run and validate

From this repository's root:

```sh
npm ci
npm run check
npm run dev
```

On an already-installed checkout, skip `npm ci`. Use the actual URL printed by the server; the documented default is `http://localhost:4111`. Do not reuse an unrelated service already occupying that port. This checkout defaults `server.host` to `127.0.0.1` so the local application is not bound to all network interfaces; the standard `MASTRA_HOST` environment variable can override it only when deliberately needed. Keep the development server local; do not expose it through a public tunnel.

`npm run check` checks TypeScript. A successful check does not verify platform provisioning, authentication, database connectivity, or onboarding. Verify startup and the Factory page separately. Stop a terminal-owned server with Ctrl+C; no operating-system auto-start service is installed.

The bundled Docker database scripts are optional alternatives, not needed for the selected platform-backed database. `npm run deploy` is intentionally outside this local setup's scope.

## Human onboarding

1. Sign in to the local Factory UI. CLI login and browser login are separate steps.
2. Choose the repository agents should modify; do not assume this installation repository is the target.
3. Review and explicitly authorize any requested GitHub App permissions. Connect only the intended codebase.
4. Configure encryption first, then connect a model provider directly in the UI. Review provider costs and permissions.
5. Leave **Auto-start runs** and **Auto-approve plans** off; verify both settings before enabling any issue intake.
6. Enable issue intake only when explicitly requested. Do not create test issues, start investigations, approve plans, or launch agents as an installation check.

No target codebase or model provider has been selected by this setup.

## References

- [Factory setup guide](https://factory.mastra.ai/)
- [Factory overview](https://mastra.ai/factory)
- [Official installer reference](https://factory.mastra.ai/reference/create-factory)
- [Environment variables](https://factory.mastra.ai/reference/environment-variables)
