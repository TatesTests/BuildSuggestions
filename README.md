# gitstream-mkdocs

## Setup

Run Docker daemon, and then:

```
docker pull squidfunk/mkdocs-material
```

For Mac M1, better to use different container:
```
docker pull ghcr.io/afritzler/mkdocs-material
```

Note: When creating this repo, the following command was used
```
docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material new .
```

## Run localy

```
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material
```

For Mac M1, its better to use:
```
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs ghcr.io/afritzler/mkdocs-material
```

Then go to http://localhost:8000

## Deploy to GitHub pages

Set up pyhton env:

```
python3 -m venv .venv
. ./.venv/bin/activate
pip install mkdocs-material
mkdocs gh-deploy
```

