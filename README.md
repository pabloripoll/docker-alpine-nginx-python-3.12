<div style="width:100%;float:left;clear:both;margin-bottom:50px;">
    <a href="https://github.com/pabloripoll?tab=repositories">
        <img style="width:150px;float:left;" src="https://pabloripoll.com/files/logo-light-100x300.png"/>
    </a>
</div>

# Python 3.12 Docker - Base Container

[THIS REPOSITORY IS UNDER DEVELOPMENT]

## Infrastructure

- Alpine : 3.20
- Nginx : 1.24
- Python : 3.12.3
- Pip : 24.0

## Backend Service

Build the backend service container
```bash
$ make project-create
```

Get into container
```bash
$ make backend-ssh

/var/www/htdocs $
```

It cannot be installed any package as `$ ptyhon -m pip install <some-package>`
```bash
/var/www/htdocs $ python -m pip install fastapi "uvicorn[standard]"
error: externally-managed-environment

× This environment is externally managed
╰─>
    The system-wide python installation should be maintained using the system
    package manager (apk) only.

    If the package in question is not packaged already (and hence installable via
    "apk add py3-somepackage"), please consider installing it inside a virtual
    environment, e.g.:

    python3 -m venv /path/to/venv
    . /path/to/venv/bin/activate
    pip install mypackage

    To exit the virtual environment, run:

    deactivate

    The virtual environment is not deleted, and can be re-entered by re-sourcing
    the activate file.

    To automatically manage virtual environments, consider using pipx (from the
    pipx package).

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
```

So, placed in `/var/www/htdocs` directory create an enviroment
```bash
$ python3 -m venv . && . bin/activate
```

Then it can be install any app
```bash
$ pip install fastapi "uvicorn[standard]"
```

Location where packages are installed
```bash
$ python -m pip show pip
```

## Project Destroy

```bash
$ make project-stop
$ make project-destroy
$ sudo docker system prune

WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - unused build cache

Are you sure you want to continue? [y/N] y
Deleted build cache objects:
sjwjg882ehmjpljh8niohnnhg
...
sfv5qrevgtoh4aeavzlcfp6uv

Total reclaimed space: 137.2MB
```

## Automation commands

```bash
$ make help

usage: make [target]

targets:
Makefile  help                   shows this Makefile help message
Makefile  hostname               shows local machine ip
Makefile  fix-permission         sets project directory permission
Makefile  host-check             shows this project ports availability on local machine
Makefile  project-set            sets the project enviroment file to build the container
Makefile  project-create         creates the project container from Docker image
Makefile  project-start          starts the project container running
Makefile  project-stop           stops the project container but data won't be destroyed
Makefile  project-destroy        removes the project from Docker network destroying its data and Docker image
Makefile  backend-ssh            enters the backend container shell
Makefile  backend-update         updates the backend set version into container
Makefile  repo-flush             clears local git repository cache specially to update .gitignore
Makefile  repo-commit            echoes common git commands
```