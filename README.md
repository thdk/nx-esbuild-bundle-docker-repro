# Repro

Install step in dockerfile remains required even when app is bundled.

```sh
npm i
```

```sh
npx nx docker-build api
# or if using podman
npx nx podman-build api
```

```sh
docker run api
# or if using podman
podman run localhost/api
```

Expected:

# How this repro project was created

```sh
npx create-nx-workspace@latest nx-esbuild-app --preset ts
cd nx-esbuild-app
npm i -D @nx/node
npx nx g @nx/node:app --name api --directory apps/api --framework fastify --docker --e2eTestRunner none --projectNameAndRootFormat as-provided
```

(Adjust api/project.json to also have a `podman-build` target)
