Role Variables
--------------

#### General

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_version`              | `14.2`                       | Define the package version of PostgreSQL to install              |
| `postgresql_role`                 | `primary`                    | Define the role of the instance (`primary` or `standby`). This adjust few settings to prepare instance for replication depending on the given role. Set this to `primary` for standalone instance(s). |
| `postgresql_cluster_name`         | `null`                       | Sets a name that identifies this database cluster (instance) for various purposes. The cluster name appears in the process title for all server processes in this cluster. Moreover, it is the default application name for a standby connection |
| `postgresql_checksum_enabled`     | `false`                      | If set to true, enable checksuming on data pages to help detect corruption by the I/O system that would otherwise be silent. Enabling checksums may incur a noticeable performance penalty. |
| `postgresql_initdb_extra_opts`    | `""`                         | Extra args to pass to the `initdb` command                       |

#### Service

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_service_env_file`     | see [vars](../vars/)         | Defines where the PostgreSQL environment vars file should be stored (based on OS) |
| `postgresql_service_kill_signal`  | `SIGINT`                     | Defines the POSIX signal to send to the PostgreSQL postmaster process when stopping service |
| `postgresql_service_extra_opts`   | `{}`                         | Defines the extra arguments to pass to the PostgreSQL postmaster process when starting the instance |

#### File locations

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_root_dir`             | `/var/lib/postgresql`        | Directory where everything related to the PostgreSQL instance is stored |
| `postgresql_data_dir`             | `{{ postgresql_root_dir }}/{{ postgresql_release }}/data` | Directory where PostgreSQL will store database data |
| `postgresql_home_dir`             | `{{ postgresql_root_dir }}`  | Home directory for the `postgres` user                           |
| `postgresql_config_dir`           | `{{ postgresql_data_dir }}`  | Defines the directory where to store the different PostgreSQL configuration files |

#### Connections and authentications

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_listen_addresses`     | `localhost`                  | Specifies the TCP/IP address(es) on which the server is to listen for connections from client applications. The value takes the form of a comma-separated list of host names and/or numeric IP addresses. |
| `postgresql_port`                 | `5432`                       | The TCP port the server listens on                               |
| `postgresql_max_connections`      | `100`                        | Determines the maximum number of concurrent connections to the database server |
| `postgresql_superuser_reserved_connections` | `3`                | Determines the number of connection “slots” that are reserved for connections by PostgreSQL superusers |
| `postgresql_authentication_timeout` | `1min`                     | Maximum amount of time allowed to complete client authentication |
| `postgresql_password_encryption`  | `scram-sha-256`              | When a password is specified in `CREATE ROLE` or `ALTER ROLE`, this parameter determines the algorithm to use to encrypt the password |

#### Resource usage (except WAL)

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_shared_buffers`       | `128MB`                      | Sets the amount of memory the database server uses for shared memory buffers |
| `postgresql_temp_buffers`         | `8MB`                        | Sets the maximum amount of memory used for temporary buffers within each database session |
| `postgresql_work_mem`             | `4MB`                        | Sets the base maximum amount of memory to be used by a query operation (such as a sort or hash table) before writing to temporary disk files |
| `postgresql_hash_mem_multiplier`  | `1.0`                        | Used to compute the maximum amount of memory that hash-based operations can use |
| `postgresql_maintenance_work_mem` | `64MB`                       | Specifies the maximum amount of memory to be used by maintenance operations, such as `VACUUM`, `CREATE INDEX` and `ALTER TABLE ADD FOREIGN KEY` |
| `postgresql_max_prepared_transactions` | `0`                     | Sets the maximum number of transactions that can be in the `prepared` state simultaneously. Setting this parameter to zero (which is the default) disables the prepared-transaction feature. This parameter can only be set at server start. If you are not planning to use prepared transactions, this parameter should be set to zero to prevent accidental creation of prepared transactions. |

#### Write-Ahead Log

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_wal_level`            | `replica`                    | Determines how much information is written to the WAL            |
| `postgresql_wal_sync_method`      | `fdatasync`                  | Method used for forcing WAL updates out to disk (see documentation for possible values) |
| `postgresql_wal_compression`      | `false`                      | When this parameter is set to `true`, the PostgreSQL server compresses full page images written to WAL when `full_page_writes` (the default) is on or during a base backup |
| `postgresql_wal_buffers`          | `-1`                         | The amount of shared memory used for WAL data that has not yet been written to disk. The default setting of `-1` selects a size equal to 1/32nd (about 3%) of `shared_buffers`, but not less than `64kB` nor more than the size of one WAL segment, typically `16MB` |
| `postgresql_synchronous_commit`   | `on`                         | Specifies how much WAL processing must complete before the database server returns a "success" indication to the client. Valid values are `remote_apply`, `on` (the default), `remote_write`, `local`, and `off`.|
| `postgresql_checkpoint_timeout`   | `5min`                       | Maximum time between automatic WAL checkpoints                   |
| `postgresql_archive_mode_enabled` | `false`                      | When `archive_mode` is enabled, completed WAL segments are sent to archive storage by setting `archive_command` |
| `postgresql_archive_command`      | `null`                       | The local shell command to execute to archive a completed WAL file segment |
| `postgresql_restore_command`      | `null`                       | The local shell command to execute to retrieve an archived segment of the WAL file series. This parameter is required for archive recovery, but optional for streaming replication |

#### Replication

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_max_wal_senders`      | `10`                         | Specifies the maximum number of concurrent connections from standby servers or streaming base backup clients (`primary` node only) |
| `postgresql_max_replication_slots`| `10`                         | Specifies the maximum number of replication slots that the server can support (`primary` node only) |
| `postgresql_synchronous_standby_names`| `[]`                     | Specifies a list of standby servers that can support synchronous replication |
| `postgresql_hot_standby`          | `true`                       | Specifies whether or not you can connect and run queries during recovery (`standby` node(s) only ) |

#### Reporting and logging

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_log_destination`      | `stderr`                     | PostgreSQL logging method (`stderr`, `csvlog` or `syslog`)      |
| `postgresql_logging_collector_enabled` | `false`                 | This parameter enables the logging collector, which is a background process that captures log messages sent to stderr and redirects them into log files |
| `postgresql_log_directory`        | `log`                        | When `logging_collector` is enabled, this parameter determines the directory in which log files will be created |
| `postgresql_log_filename`         | `postgresql-%Y-%m-%d_%H%M%S.log` | When `logging_collector` is enabled, this parameter sets the file names of the created log files |
| `postgresql_log_rotation_age`     | `1d`                         | When `logging_collector` is enabled, this parameter determines the maximum amount of time to use an individual log file, after which a new log file will be created |
| `postgresql_log_min_messages`     | `warning`                    | Controls which message levels are written to the server log (`DEBUG5`, `DEBUG4`, `DEBUG3`, `DEBUG2`, `DEBUG1`, `INFO`, `NOTICE`, `WARNING`, `ERROR`, `LOG`, `FATAL` and `PANIC`) |
| `postgresql_log_min_error_statement` | `error`                   | Controls which SQL statements that cause an error condition are recorded in the server log (`DEBUG5`, `DEBUG4`, `DEBUG3`, `DEBUG2`, `DEBUG1`, `INFO`, `NOTICE`, `WARNING`, `ERROR`, `LOG`, `FATAL` and `PANIC`) |
| `postgresql_log_min_duration_statement` | `-1`                   | Causes the duration of each completed statement to be logged if the statement ran for at least the specified amount of time |
| `postgresql_log_timezone`         | `Etc/UTC`                    | Sets the time zone used for timestamps written in the server log |
| `postgresql_syslog_facility`      | `LOCAL0`                     | If `postgresql_log_destination` is set to `syslog`, this parameter determines the syslog facility to be used |
| `postgresql_syslog_ident`         | `postgres`                   | When logging to syslog is enabled, this parameter determines the program name used to identify PostgreSQL messages in syslog logs |
| `postgresql_syslog_sequence_numbers` | `true`                    | If set to `true`, each message will be prefixed by an increasing sequence number |
| `postgresql_syslog_split_messages` | `true`                      | If set to `true`, this parameter determines how messages are delivered to syslog. When `true`, messages are split by lines, and long lines are split so that they will fit into 1024 bytes. When `false`, PostgreSQL server log messages are delivered to the syslog service as is, and it is up to the syslog service to cope with the potentially bulky messages |

#### Statistics

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_track_activities`     | `true`                       | Enables the collection of information on the currently executing command of each session, along with its identifier and the time when that command began execution |
| `postgresql_track_activity_query_size`| `1024`                   | Specifies the amount of memory reserved to store the text of the currently executing command for each active session, for the `pg_stat_activity.query` field |
| `postgresql_track_counts`         | `true`                       | Enables collection of statistics on database activity (required by autovacuum) |
| `postgresql_track_io_timing`      | `false`                      | Enables timing of database I/O calls. This parameter is off by default, as it will repeatedly query the operating system for the current time, which may cause significant overhead on some platforms |
| `postgresql_track_wal_io_timing`  | `false`                      | Enables timing of WAL I/O calls. This parameter is off by default, as it will repeatedly query the operating system for the current time, which may cause significant overhead on some platforms |
| `postgresql_track_functions`      | `none`                       | Enables tracking of function call counts and time used. Specify `pl` to track only procedural-language functions, `all` to also track SQL and C language functions. The default is `none`, which disables function statistics tracking |
| `postgresql_stats_temp_directory` | `pg_stat_tmp`                | Sets the directory to store temporary statistics data in         |

#### Autovacuum

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_autovacuum_enabled`   | `true`                       | Controls whether the server should run the autovacuum launcher daemon |
| `postgresql_autovacuum_max_workers` | `3`                        | Specifies the maximum number of autovacuum processes (other than the autovacuum launcher) that may be running at any one time |
| `postgresql_autovacuum_naptime`   | `1min`                       | Specifies the minimum delay between autovacuum runs on any given database |

#### Client connection defaults

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_timezone`             | `Etc/UTC`                    | Sets the time zone for displaying and interpreting time stamps   |
| `postgresql_client_encoding`      | `UTF8`                       | Sets the client-side encoding (character set)                    |
| `postgresql_locale`               | `C.UTF-8`                    | Sets the locale                                                  |
| `postgresql_shared_preload_libraries` | `[]`                     | Specifies one or more shared libraries to be preloaded at server start |

#### Lock management

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_deadlock_timeout`     | `1s`                         | This is the amount of time to wait on a lock before checking to see if there is a deadlock condition |
 
#### Error handling

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_restart_after_crash`  | `true`                       | When set to on, which is the default, PostgreSQL will automatically reinitialize after a backend crash |

#### pg_hba

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_hba_entries`          | `[]`                         | Entries to set inside the `pg_hba.conf` file to manage Host-Based Access |

#### pg_ident

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_ident_entries`        | `[]`                         | Entries to set inside the `pg_ident.conf` file to manage User Name maps |

[Return to main page](../README.md)
