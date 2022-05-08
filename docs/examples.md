Examples
--------

#### Manage PostgreSQL host-based access (pg_hba)

```YAML
postgresql_hba_entries:
- { type: host,  db: 'mydb',        user: 'myuser',      address: '0.0.0.0/0',        method: 'scram-sha-256' }
- { type: host,  ds: 'mydb1,mydb2', user: 'mike',        address: '192.168.122.0/24', method: 'md5' }
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


#### Change configuration files location

On Red-Hat based systems, the default location of the PostgreSQL configurations are located inside the data directory. If you want to change it to `/etc` i.e, you can tweak some of the role variables :

```YAML
# Defines the new directory (will be created if missing)
postgresql_config_dir: "/etc/postgresql/14"

# Add an extra argument to the postmaster
postgresql_service_extra_opts: [ "--config_file=/etc/postgresql/14/postgresql.conf" ]
```

All PostgreSQL configuration files will be pushed into the `/etc/postgresql/14` directory :

```SHELL
[root@pg ~]# ls -l /etc/postgresql/14/
total 16
-rw------- 1 postgres postgres  783 May  8 16:46 pg_hba.conf
-rw------- 1 postgres postgres  287 May  8 16:39 pg_ident.conf
-rw------- 1 postgres postgres 4185 May  8 16:32 postgresql.conf
```
