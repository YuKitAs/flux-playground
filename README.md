# Flux Playground

* Bootstrap Flux for staging/production (set `kubectl` context `flux-staging` and `flux-prod` in `~/.kube`):

```console
$ flux bootstrap github --context=flux-staging --owner=${GITHUB_USER} --repository=flux-playground --branch=main --personal --path=clusters/staging
$ flux bootstrap github --context=flux-prod --owner=${GITHUB_USER} --repository=flux-playground --branch=main --personal --path=clusters/prod
```

It will commit the component and sync manifests in `clusters/{staging,prod}/flux-system/` directory and create a deploy key with read-only access on GitHub in order to pull changes inside the corresponding cluster.

* List HelmRelease objects:

```console
$ flux get helmreleases --all-namespaces
```

* Watch Kustomization statuses:

```console
$ flux get kustomizations -w
```

* Clean up Flux components, custom resources and namespace:

```console
$ flux uninstall --namespace=flux-system [--dry-run]
```
