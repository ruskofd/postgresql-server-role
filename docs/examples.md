Examples
--------

#### Manage PostgreSQL host-based access (pg_hba)

```YAML
postgresql_hba_entries:
- { type: host,  db: 'mydb',        user: 'myuser',      address: '0.0.0.0/0',        method: 'scram-sha-256' }
- { type: host,  db: 'mydb1,mydb2', user: 'mike',        address: '192.168.122.0/24', method: 'md5' }
- { type: local, db: 'telegraf',    user: 'telegraf',    method: 'peer' }
- { type: host,  db: 'replication', user: 'replication', address: '192.168.122.10/32' method: 'scram-sha-256' }
```

More informations in the official "pg_hba" [documentation](https://www.postgresql.org/docs/current/auth-pg-hba-conf.html).

#### Manage PostgreSQL user name maps (pg_ident)

When using an external authentication system such as Ident or GSSAPI, the name of the operating system user that initiated the connection might not be the same as the database user (role) that is to be used. In this case, a user name map can be applied to map the operating system user name to a database user. 

```YAML
postgresql_ident_entries:
- { mapname: 'omicron', system_username: 'bryanh', pg_username: 'bryanh' }
- { mapname: 'omicron', system_username: 'bryanh', pg_username: 'guest1' }
- { mapname: 'mymap', system_username: '/^(.*)@mydomain\.com$' pg_username: '\1' }
- { mapname: 'mymap', system_username: '/^(.*)@otherdomain\.com$', pg_username: 'guest' }
```

More informations in the official User Name Maps [documentation](https://www.postgresql.org/docs/current/auth-username-maps.html)

#### Manage PostgreSQL synchronous streaming replication

By default, PostgreSQL streaming is asynchronous, but you can configure it to be synchronous if needed :

```YAML
# You need to define a unique name for each replica node
postgresql_cluster_name: "server1"

# Enable synchronous replication (there are multiples options such as 'on', 'remote_write' and 'remote_apply')
postgresql_synchronous_commit: 'on'

# Define the nodes that should be replicated synchronously
postgresql_synchronous_standby_names: "server1, server2, server3"
```

You can also define different topologies for synchronous replication :

```YAML
# This will cause each commit to wait for replies from three higher-priority standbys chosen 
# from standby servers server1, server2, server3 and server4. The standbys whose names appear 
# earlier in the list are given higher priority and will be considered as synchronous.
postgresql_synchronous_standby_names: 'FIRST 3 (server1, server2, server3, server4)'

# This will cause synchronous commit to wait for reply from any 2 standby servers
postgresql_synchronous_standby_names: 'ANY 2 (*)'
```

More informations in the official replication [documentation](https://www.postgresql.org/docs/current/runtime-config-replication.html)

#### Switch back to configurations located in data directory

Since version 2.0.0 of this role, PostgreSQL configuration files are now located in `/etc/postgresql/<postgresql release>` directory by default. This change helps to manage configurations outside of the data directory, especially for streaming replication setup (there is no need to save/restore configurations before/after the initial basebackup anymore).

If you want to use the default PostgreSQL behavior by having the configurations inside the data directory, simply tweak this variable :

```YAML
postgresql_config_dir: "{{ postgresql_data_dir }}"
```

[Return to main page](../README.md)
