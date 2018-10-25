## This is a fork of [GuildEducationInc/docker-amazon-redshift](https://github.com/GuildEducationInc/docker-amazon-redshift)

The fork was done with only one purpose:

To use the **PostgreSQL v.8.4.0** instead of **v.8.0.2**

The change was made due to a reason that the PostgreSQL v.8.0.2 doesn't support the **[Window Functions](https://www.postgresql.org/docs/8.4/static/functions-window.html)**. Amazon Redshift, on the other hand, supports the Window Functions, as the PostgreSQL v.8.4.0 does.

So, the Docker image was updated to use the PostgreSQL v.8.4.0 to provide an ability to test the usage of the Window Functions and see how they will work on Amazon Redshift.

# Docker Amazon Redshift

This is an unofficial implementation of Amazon Redshift inside a Docker container. At this time, the container is merely PostgreSQL 8.4.0 running  on port 5439 inside the container. No other changes have been made to make it more closely match the behavior of Redshift.

There are two variants: one based on Debian Jessie and another based on Alpine 3.5. The Dockerfiles and entrypoint scripts are closely based on those used in the [official Postgres images](https://hub.docker.com/_/postgres/).

## How to use the image

### Running
`docker run -d --name my-redshift nazarhopchak/docker-amazon-redshift-postgres8.4.0`

Port 5439 is set via an `EXPOSE` command so it should be available to linked containers. Optionally, you can map it to the host:

`docker run -d -p 5439:5439 --name my-redshift nazarhopchak/docker-amazon-redshift-postgres8.4.0`

### Persisting Data

In order to have data persist between container runs, map the `PGDATA` directory to a volume. This variable is defaulted to `/var/lib/postgresql/data`:

`docker run -d -p 5439:5439 -v /path/on/host:/var/lib/postgresql/data --name my-redshift nazarhopchak/docker-amazon-redshift-postgres8.4.0`

`initdb` is run on the `PGDATA` directory automatically on container start

### Setting a password

It is recommended to set a password for the default postgres user:

`docker run -d -p 5439:5439 -v /path/on/host:/var/lib/postgresql/data -e POSTGRES_PASSWORD=your_password --name my-redshift nazarhopchak/docker-amazon-redshift-postgres8.4.0`

## Environment Variables

* `PGPORT` - port Postgres will run on. Default is `5439`.
* `PGDATA` - Data directory for Postgres. Default is `/var/lib/postgresql/data`.
* `POSTGRES_PASSWORD` - password for the database user. Default is no password.
* `POSTGRES_USER` - Sets a different user to run the database under. Default is `postgres`.
* `POSTGRES_DB` - Sets the name of the db that is created during the `initdb` run. Note the default `postgres` db will be created anyway.
* `POSTGRES_INITDB_ARGS` - Sets additional `initdb` args.
* `POSTGRES_INITDB_XLOGDIR` - Defines a different location for the Postgres transaction log
