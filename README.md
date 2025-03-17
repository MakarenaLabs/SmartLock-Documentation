# SmartLock Documentation

Documentation of the SmartLock project

## Prerequisites

- Python3
- Python virtual environment (`python3-venv`)

## Virtual environment installation

Starting from the root directory of the project, you have to install the virtual environment in order to build and serve
the documentation.
To do that, please install the virtual environment using

### Linux
```bash
python3 -m venv venv
```

### Windows
```powershell
python.exe -m venv venv
```

A directory called `venv` should be created in the current directory.

## Environment configuration

First of all, you have to activate the virtual environment. Please run

### Linux
```bash
source venv/bin/activate
```

### Windows
```powershell
.\venv\Scripts\activate
```


Once done, go ahead installing all the requirements with

```bash
pip install -r requirements.txt
```

## Documentation real-time testing

While the virtual environment is still active, serve the documentation running

```bash
mkdocs serve
```

An example of complete terminal flow should be very close to

```bash
/path/to/doc $ python3 -m venv venv
/path/to/doc $ source venv/bin/activate
(venv) /path/to/doc $ pip install -r requirements.txt 
[...]
(venv) /path/to/doc $ mkdocs serve
INFO    -  Building documentation...
INFO    -  Cleaning site directory
INFO    -  The following pages exist in the docs directory, but are not included in the "nav" configuration:
             - boards/amd-kria-kv260/testbench.md
INFO    -  Doc file 'boards/amd-kria-kv260/initial-setup-configuration.md' contains an absolute link '/hw-sw-requirements', it was left as is. Did you mean '../../hw-sw-requirements.md'?
INFO    -  Documentation built in 0.07 seconds
INFO    -  [15:18:43] Watching paths for changes: 'markdown', 'mkdocs.yaml'
INFO    -  [15:18:43] Serving on http://127.0.0.1:8000/
```

At the address http://127.0.0.1:8000 you should find the documentation.

## Authors

[Staff MakarenaLabs](mailto:staff@makarenalabs.com)