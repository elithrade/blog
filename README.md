# My personal blog

This repo is made using [MkDocs](https://www.mkdocs.org/user-guide/deploying-your-docs/).

The site is hosted at https://elithrade.github.io/blog/ using GitHub Pages. This repo currently only contains some technical study notes although it is called blog.

All the files are in `Markdown` located under `/docs` folder.

[MkDocs](https://www.mkdocs.org/user-guide/deploying-your-docs/) is a static site generator geared towards (technical) project documentation.
This repo is made using [Material for MkDocs]. For local installation, `Python` and `pip` is required.

`pip` can be installed on Ubuntu 20.04 using:
```
sudo apt install python3-pip
```
then install `mkdocs-material` using:
```
pip install mkdocs-material
```

# Bootstrap a new site
After installing `mkdocs-material`, you can bootstrap your project documentation using the `mkdocs` executable. Go to the directory where you want your project to be located and enter:
```
mkdocs new .
```
This will create the following structure:
```
.
├─ docs/
│  └─ index.md
└─ mkdocs.yml
```

`mkdocs.yml` contains the configuration used by `mkdocs`, such as `site_url`, the name of the site and themes.
```yml
theme:
  name: material

site_name: Memory blocks
site_url: https://elithrade.github.io/blog/

markdown_extensions:
  - pymdownx.highlight
  - pymdownx.superfences
```

Once the basic site is bootstraped, we can start writing and see the live changes.

# Writing documentation
`MkDocs` allows you to write documentations in `Markdown` and it will generate `html` pages.

`MkDocs` includes a live preview server, so you can preview your changes as you write your documentation. The server will automatically rebuild the site upon saving. Start it with:
```
mkdocs serve
```
When you're finished editing, you can build a static site from your Markdown files with:
```
mkdocs build
```
`./site` will be generated that contains all the `html` pages needed to host the site.

# Integrate with GitHub Pages
We can integrate the static site with with `{username}.github.io`.
We need to:
1. Create a new repo in GitHub
2. Push the content to repo
3. (Optional) Update `mkdocs.yml` by adding `site_url` and pointing to `{username}.github.io/{repo_name}`. It will always use the repo name as `site_url`.
```
git init
git add .
git commit -m 'Initial commit'
git remote add origin git@github.com:{username}/{repo_name}.git
git branch -M main
git push -u origin main
```

## Publish locally
Push the local static content to GitHub Pages.
```
mkdocs gh-deploy --force
```

# Integrate with GitHub Actions
We can create a GitHub Action to trigger on merging into main and does a auto deployment.

```yml
name: ci
on:
  push:
    branches:
      - master
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: 3.x
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force
```
