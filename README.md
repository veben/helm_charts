# Helm Charts
This repository contains Helm charts.
The associated codes are stored in **Github**. The associated artifacts are stored in **Github pages**.

## I. Configuration of the repository
### 1. Activate Github Pages on the repo:
- Create `gh-pages` branch:
```sh
git checkout -b gh-pages
git commit -m "Github pages branch" --allow-empty
git push --set-upstream origin gh-pages
```
- Go to Github Pages section from your repository following `Settings > Pages`
- Choose `Deploy from a branch` and choose `gh-pages`

### 2. Create a personal token
- Click on your Github avatar
- Go to `Settings > Developer settings > Personal access tokens > Tokens (classic)`
- Create a new classic token with `repo` and `write:packages`

### 3. Define CI token
- Go to the repository's settings
- Open `Secrets and variables > Acions`
- Create a new **repository secret** named `CR_TOKEN` with your personal token as value

### 4. Create a helm chart
Define in helm chart in the `charts` folder

### 5. Create the CI
Create a GitHub Actions Workflow `.github/workflows/release.yml` in the main branch and add the following configuration. This will now turn our repo to self-hosted Helm Chart repo. With every push to the main branch it will check the chart and if there is a new chart version creates a corresponding GitHub release, adds Helm chart artefacts to the release and creates a deployment which will be then hosted on GitHub Pages.
```
name: Release Charts

on:
  push:
    branches:
      - main

jobs:
  release:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Configure Git
        run: |
          git config user.name "$GITHUB_ACTOR"
          git config user.email "$GITHUB_ACTOR@users.noreply.github.com"

      - name: Install Helm
        uses: azure/setup-helm@v4
        env:
          GITHUB_TOKEN: "${{ secrets.CR_TOKEN }}"

      - name: Run chart-releaser
        uses: helm/chart-releaser-action@v1.6.0
        env:
          CR_TOKEN: "${{ secrets.CR_TOKEN }}"
```

## II. Verifications
- On the repository, very multiple things
  - Click in `Actions` and verify the pipelines has been executed and are green
  - Switch to `gh-pages` and verify a `index.yaml` file has been created
  - Look at **Releases** section and verify it contains a release of your chart
  - Look at **Deployments** section and verify it contains a deployment of your chart
  - Verify the URL `https://<user>.github.io/helm_charts/` is pointing to a github page exposing the readme
  - Verify the URL `https://<user>.github.io/helm_charts/index.yaml` is exposing the release's metadata

## III. Deployment of a chart
### 1. Configure and deploy a Kubernetes cluster
You can for example deploy a small cluster with **Civo**
> See: https://github.com/veben/civo_k8s_easy_cluster/blob/main/readme.md

### 2. Install **Helm**
> See: https://helm.sh/docs/intro/install/

### 3. Deploy helm chart
- Add the repository:
```sh
helm repo add helm_charts https://veben.github.io/helm_charts/
```
- Perform an helm update to fetch latest
```sh
helm repo update
```
- Search the charts contained in the repository
```sh
helm search repo helm_charts
```
- Install the chart on your cluster
```sh
helm upgrade --install helm_charts/mario-bros
```
