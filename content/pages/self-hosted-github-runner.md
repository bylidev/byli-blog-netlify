---
type: PostLayout
title: Run self-hosted github-runner
date: '2022-10-10'
author: content/data/person1.json
excerpt: >-
  Facilisis dui. Nulla molestie risus in mi dapibus, eget porta lorem semper.
  Donec sed facilisis nibh.
featuredImage:
  type: ImageBlock
  url: /images/abstract-feature1.svg
  altText: Thumbnail
  elementId: ''
  styles:
    self:
      padding:
        - pt-0
        - pl-0
        - pb-0
        - pr-0
bottomSections:
  - type: DividerSection
    title: Divider
    elementId: ''
    colors: bg-light-fg-dark
    styles:
      self:
        padding:
          - pt-3
          - pl-3
          - pb-3
          - pr-3
  - type: RecentPostsSection
    title:
      type: TitleBlock
      text: Recent posts
      color: text-dark
      styles:
        self:
          textAlign: center
    recentCount: 3
    showThumbnail: true
    showExcerpt: true
    showDate: true
    showAuthor: true
    actions: []
    elementId: ''
    variant: three-col-grid
    colors: bg-light-fg-dark
    styles:
      self:
        justifyContent: center
slug: self-hosted-github-runner
isFeatured: false
isDraft: false
seo:
  type: Seo
  metaTitle: lorem-ipsum
  metaDescription: lorem-ipsum
  addTitleSuffix: false
  metaTags: []
colors: bg-light-fg-dark
styles:
  self:
    flexDirection: col
---
# Why Self-Hosted Runners?

Self-hosted GitHub Runners let you:

*   Run on your own servers for tailored performance.

*   Keep sensitive workflows secure.

*   Avoid GitHub-hosted runner quotas.

# Github-Runner docker compose

```
```

# The CI/CD Pipeline

We define our pipeline in `.github/workflows/ci.yaml` . This example triggers on pushes to main or tagged releases (`e.g., v1.0.0` ), builds a Docker image, pushes it to GitHub Container Registry (GHCR), and deploys it as a **stack.yaml**

```
name: CI Pipeline
on:
  push:
    branches:
      - main
    tags:
      - 'v*.*.*'
env:
  PROJECT_NAME: ${{ github.event.repository.name }}
  TAG_NAME: ${{ github.ref_name }}
  IMAGE: ghcr.io/${{ github.repository_owner }}/${{ github.event.repository.name }}:${{ github.ref_name }}
jobs:
  build-and-deploy:
    runs-on: self-hosted
    steps:

      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build Docker Image
        run: |
          docker build . \
            -t $IMAGE

      - name: Log in to GitHub Container Registry
        if: startsWith(github.ref, 'refs/tags/')
        run: |
          echo "${{ secrets.GIT_TOKEN }}" | docker login ghcr.io -u ${{ github.actor }} --password-stdin

      - name: Push Docker Image
        if: startsWith(github.ref, 'refs/tags/')
        run: docker push $IMAGE

      - name: Deploy Docker Stack
        if: startsWith(github.ref, 'refs/tags/')
        run: |
          sed -i "s|__IMAGE__|$IMAGE|" ./docker-compose.yml
          sed -i "s|__REG_TOKEN__|${{ secrets.REG_TOKEN }}|" ./docker-compose.yml
          mv ./docker-compose.yml /stacks/github/$PROJECT_NAME.yml
          docker stack deploy --compose-file /stacks/github/$PROJECT_NAME.yml $PROJECT_NAME >> /dev/null
```

# How It Works

*   **Triggers**: Runs on push to **main** or tags like **v1.0.0**.

*   **Environment**:

    *   Sets envs for the Docker build.

    *   Defines IMAGE as **ghcr.io/owner/repo:tag** (e.g., ghcr.io/my-org/my-app:v1.0.0-prod-1).

*   **Job**:

    *   Build: Creates a Docker image with args for OS, architecture, and runner version.

    *   Push: Logs into GHCR and pushes the image, but only for tagged releases.

    *   Deploy: Updates docker-compose.yml with the image and a registration token, moves it to /stacks/github/, and deploys via Docker Swarm.

*   Runner: Uses your self-hosted runner, which needs Docker and write access to /stacks/github/.

## Setup Tips

1.  **Runner**: Add a self-hosted runner in Settings > Actions > Runners. Install Docker on it.

2.  **Secrets**: Store GIT\_TOKEN for GHCR login and REG\_TOKEN for deployment in GitHub Secrets.

3.  **Permissions**: Ensure the runner can write to /stacks/github/. Use sudo if needed.

# Conclusion

This ci.yaml automates Docker image builds and deployments with your self-hosted GitHub Runner. Customize it for tests or other steps, and enjoy fast, secure CI/CD tailored to your needs.
