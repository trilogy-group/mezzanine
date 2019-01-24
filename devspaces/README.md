# Development with Devspaces

### Devspaces 

Manage your Devspaces https://www.devspaces.io/.

Read up-to-date documentation about cli installation and operation in https://www.devspaces.io/devspaces/help.

Here follows the main commands used in Devspaces cli.

|action   |Description                                                                                   |
|---------|----------------------------------------------------------------------------------------------|
|`devspaces --help`                    |Check the available command names.                               |
|`devspaces create [options]`          |Creates a DevSpace using your local DevSpaces configuration file |
|`devspaces start <devSpace>`          |Starts the DevSpace named \[devSpace\]                           |
|`devspaces bind <devSpace>`           |Syncs the DevSpace with the current directory                    |
|`devspaces info <devSpace> [options]` |Displays configuration info about the DevSpace.                  |

Use `devspaces --help` to know about updated commands.

#### Development flow

You should have Devspaces cli services started and logged to develop with Devspaces.
The following commands should be issued from **project directory**.

1 - Create Devspaces

```bash
$ cd devspaces/docker
$ devspaces create
$ cd ../../

```

2 - Start containers

```bash
devspaces start mezzanine
```

3 - Start containers synchronization

```bash
devspaces bind mezzanine
```

4 - Grab some container info

```bash
devspaces info mezzanine
```

5 - Connect to development container

```bash
devspaces exec mezzanine
```

6 - Build Mezzanine

```bash
python setup.py install
```

7 - Create default site

```bash
mezzanine-project test_project
cd test_project
pip install -r requirements.txt
python manage.py createdb --noinput
```

8 - Enable site access

Using information grabbed in step 4 look annotate devspaces published DNS name in following format:
  `mezzanine.<devspaces-user>.devspaces.io`

Look for `test_project/local_settings.py`, and if it doesn't exists `test_project/settings.py` (in that order).

```bash
vim test_project/test_project/local_settings.py
```

Update `ALLOWED_HOSTS` entry with DNS name published name:

```bash
ALLOWED_HOSTS = ['localhost', '127.0.0.1', '[::1]', 'mezzanine.<devspaces-user>.devspaces.io']
```

9 - Run Mezzanine

```bash
python manage.py runserver 0.0.0.0:8000
```

Access application URLs:

Using information retrieved in step 4, access the following URL's:

* Application (bound to port 8000):
  * `http://mezzanine.<devspaces-user>.devspaces.io:<published-ports>/admin/`

### Docker Script Manager (CLI)

Currently, we have these command available to work using local docker compose.

```bash
devspaces/docker-cli.sh <command>
```

|action    |Description                                                               |
|----------|--------------------------------------------------------------------------|
|`build`   |Builds images                                                             |
|`deploy`  |Deploy Docker compose containers                                          |
|`undeploy`|Undeploy Docker compose containers                                        |
|`start`   |Starts Docker compose containers                                          |
|`stop`    |Stops Docker compose containers                                           |
|`exec`    |Get into the container                                                    |

#### Development flow

1 - Build and Run `docker-compose` locally.

```bash
devspaces/docker-cli.sh build
devspaces/docker-cli.sh deploy
devspaces/docker-cli.sh start
```

2 - Get into container

```bash
devspaces/docker-cli.sh exec
```

3 - Build Mezzanine

```bash
python setup.py install
```

4 - Create default site

```bash
mezzanine-project test_project
cd test_project
pip install -r requirements.txt
python manage.py createdb --noinput
```

5 - Run Mezzanine

```bash
python manage.py runserver 0.0.0.0:8000
```

Application (bound to port 8000):
  * http://localhost:8000/admin/

6 - Clean build artifacts on host (if needed)

**Obs.:** Clean script ran in docker compose may leave some files behind, that may result in files owned by `root`user in your host OS.
You may complement the cleanup with the scripts below:

```bash
 # seek
ls -dl `find . -type d` | grep root | awk '{print $(NF)}'

 # seek and destroy
ls -dl `find . -type d` | grep root | awk '{print $(NF)}' | xargs sudo rm -rf
```

### Entrypoint Actions

    1 - hold container termination
