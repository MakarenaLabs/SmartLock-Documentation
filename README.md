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

Once done, please install all the requirements with
```bash
pip install -r requirements.txt
```

An example of terminal output should be very close to
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

## Documentation real-time testing
To test the documentation, please use
```bash
mkdocs serve
```

## Authors
[Staff MakarenaLabs](mailto:staff@makarenalabs.com)