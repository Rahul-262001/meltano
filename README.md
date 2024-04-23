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
meltano config tap-mysql set --interactive
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
4. Sample Target
> This needs no configuration so good for testing
``` bash
meltano add loader target-jsonl --variant=andyh1203
```

5. Check the selected attribute
``` BASH
meltano select tap-mysql --list --all
```

6. Finally Sending the data
``` BASH
meltano run tap-mysql target-jsonl
```
> Note: usually all the contents of the database might be sent from tap so be careful
## OR
6. if properties.config file (catalog file) is available use this 
```BASH
meltano invoke tap-mysql --catalog properties.json | meltano invoke target-jsonl
```

> Note: all the output will be stored in the output folder

## The Sample python code, Just to get an idea
``` python
import json
import subprocess
result = subprocess.run("meltano invoke tap-mysql --catalog properties.json", capture_output=True, text=True)
print(result.stdout)
```

``` Json
{"type": "STATE", "value": {"currently_syncing": "example_db-animals"}}
{"type": "SCHEMA", "stream": "example_db-animals", "schema": {"properties": {"likes_getting_petted": {"inclusion": "available", "minimum": -128, "maximum": 127, "type": ["null", "integer"]}, "name": {"inclusion": "available", "maxLength": 255, "type": ["null", "string"]}, "id": {"inclusion": "available", "minimum": -2147483648, "maximum": 2147483647, "type": ["null", "integer"]}}, "type": "object"}, "key_properties": ["id"]}
{"type": "ACTIVATE_VERSION", "stream": "example_db-animals", "version": 1713860083046}
{"type": "RECORD", "stream": "example_db-animals", "record": {"likes_getting_petted": 0, "name": "aardvark", "id": 1}, "version": 1713860083046, "time_extracted": "2024-04-23T08:14:43.065251Z"}
{"type": "RECORD", "stream": "example_db-animals", "record": {"likes_getting_petted": 0, "name": "bear", "id": 2}, "version": 1713860083046, "time_extracted": "2024-04-23T08:14:43.065251Z"}
{"type": "RECORD", "stream": "example_db-animals", "record": {"likes_getting_petted": 1, "name": "cow", "id": 3}, "version": 1713860083046, "time_extracted": "2024-04-23T08:14:43.065251Z"}
{"type": "STATE", "value": {"currently_syncing": "example_db-animals"}}
{"type": "ACTIVATE_VERSION", "stream": "example_db-animals", "version": 1713860083046}
{"type": "STATE", "value": {"currently_syncing": "example_db-animals", "bookmarks": {"example_db-animals": {"initial_full_table_complete": true}}}}
{"type": "STATE", "value": {"currently_syncing": null, "bookmarks": {"example_db-animals": {"initial_full_table_complete": true}}}}

```
 ~~~All Done!!!!~~~

# important url 
``` url
https://github.com/z3z1ma/target-bigquery
```
```url
https://hub.meltano.com/loaders/target-bigquery/
```
``` url
https://docs.meltano.com/getting-started/part1/#configure-the-extractor
```
``` url
https://docs.meltano.com/getting-started/
```
```url
https://meltano.com/
```
