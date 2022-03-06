# Flux Playground

A minimal example to show how to manage multiple K8s clusters with [Flux](https://fluxcd.io/docs/), Kustomize and Helm.

* Flux version: 0.26.3

* Create K8s clusters for staging and production e.g. with [kind](https://kind.sigs.k8s.io/docs/user/quick-start/). Set `kubectl` context `flux-staging` and `flux-prod` in `~/.kube/config` for namespace `flux-system`.

* Export `GITHUB_TOKEN` and `GITHUB_USER`

* Check prerequisites with `flux check --pre`

* Bootstrap Flux for staging/production:

  ```console
  $ flux bootstrap github --context=flux-staging --owner=${GITHUB_USER} --repository=flux-playground --branch=main --personal --path=clusters/staging
  $ flux bootstrap github --context=flux-prod --owner=${GITHUB_USER} --repository=flux-playground --branch=main --personal --path=clusters/prod
  ```

  It will commit the component and sync manifests in `clusters/{staging,prod}/flux-system/` directory in the GitHub repository, and create a deploy key with read-only access on GitHub in order to pull changes inside the corresponding cluster.

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

Project structure:

```
├── apps
│   ├── base
│   │   └── podinfo
│   │       ├── kustomization.yaml
│   │       ├── namespace.yaml
│   │       └── release.yaml
│   ├── prod
│   │   ├── kustomization.yaml
│   │   └── podinfo-patch.yaml
│   └── staging
│       ├── kustomization.yaml
│       └── podinfo-patch.yaml
├── clusters
│   ├── prod
│   │   ├── apps.yaml
│   │   └── infrastructure.yaml
│   └── staging
│       ├── apps.yaml
│       └── infrastructure.yaml
├── infrastructure
│   ├── kustomization.yaml
│   ├── nginx
│   │   ├── kustomization.yaml
│   │   ├── namespace.yaml
│   │   └── release.yaml
│   └── sources
│       ├── bitnami.yaml
│       ├── kustomization.yaml
│       └── podinfo-source.yaml
```
