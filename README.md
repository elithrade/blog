# My personal blog

This repo is made using [MkDoc](https://www.mkdocs.org/user-guide/deploying-your-docs/).

All the files are in `Markdown` located under `/docs` folder.

[MkDoc](https://www.mkdocs.org/user-guide/deploying-your-docs/) is a static site generator geared towards (technical) project documentation.
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
