# Flux Playground

* Bootstrap Flux for staging/production (set `kubectl` context `flux-staging` and `flux-prod` in `~/.kube`):

```console
$ flux bootstrap github --context=flux-staging --owner=${GITHUB_USER} --repository=flux-playground --branch=main --personal --path=clusters/staging
$ flux bootstrap github --context=flux-prod --owner=${GITHUB_USER} --repository=flux-playground --branch=main --personal --path=clusters/prod
```

* List HelmRelease objects:

```console
$ flux get helmreleases --all-namespaces
```

* Watch Kustomization statuses:

```console
$ flux get kustomizations -w
```
