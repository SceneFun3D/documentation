## SceneFun3D documentation

Official documentation of the SceneFun3D project.

## Instal dependencies

**Download the repository**:
```
git clone https://github.com/SceneFun3D/documentation.git
cd documentation
```

**Create virtual environment**:
```
conda create --name docs python=3.8
conda activate docs
pip install -r requirements.txt
```

## Commands

* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        assets/   # contains the image assets
        index.md  # The documentation homepage.
        ...       # Other markdown pages organized in sections

## Acknowledgements

This documentation page is based on the [Material for MkDocs](https://github.com/squidfunk/mkdocs-material) template.


