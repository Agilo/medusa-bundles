# medusa-bundles

## Pre-requisites

- docker
- node 20
- Medusa CLI tool: `npm i @medusajs/medusa-cli -g`
- yarn (v3)
- yalc: `npm i yalc -g`

## Getting started

### Initial setup

1. Copy `.env.example` to `.env` and edit if needed
2. `docker compose up`
3. Open a new terminal tab
4. Install dependencies in all packages: `yarn install && yarn run setup`
5. Run the migrations: `cd dev/medusa && medusa migrations run && cd ../..`
6. Seed the database: `cd dev/medusa && yarn run seed:medusa-plugin-bundles && cd ../..`
7. `yarn run start`
8. Medusa Admin is now available at http://localhost:7001 and Medusa Storefront at http://localhost:8000

Default credentials for Medusa Admin are:

```
admin@medusa-test.com
supersecret
```

### Development

After the initial setup you can simply run `docker compose up` and `yarn run start`.

### Migration workflow

Unfortunately DX when generating migrations which extend or relate to core entities is not great, so here's a workflow that works for me:

1. Make sure `yarn run start` is running
2. If needed, copy and edit `medusa-plugin-bundles/.env.example` to `medusa-plugin-bundles/.env`
3. Edit/create migration files in `medusa-plugin-bundles/src/migrations`
4. `npx typeorm migration:generate -d datasource.js src/migrations/BundleUpdate` - this will auto generate a bunch of migrations in `src/migrations/<timestamp>-BundleUpdate.ts` file for you, the migration file will contain migrations for both core medusa entities + your plugin entities
5. `npx typeorm migration:create src/migrations/BundleUpdate` - this will generate an empty migration file for you, you can then cherry pick the migrations you want to run from the previously auto generated file and copy them over to this file, after that you can delete the auto generated file
6. In the `dev/medusa` dir run `medusa migrations run`
7. ... ???

<!-- 1. `docker-compose up`
2. Open a new terminal tab
3. `yarn install`
5. `yarn workspace medusa-plugin-bundles run watch`
6. Open a new terminal tab
8. `yarn workspace medusa run seed`
9. `yarn workspace medusa run start`
10. Open a new terminal tab
11. `yarn workspace medusa-storefront run start` -->

<!-- 3. `yarn run watch`
4. In a new terminal tab run `cd dev/medusa`
6. `medusa develop`
7. In a new terminal tab run `cd dev/medusa-storefront`
8. `npm run dev` -->
