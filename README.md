# Reusable Workflows
> collection of reusable workflows for GitHub Actions

This collection of [Reusable Workflows](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows) can be used in any GitHub Action.

## Examples

### Node job workflow

> checkout, install dependencies and run a node command script

```yml
prettier:
    uses: kerhub/reusable-workflows/.github/workflows/node-job.yml@main
    with:
      command: npm run prettier --check \"**\"
```

### Netlify preview workflow

> build the app and deploy it to a preview Netlify channel

```yml
deploy_preview:
    name: 'deploy preview'
    if: github.event_name == 'pull_request'
    needs: [prettier, linter, unit_tests]
    uses: kerhub/reusable-workflows/.github/workflows/netlify-preview-deploy.yml@main
    with:
      build_directory: './output/dist'
    secrets:
      netlifyAuthToken: "${{ secrets.NETLIFY_AUTH_TOKEN }}"
      netlifySiteId: "${{ secrets.NETLIFY_SITE_ID }}"
      repoToken: "${{ secrets.GITHUB_TOKEN }}"
```

### Netlify live workflow

> build the app and deploy it to the live Netlify channel

```yml
deploy_live:
    name: 'deploy live'
    if: github.event_name == 'push'
    needs: [prettier, linter, unit_tests]
    uses: kerhub/reusable-workflows/.github/workflows/netlify-live-deploy.yml@main
    with:
      build_directory: './output/dist'
    secrets:
      netlifyAuthToken: "${{ secrets.NETLIFY_AUTH_TOKEN }}"
      netlifySiteId: "${{ secrets.NETLIFY_SITE_ID }}"
```

### Lighthouse preview

> audit a preview channel with Lighthouse

```yml
lighthouse_preview:
    name: 'lighthouse preview'
    needs: deploy_preview
    uses: kerhub/reusable-workflows/.github/workflows/lighthouse-preview.yml@main
    with:
      siteName: 'dojo-realworld'
    secrets:
      netlifyAuthToken: "${{ secrets.NETLIFY_AUTH_TOKEN }}"
```

### Lighthouse live

> audit a live channel with Lighthouse

```yml
lighthouse_live:
    name: 'lighthouse live'
    needs: deploy_live
    uses: kerhub/reusable-workflows/.github/workflows/lighthouse-live.yml@main
    with:
      siteUrl: 'https://dojo-realworld.netlify.app/'
```