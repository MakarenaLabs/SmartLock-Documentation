# SmartLock Documentation

Documentation of the SmartLock project

## Prerequisites

 - Python3
 - Python virtual environment (`python3-venv`)

## Configuration
To start building the documentation, please run
```bash
python3 -m venv venv
```
and then, to activate the virtual environment
```bash
source venv/bin/activate
```

## Documentation real-time testing
To test the documentation, please use
```bash
mkdocs serve --config-file mkdocs_local.yaml
```

## Build the documentation
Login into MuseBox Server (`ssh whopper@musebox.it`)

```bash
cd smartlock-doc/
source venv/bin/activate
mkdocs build --config-file mkdocs_serve.yaml
```
