# `cobraz/setup-gke-gcloud-auth-plugin`

This GitHub Actions installs a pre-requisite for "gcloud container clusters get-credentials"
command with a modern k8s client.

## Quickstart

```yaml
- uses: cobraz/setup-gke-gcloud-auth-plugin@v1
```

### Example

```yaml
name: On Push

permissions:
  id-token: write
  contents: read

on:
  push:
    branches:
      - main

jobs:
  apply-changes:
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Authenticate to GCP
        uses: google-github-actions/auth@v1
        with:
          credentials_json: ${{ secrets.GOOGLE_PROJECT_SA_KEY }}
          project_id: ${{ secrets.GOOGLE_PROJECT_ID }}

      - uses: cobraz/setup-gke-gcloud-auth-plugin@v1

      - uses: google-github-actions/get-gke-credentials@v1
        with:
          cluster_name: my-cluster
          location: us-central1-a

      # At this point you should be able to communicate with your cluster
      - run: kubectl get pods
```
