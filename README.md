# Repro

Install step in dockerfile remains required even when app is bundled.

```sh
npm i
```

```sh
npx nx docker-build api
```

```sh
docker run api
```

Expected:

```sh
❯ podman run localhost/api
{"level":30,"time":1705253602761,"pid":1,"hostname":"b5589a64d8ea","msg":"Server listening at http://0.0.0.0:3000"}
[ ready ] http://0.0.0.0:3000
```

Actual:

```sh
❯ podman run localhost/api
node:internal/modules/cjs/loader:1051
  throw err;
  ^

Error: Cannot find module 'fastify'
Require stack:
- /app/api/main.js
    at Module._resolveFilename (node:internal/modules/cjs/loader:1048:15)
    at Module._load (node:internal/modules/cjs/loader:901:27)
    at Module.require (node:internal/modules/cjs/loader:1115:19)
    at require (node:internal/modules/helpers:130:18)
    at Object.<anonymous> (/app/api/main.js:25:30)
    at Module._compile (node:internal/modules/cjs/loader:1241:14)
    at Module._extensions..js (node:internal/modules/cjs/loader:1295:10)
    at Module.load (node:internal/modules/cjs/loader:1091:32)
    at Module._load (node:internal/modules/cjs/loader:938:12)
    at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:83:12) {
  code: 'MODULE_NOT_FOUND',
  requireStack: [ '/app/api/main.js' ]
}

Node.js v20.9.0
```

# How this repro project was created

```sh
npx create-nx-workspace@latest nx-esbuild-app --preset ts
cd nx-esbuild-app
npm i -D @nx/node
npx nx g @nx/node:app --name api --directory apps/api --framework fastify --docker --e2eTestRunner none --projectNameAndRootFormat as-provided
```
