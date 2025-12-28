# Deployment code for the Delinquants servers

This repository contains generic collections and specific deployment playbooks used to deploy the delinquants.fr server.

## Usage

### Pre-requisites

* Create an inventory file in `inventory/inventory.yaml` and define the following variables for the target hosts:
  * `delinquants_server_name`: The main server name to configure (e.g.: `www.delinquants.fr`)
* Install the dependencies from ansible-galaxy:

```sh
ansible-galaxy role install -r requirements.yml
```

* Deploy the web server with the deployment playbook:

```sh
ansible-playbook deploy.yml --diff
```

### Optional configuration variables

* `delinquants_server_additional_dns`: An optional list of alternative server names that will redirect to the main server name set in `delinquants_server_name`.
* `configure_knockd`: Toggle knockd configuration

## Documentation generation

To generate a web version of the documentation, use `antsibull-docs` version `2.23.1` or higher:

* Install antsibull-docs in a virtual environment:

```sh
python -m venv .venv
source .venv/bin/activate
pip install antsibull-docs
```

* Initialize the documentation build directory:

```sh
mkdir --mode 0700 built-docs
antsibull-docs sphinx-init --use-current --squash-hierarchy delinquants.server --dest-dir built-docs
pip install -r built-docs/requirements.txt
```

* Build the documentation:

```sh
ANSIBLE_COLLECTIONS_PATH="$(pwd)" bash -c 'cd built-docs/ && ./build.sh'
```

* View the documentation in a browser:

```sh
xdg-open built-docs/build/html/index.html
```
