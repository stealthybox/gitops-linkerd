# gitops-linkerd

Progressive Delivery with Linkerd, Flagger and Flux v2

## Prerequisites

In order to install the workshop prerequisites you'll need a Kubernetes cluster 1.18
or newer with Load Balancer support and RBAC enabled.

### Install Flux v2

Install the CLI on MacOS or Linux using Homebrew:

```sh
brew install fluxcd/tap/flux yq
```

Verify that your cluster satisfies the prerequisites with:

```console
$ flux check --pre

► checking prerequisites
✔ kubectl 1.19.2 >=1.18.0
✔ Kubernetes 1.18.9 >=1.16.0
✔ prerequisites checks passed
```

Fork this repo, and create a [Personal Access Token](https://github.com/settings/tokens) with `repo` access.
Then export your github-user, repo name, and PAT.

```console
export GITHUB_USER=stealthybox
export GITHUB_TOKEN="$(cat ~/.config/gh/flux-bootstrap-demos.pat)"
export GITHUB_REPO=gitops-linkerd
```

Bootsrap Flux and your repository into your cluster.
If you do not have one, you can create one with `kind create cluster`

```console
flux bootstrap github \
  --personal \
  --owner "${GITHUB_USER}" \
  --repository "${GITHUB_REPO}" \
  --path "clusters/kind0"
```

If you use a different `--path`, be sure to copy the `./flux-system-config` directory to it.

The cluster will now boot a dependency tree including Flux, Linkerd, Contour, Flagger, and the frontend/backend podinfo workloads.

From here, you can carry on with Progressive Delivery experiments using the Flagger Canary objects for the frontend and backend.
See the [EKS handson, Canary Releases and Canary Tests](https://eks.handson.flagger.dev/canary/#application-bootstrap) sections for more references.
