## wordpress pipeline reviso

CI: Jenkins

url: http://35.158.236.53/


# docker-wordpress

This repository is an example configuration of [Wordpress](http://wordpress.org/), running on multiple
[Docker](http://www.docker.com/) containers. [Docker Compose](http://docs.docker.com/compose/)
is used for orchestration.

## Multiple containers

This environment uses 2 containers:

1. `web` container with Apache2 and PHP, being build from `Dockerfile`. This container shares application sources with
host machine.
2. `db` container with MySQL, pulled directly from [tutum/mysql](https://registry.hub.docker.com/u/tutum/mysql/) image
on Docker Hub. 

Containers are managed by Docker Compose. The configuration is in `docker-compose.yml`.

## Requirements

Basically, you need to have [Docker Compose installed](http://docs.docker.com/compose/#installation-and-set-up).

If you are using Mac OS X (Tested on 10.13) or Windows as your host OS, I recommend using [Docker Toolbox](https://www.docker.com/products/docker-toolbox)
as proxy VM to run Docker.


### Mac OS X
If Mac OS X is your host OS and you use toolbox to launch Docker you will probably encounter [the bug with writing
to shared volume]. The following workaround works perfectly:

### Once Docker Toolbox is Installed
0. Go to settings > shared folders > click + > choose the folder path to [`docker-composer-wordpress`]

1. Edit the `docker-composer-wordpress/docker-compose.yml` file: `sudo vim docker-compose.yml`

3. Add a # before the following lines (this will remove the local volume reference):
    ```
		#volumes:
    #- /mnt/sda1/var/lib/wordpress:/var/lib/mysql
    ```

4. Exit and restart it: `docker-compose build`

## How to use it?

### Start the environment

In the project root directory run the following command:

```
docker-compose up -d
```

This command will build `web` Docker image and run its container together with `db` container.

### Install dependencies

1. Log in to the container by running the following command:
    ```
    docker exec -i -t dockerwordpress_web_1 bash
    ```

2. Install dependencies by running the following command:
    ```
    composer install -n
    ```

### Create database for Wordpress

In project root folder on your host machine run the following command:
```
docker exec -it dockerwordpress_db_1 mysql -u root -e "CREATE DATABASE wordpress;"
```

### Update your hosts

#### Mac OS X

1. Check boot2docker IP address: `boot2docker ip`.

2. Assuming its 192.168.59.103, add the following line to your `/etc/hosts` file:
    ```
    192.168.59.103 wordpress.dev
    ```

#### Linux

TBA

#### Windows

1. Check boot2docker IP address: `boot2docker ip`.

2. Assuming its 192.168.59.103, add the following line to your `%SystemRoot%\System32\drivers\etc\hosts` file:
    ```
    192.168.59.103 wordpress.dev
    ```
