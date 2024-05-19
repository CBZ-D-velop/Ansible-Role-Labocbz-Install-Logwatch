# Ansible role: labocbz.install_logwatch

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Logwtach](https://img.shields.io/badge/Tech-Logwtach-orange)

An Ansible role to install and configure Logwtach on your host.

The Ansible role installs and configures Logwatch on a target system. It provides customizable settings to set up a cron job for sending daily reports or disable it if desired, all of which can be configured through variables.

The role allows customization of Logwatch's behavior, including the option to create a cron job for daily reports. Administrators can define the report's format, the email address to receive reports, the sender's name, and the time range and detail level for the reports.

This flexibility empowers administrators to tailor Logwatch's functionality according to their specific needs, ensuring effective log analysis and monitoring for the system.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_logwatch__create_cron_job: true
install_logwatch__cron_job_weekday: "*"
install_logwatch__cron_job_minute: "6"
install_logwatch__cron_job_hour: "6"

install_logwatch__output_format: "html"
install_logwatch__report_email_address: "your.address@domain.tld"
install_logwatch__email_from: "Logwatch"
install_logwatch__range: "Yesterday"
install_logwatch__detail: "Med"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_logwatch__create_cron_job: true
inv_install_logwatch__cron_job_weekday: "*"
inv_install_logwatch__cron_job_minute: "6"
inv_install_logwatch__cron_job_hour: "6"

inv_install_logwatch__report_email_address: "your.address@domain.tld"
inv_install_logwatch__output_format: "html"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_logwatch"
    tags:
    - "labocbz.install_logwatch"
    vars:
    install_logwatch__create_cron_job: "{{ inv_install_logwatch__create_cron_job }}"
    install_logwatch__cron_job_weekday: "{{ inv_install_logwatch__cron_job_weekday }}"
    install_logwatch__cron_job_minute: "{{ inv_install_logwatch__cron_job_minute }}"
    install_logwatch__cron_job_hour: "{{ inv_install_logwatch__cron_job_hour }}"
    install_logwatch__report_email_address: "{{ inv_install_logwatch__report_email_address }}"
    install_logwatch__output_format: "{{ inv_install_logwatch__output_format }}"
    ansible.builtin.include_role:
    name: "labocbz.install_logwatch"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2024-02-23: New CICD and fixes

* Added support for Ubuntu 22
* Added support for Debian 11/22
* Edited vars for linting (role name and __)
* Added generic support for Docker dind (can add used for obscures reasons ... user in use)
* Fix idempotency
* Added informations for UID and GID for user/groups
* Added support for user password creation (on_create)
* New CI, need work on tag and releases
* CI use now Sonarqube

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
