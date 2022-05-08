<p><img src="https://icon-library.com/images/postgresql-icon/postgresql-icon-20.jpg" alt="pg-logo" title="pg" align="top" height=220 /></p>

### General informations

This Ansible role is designed to deploy and configure PostgreSQL standalone database instances on target servers.

**Table of Contents**

  - [Roles variables](#role-variables)
  - [Roles tags](#role-variables)
  - [Install and use this role](#install-and-use-this-role)

**Supported Platforms**

  - RHEL/CentOS Stream 8
  - RHEL/CentOS Stream 9
  - Fedora 35

**Supported PostgreSQL releases**

  - PostgreSQL 14.x (default)

**References**

  - PostgreSQL : https://www.postgresql.org/

### Role variables

The role variables are documented [HERE](docs/variables.md)

### Examples

You can find some configurations examples [HERE](docs/examples.md)

### Role tags

This role use some tag that you can use for some operations without replaying the entire role :

- `pg_install` : manage PostgreSQL repositories and install server packages
- `pg_initdb` : initialize the database
- `pg_config` : deliver the PostgreSQL configurations (`postgresql.conf` and `pg_hba.conf`)
- `pg_service` : deliver the PostgreSQL systemd unit and manage service state

### Install and use this role

* Install the role using the command-line :

  ```shell
  $ ansible-galaxy collection install git+https://github.com/ruskofd/postgresql-server-role.git
  ```

* Install the collection in your projects using a `requirements.yml` file and `ansible-galaxy` command-line :

  ```YAML
  $ cat requirements.yml
  ---
  roles:
    - name: postgresql-server
      src: https://github.com/ruskofd/postgresql-server-role.git
      scm: git
      version: '1.1.0'

  $ ansible-galaxy install-f -r requirements.yml
  ```

* Once the collection is installed, you can use the roles in your playbooks :

  ```yaml
  - name: Install PostgreSQL server
    hosts: postgresql
    roles:
      - role: postgresql-server
  ```
