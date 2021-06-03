# Custom Styles, Metadata, and Features

## Custom Styles

If you'd like to edit the default configurations, styles, production tasks, or templates, you can do so by editing these files:

- `assets/`: This folder contains the files for configurations (`defaults/`), styles (`styles/`), dependencies (`lib/`), scripts (`scripts/`,), and templates (`templates/`) Lantern uses to produce the output formats. 

- `Makefile`: This file defines the core commands Lantern uses to produce your output files.

## Custom Metadata

You can define new metadata fields in the `metadata.yml` file by creates new [YAML keys](https://learnxinyminutes.com/docs/yaml/) and referencing those keys in the [output templates](https://github.com/nulib-oer/lantern/tree/main/assets/templates). The Pandoc manual has more information about customizing [templates](https://pandoc.org/MANUAL.html#templates) and [metadata](https://pandoc.org/MANUAL.html#metadata-variables).  