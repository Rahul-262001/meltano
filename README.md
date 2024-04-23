# Documentation
## Meltano 
* This is an ETL Tool 
> **Note:**  Meltano is an application, it should always be installed into a clean virtual environment without any other packages installed alongside it.
## Installation
1. Create and navigate to a directory to hold your Meltano projects:
``` BASH
mkdir meltano-projects
cd meltano-projects
```
2. Install the pipx package manager:
``` BASH
#For Windows (PowerShell): New-Alias Python3 Python
python -m pip install --user pipx
python -m pipx ensurepath
```
> For Windows (PowerShell): Open up a new powershell instance to load your new path variables

3. Install meltano
```BASH
pipx install meltano
```
4. Verify the installation
```BASH
meltano --version
```

## Getting Data from mysql
1. Creating a new project
```BASH
meltano init my-meltano-project
```
2. Add an Extractor to Pull Data from a Source
``` BASH
meltano add extractor tap-mysql --variant=meltanolabs
```
> ### Note: Might take some time.
3. follow the instruction to create the yml file
``` BASH
meltano invoke tap-mysql --help
```

>### Might look something like this
``` yml
version: 1
default_environment: dev
project_id: 6bff755e-f038-4f64-af20-5df45d589725
environments:
- name: dev
- name: staging
- name: prod
plugins:
  extractors:
  - name: tap-mysql
    variant: transferwise
    pip_url: pipelinewise-tap-mysql
    config:
      database: example_db
      port: 3306
      user: root
      host: localhost
```
