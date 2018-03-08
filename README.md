# TimescaleDB Dockerized w/ Replication

Idea from https://github.com/DanielDent/docker-postgres-replication

Using this image
  * `git clone https://github.com/xvaara/timescaledb-replication.git`
  * `cd timescaledb-replication`
  * `docker build -t "timescale-rep" --no-cache .`
  * `docker run -it --name replicant-timescaledb -p 5432:5432 -e REPLICATE_FROM=rephost -e POSTGRES_USER=repuser -e POSTGRES_PASSWORD=reppassword timescale-rep:latest`

Master/Slave Postgres Replication in 30 seconds.

  * Quickstart: `docker-compose up`
  * For production, use docker-compose, Kubernetes, Rancher, Tutum, other PaaS tooling, ... or roll your own.
  * To see container environment variable requirements, see `docker-compose.yml`.
  * To demonstrate multiple slaves:
    * `docker-compose up`
    * `docker-compose scale pg-slave=3`

## Notes

   * No additional replication user is setup - the postgres admin user is used. This means the superuser credentials must be identical on the master and all slaves.
   * setup-replication.sh is only executed when a container's data volume is first initialized.
   * REPLICATE_FROM environment variable is only used during container initialization - if the master changes after the database has been initialized, you'll need to manually adjust the recovery.conf file in the slave containers' data volume.
   * Configuration:
     * PG_MAX_WAL_SENDERS 8 - Maximum number of slaves
     * PG_WAL_KEEP_SEGMENTS 32 - See http://www.postgresql.org/docs/9.6/static/runtime-config-replication.html
