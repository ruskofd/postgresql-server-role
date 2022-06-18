<p><img src="https://icon-library.com/images/postgresql-icon/postgresql-icon-20.jpg" alt="pg-logo" title="pg" align="top" height=220 /></p>

***PostgreSQL** is a powerful, open source object-relational database system that uses and extends the SQL language combined with many features that safely store and scale the most complicated data workloads. PostgreSQL has earned a strong reputation for its proven architecture, reliability, data integrity, robust feature set, extensibility, and the dedication of the open source community behind the software to consistently deliver performant and innovative solutions.*

### General informations

This Ansible role is designed to deploy and configure PostgreSQL standalone database instances on target servers.

**Table of Contents**

  - [Roles variables](#role-variables)
  - [Roles tags](#role-variables)
  - [Install and use this role](#install-and-use-this-role)

**Supported Platforms**

  - Red Hat Enterprise Linux 8.x/9.x
  - Fedora 36

**Supported PostgreSQL releases**

  - PostgreSQL 14.x (default)

**References**

  - PostgreSQL : https://www.postgresql.org/

### Role variables

The role variables are documented [HERE](docs/variables.md)

### Examples

You can find some configurations examples [HERE](docs/examples.md)

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
      version: '2.0.0'

  $ ansible-galaxy install-f -r requirements.yml
  ```

* Once the collection is installed, you can use the roles in your playbooks :

  ```yaml
  - name: Install PostgreSQL server
    hosts: postgresql
    roles:
      - role: postgresql-server
  ```
