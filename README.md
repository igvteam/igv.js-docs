
This repository contains the source for the documentation for igv.js (https://github.com/igvteam/igv.js).
The documentation is Markdown based and the website is generated using the [MkDocs](https://www.mkdocs.org/) static site generator.

To get started, pull this repo and then:
- `conda env create -f igv.js-docs.yml` to install the Conda environment with necessary dependencies.
- `conda activate igv.js-docs` to activate the environment (the exact command might differ for Anaconda).
- `mkdocs serve` to start the MkDocs live-preview web server.
    -  Note: if you get an error concerning ```jinja2``` try the following `pip install jinja2==3.0.0`
    
- `mkdocs build` to build the static site. A subdirectory named `site` will be created.

Note: you might prefer `conda config --set auto_activate_base false`

We're using [Python Markdown](https://python-markdown.github.io/) in the [MkDocs](https://www.mkdocs.org/) static site generator.
With the `extra` [package of extensions](https://python-markdown.github.io/extensions/extra/) we're using it "mostly immitates" 
[PHP Markdown](https://michelf.ca/projects/php-markdown/extra/).

Be sure to update the `mkdocs.yml` file if you add, remove, or rename any pages since that is where the TOC navigation is defined.
Pages not specified there will be included in the site but not shown in the TOC.

We use the [Windmill template](https://github.com/gristlabs/mkdocs-windmill) for MkDocs.
