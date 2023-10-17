# Part 4: Next.js + Codespaces + GitHub Pages

In this lab you'll build a full-fledged Next.js website using GitHub Codespaces, GitHub Pages, and GitHub Actions.

1. Go to [https://github.com/codespaces/templates](https://github.com/codespaces/templates) and select `Next.js`. This opens a new Codespace with a fully built out Next.js template and running development server.
2. Right now, this Codespace is not attached to a repo. Select the version control tab and click `Publish Branch`. This prompts you to create a private or public repo. You can change the name of the repo by editing the text in the text field. Select `public` to create a public repo.

<img width="617" alt="cs-repo" src="https://github.com/mor10/github-for-the-rest-of-us/assets/1132200/dfb45251-2a6f-4725-a568-05ff9cc3c4c8">

3. Use the version control tools to create a new branch named `gh-pages` and publish it.
4. Open the current repo in a separate browser tab.
5. Go to `Settings > Pages` and select the `gh-pages` branch.

<img width="1146" alt="gh-pages" src="https://github.com/mor10/github-for-the-rest-of-us/assets/1132200/248c6e7d-d51a-48dd-b3c3-1a2b5c8ffa6a">

6. At this point you should be able to follow the URI at the top of `Settings > Pages` to a landing page for Next.js.
7. Create a new file titled `config.next.js`
8. In `config.next.js`, add the following code:

```js
const isGithubActions = process.env.GITHUB_ACTIONS || false

let assetPrefix = ''
let basePath = ''

if (isGithubActions) {
  const repo = process.env.GITHUB_REPOSITORY.replace(/.*?\//, '')

  assetPrefix = `/${repo}/`
  basePath = `/${repo}`
}

module.exports = {
  assetPrefix: assetPrefix,
  basePath: basePath,
}
```
9. Create a new folder called `.github`. Make sure to include the punctuation mark.
10. Inside the `.github` folder, create a new folder titled `workflows`.
11. Inside the `.github/workflows` folder, create a new file titled `publish.yml`.
12. In `publish.yml`, add the following code:

```yml
name: Node.js CI

on:
  push:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
      # https://github.com/actions/checkout
      - uses: actions/checkout@v3
        
      # a standard step for GitHub actions on Node
      # https://github.com/actions/setup-node
      - uses: actions/setup-node@v3
        with:
          # update the Node version to meet your needs
          node-version: 16
          cache: npm

      - name: Build
        run: |
          npm ci
          npm run build
          npm run export
          touch out/.nojekyll

      - name: Deploy
        # https://github.com/JamesIves/github-pages-deploy-action
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          branch: gh-pages
          folder: out
```

11. Checkout the `main` branch.
12. Stage and commit the new files to the `main` branch.
13. Sync the new changes to the repo.
14. Go to the repo and click on the `Actions` tab. You should now see the action kick in.
15. Go back to Codespaces and create a new file in the `pages` folder titled `about.js`
