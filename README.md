# txAdminRecipe

A [txAdmin recipe](https://github.com/tabarra/txAdmin/blob/master/docs/recipe.md) that deploys the Hirogi Games framework as a ready-to-start server: base cfx resources + the full `hg-*` stack + a generated `server.cfg` wired for PostgreSQL.

## What it deploys

| Layer | Resources |
|---|---|
| Base (cfx-server-data) | mapmanager, chat, spawnmanager, sessionmanager, baseevents, hardcap |
| Framework | hg-lib → hgpostgresql → hg-framework-core (start order matters) |
| Systems | hg-inventory, hg-weathersync, hg-doorlock |
| Jobs | hg-recyclejob, hg-scrapyard, hg-weed |

`basic-gamemode` is deliberately not started — `hg-framework-core` spawns players itself. `hg-core` is not deployed (it has no fxmanifest; it's a library, not a runnable resource).

## Prerequisites

1. **This workspace on GitHub** — the recipe downloads the resources from `https://github.com/xxxjahlilallenxxx/hirogi-games-core-fivem` (`master`). Push the workspace there, or edit the second `download_github` task in [recipe.yaml](recipe.yaml) to your repo.
2. **Build output committed** — `hgpostgresql/dist/index.js` must exist in the repo (`bun run build` in `hgpostgresql/`); recipes can't run build tools.
3. **A reachable PostgreSQL server** — local install or Azure Database for PostgreSQL. The resources create their own tables on first start.

## How to use

1. Start txAdmin (`FXServer.exe +set serverProfile default`) and open the setup wizard.
2. Choose **Custom Recipe** and either paste the raw GitHub URL of `recipe.yaml` or paste its contents.
3. Let the deployer run, then in the generated `server.cfg` **edit the `postgres_*` convars** (or use `postgres_connection_string`) before the first start.
4. Start the server. Watch the console for `hgpostgresql` connecting and `hg-framework-core` creating its tables.

## After deploy

- Admin commands work for the txAdmin master account (`{{addPrincipalsMaster}}` + `add_ace group.admin command allow`): `/setjob`, `/addmoney`, `/removemoney`, `/giveitem`, `/adddoor`, `/weather`, `/time`, `/freezetime`, `/freezeweather`.
- Per-resource docs live in [hg-docs](../hg-docs/README.md).
