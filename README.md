# Laradock for TMS

Laradock is a Docker PHP development environment. It facilitate running **PHP** Apps on **Docker**.

Laradock is configured to run Laravel Apps by default, and it can be modifyed to run all kinds of PHP Apps (Symfony, Codeigniter, Wordpress, Drupal...).

>Use Docker first and learn about it later.

## Contents

- [Readme Languages](#)
	- [English (Default)](#)
- [Intro](#Intro)
	- [Features](#features)
	- [Supported Software](#Supported-Containers)
	- [What is Docker](#what-is-docker)
	- [Why Docker not Vagrant](#why-docker-not-vagrant)
- [Requirements](#Requirements)
- [Installation](#Installation)
- [Usage](#Usage)
    - [Docker Shell Command](#DockerShellCommand)
- [Documentation](#Documentation)
	- [Docker](#Docker)
		- [List current running Containers](#List-current-running-Containers)
		- [Close all running Containers](#Close-all-running-Containers)
		- [Delete all existing Containers](#Delete-all-existing-Containers)
		- [Enter a Container (run commands in a running Container)](#Enter-Container)
		- [Build/Re-build Containers](#Build-Re-build-Containers)
		- [View the Log files](#View-the-Log-files)
	- [PHP](#PHP)
		- [Install xDebug](#Install-xDebug)
		    - [Start/Stop xDebug](#Controll-xDebug)
	- [Laravel](#Laravel):
		- [Run Artisan Commands](#Run-Artisan-Commands)
	- [Misc](#Misc)
		- [Change the timezone](#Change-the-timezone)
		- [Cron jobs](#CronJobs)
		- [Access workspace via ssh](#Workspace-ssh)
		- [MySQL access from host](#MySQL-access-from-host)
		- [Use custom Domain](#Use-custom-Domain)
- [Help & Questions](#Help)



<a name="Intro"></a>
## Intro

Laradock strives to make the PHP development experience easier and faster.

It contains pre-packaged Docker Images that provides you a wonderful *development* environment without requiring you to install PHP, NGINX, MySQL, REDIS, and any other software on your machines.


**Usage Overview:**

Let's see how easy it is to install ***`TMS`*** with `PHP`, `Composer`, `MySQL` and `Apache`.

1. Get LaraDock inside your Laravel project:
<br>
`git clone https://github.com/studysocial/laradock.git`.
Insert the following variables in the `docker-compose` `.env` file.
2. Create file named `.env` inside the `laradock` folder. This file is your `docker-compose` `.env` file and is different from your `laravel` `.env` file. The latter can be edited inside the workspace folder. See below.
Insert the following variables in the `docker-compose` `.env` file:
```
MYSQL_DATABASE=newtms
MYSQL_USER=root
MYSQL_PASSWORD=123MySQL101.
MYSQL_ROOT_PASSWORD=123MySQL101.
```
Bear in mind, whatever you use in the above variables will need to be reflected in the corresponding variables in your `laravel` `.env` file.
3. Enter the laradock folder and run only these Containers:
<br>
`docker-compose -p tms up -d php-fpm mysql apache2`
3. Open your `.env` file and set `DB_HOST` to `mysql`. The other variables - `MYSQL_DATABASE`, `MYSQL_USER` and `MYSQL_PASSWORD` will need to be the same as in  your `docker-compose` `.env` file. This will be addressed to happening automatically in the future.
4. Open your browser and visit the localhost: `http://newtms.dev` if you have your `dnsmasq` (or any other DNS daemon that you may be using, including `hosts` file) set up. Otherwise go to `http://localhost`.
>This guide assumes this is run on a clean system. Any other daemons that may be interfering with Apache/MySQL/PHP-FPM need to be stopped. Ports that need to be available are 80/443/3306 and 9000 (for XDebug).



<a name="features"></a>
### Features

- Every software runs on a separate container: PHP-FPM, APACHE, PHP-CLI...
- Easy to customize any container, with simple edit to the `dockerfile`.
- All Images extends from an official base Image. (Trusted base Images).
- Easy to apply configurations inside containers.
- Clean and well structured Dockerfiles (`dockerfile`).
- Latest version of the Docker Compose file (`docker-compose`).
- Everything is visible and editable.
- Fast Images Builds.


<a name="Supported-Containers"></a>
### Supported Software (Containers)

- **Database Engines:**
	- MySQL - ***verified***
	- PostgreSQL
	- MariaDB
	- MongoDB
	- Neo4j
- **Cache Engines:**
	- Redis - ***verified as part of the logging platform ELK***
	- Memcached
	- Aerospike
- **PHP Servers:**
	- NGINX
	- Apache2 - ***verified***
	- Caddy
- **PHP Compilers:**
	- PHP-FPM - ***verified***
	- HHVM
- **Tools:**
	- Workspace (PHP56-CLI, Composer, Git, Node, Gulp, xDebug...) - ***verified***
	- PhpMyAdmin
	- PgAdmin
	- ElasticSearch - ***verified***




<a name="what-is-docker"></a>
### What is Docker?

[Docker](https://www.docker.com) is an open-source project that automates the deployment of applications inside software containers, by providing an additional layer of abstraction and automation of [operating-system-level virtualization](https://en.wikipedia.org/wiki/Operating-system-level_virtualization) on Linux, Mac OS and Windows.


<a name="why-docker-not-vagrant"></a>
### Why Docker not Vagrant!?

[Vagrant](https://www.vagrantup.com) creates Virtual Machines in minutes while Docker creates Virtual Containers in seconds.

Instead of providing a full Virtual Machines, like you get with Vagrant, Docker provides you **lightweight** Virtual Containers, that share the same kernel and allow to safely execute independent processes.

In addition to the speed, Docker gives tons of features that cannot be achieved with Vagrant.

Most importantly Docker can run on Development and on Production (same environment everywhere). While Vagrant is designed for Development only, (so you have to re-provision your server on Production every time).



<a name="Requirements"></a>
## Requirements

- [Git](https://git-scm.com/downloads)
- [Docker](https://www.docker.com/products/docker/) `>= 1.12`



<a name="Installation"></a>
## Installation


Clone the `LaraDock` repository:

**A)** If you already have a Laravel project, clone this repository on your `Laravel` root directory:

```bash
git submodule add https://github.com/studysocial/laradock.git
```
>If you are not already using Git for your Laravel project (which you absolutely must!), you can use `git clone` instead of `git submodule`.

<br>

<a name="Usage"></a>
## Usage


**Read Before starting:**

If you are using **Docker Toolbox** (VM), do one of the following:

- Upgrade to Docker [Native](https://www.docker.com/products/docker) for Mac/Windows (Recommended). Check out [Upgrading Laradock](#upgrading-laradock)

<br>

1 - Run Containers: *(Make sure you are in the `laradock` folder before running the `docker-compose` commands).*



**Example:** Running TMS (Apache + MySQL):

```bash
docker-compose up -d apache2 mysql php-fpm
```

**Note**: First time run should take care of the DB server start and DB dump restore. For some reason, the MySQL container stops the first time it is run - when it creates the database. Therefore, it needs to be run twice, so as a prerequisite, please run:
```bash
docker-compose up -d mysql
```
Wait for the container to start and check if it is alive:
```bash
docker ps -a
```

You should see something like
```bash
d0a0489d993f        tms_mysql           "docker-entrypoint.sh"   57 minutes ago      Exited (0) 3 seconds ago                                                  tms_mysql_1
```
Note the `Exited` state. If you see instead
```bash
d0a0489d993f        tms_mysql           "docker-entrypoint.sh"   59 minutes ago      Up 3 seconds                   0.0.0.0:3306->3306/tcp                     tms_mysql_1
```
then the container is running.

If not, start it again with
```bash
docker-compose up -d mysql
```
The DB server should be running and you can restore the database.

After the DB is restored successfully, run this (again)

```bash
docker-compose up -d apache2 mysql php-fpm
```

**Note**: The `workspace` and `php-fpm` should run automatically in most of the cases, so no need to specify them in the `up` command. If you couldn't find them running then you need specify them as follow: `docker-compose -p tms up -d apache php-fpm mysql workspace`.


Theoretically, you can select your own combination of Containers form the list below:

`nginx`, `hhvm`, `php-fpm`, `mysql`, `redis`, `postgres`, `mariadb`, `neo4j`, `mongo`, `apache2`, `caddy`, `memcached`, `beanstalkd`, `beanstalkd-console`, `rabbitmq`, `workspace`, `phpmyadmin`, `aerospike`, `pgadmin`, `elasticsearch`.

However, for TMS we only need the exact combination installed in production:

`apache2`, `php-fpm`, `mysql`.

<br>
2 - Enter the Workspace container, to execute commands like (Artisan, Composer, PHPUnit, Gulp, ...).

```bash
docker-compose exec workspace bash
```
Alternatively, for Windows Powershell users: execute the following command to enter any running container:

```bash
docker exec -it {workspace-container-id} bash
```

**Note:** You can add `--user=laradock` (example `docker-compose exec --user=laradock workspace bash`) to have files created as your host's user. (you can change the PUID (User id) and PGID (group id) variables from the `docker-compose.yml`).



<br>
3 - Edit your project configurations.

Open your `.env` file and set the `DB_HOST` to `mysql`:

```env
DB_HOST=mysql
```



<br>
4 - Open your browser and visit your localhost address (`http://localhost/`).



<br>
**Debugging**: if you are facing any problem here check the [Debugging](#debugging) section.


<a name="DockerShellCommand"></a>
### Docker Shell Command

In Mac/Linux, you can use the docker shell command tool.

Command line:
```bash
./docker <options>
```
- You can use the same options in a normal docker-compose command.
- On the first run, you have to wait until the gulp image stops, then you can run the project.
- By default, the shell command launch the servises enabled in the `.env` file. So if you want to enable services, you should set the value to 1/true, otherwise is disabled.

For developers, you need to:
- Replace the content of `laradock/workspace/dotenv` with `laradock/workspace/dotenv.dev`.
- For email testing, you need a mailtrap account and copy the configuration in the `dotenv` file.

After configure all, if you want to run tms, just need to run:
```bash
./docker up -d
```
And to stop it, run:
```bash
./docker down
```

If you make changes in the `package.json`, you need to pre-compile gulp:
```bash
./docker gulp-pre
```
And when you make changes in any frontend, you can restart the docker gulp image, or run the gulp command:
```bash
#compile all (default, the same if you restart the gulp image)
./docker gulp

#or if you don't want to recompile everithing, you can:

#compile only the client
./docker gulp client
#compile only the super panel
./docker gulp super
#compile only the school panel
./docker gulp school
```

<br>
<a name="Documentation"></a>
## Documentation


<a name="Docker"></a>




<a name="List-current-running-Containers"></a>
### List current running Containers
```bash
docker ps
```
You can also use the this command if you want to see only this project containers:

```bash
docker-compose -p tms ps
```





<br>
<a name="Close-all-running-Containers"></a>
### Close all running Containers
```bash
docker-compose -p tms stop
```

To stop single container do:

```bash
docker-compose -p tms stop {container-name}
```






<br>
<a name="Delete-all-existing-Containers"></a>
### Delete all existing Containers
```bash
docker-compose -p tms down
```

*Note: Careful with this command as it will delete your Data Volume Container as well. (if you want to keep your Database data than you should stop each container by itself as follow):*







<br>
<a name="Enter-Container"></a>
### Enter a Container (run commands in a running Container)

1 - first list the current running containers with `docker ps`

2 - enter any container using:

```bash
docker-compose -p tms exec {container-name} bash
```

*Example: enter MySQL container*

```bash
docker-compose -p tms exec mysql bash
```

3 - to exit a container, type `exit`.







<br>

3 - Re-build the container:

```bash
docker-compose -p tms build mysql
```
More info on Containers rebuilding [here](#Build-Re-build-Containers).








<br>
<a name="Build-Re-build-Containers"></a>
### Build/Re-build Containers

If you do any change to any `dockerfile` make sure you run this command, for the changes to take effect:

```bash
docker-compose -p tms build
```
Optionally you can specify which container to rebuild (instead of rebuilding all the containers):

```bash
docker-compose -p tms build {container-name}
```

You might use the `--no-cache` option if you want full rebuilding (`docker-compose -p tms build --no-cache {container-name}`).












<br>
<a name="View-the-Log-files"></a>
### View the Log files
The Nginx Log file is stored in the `logs/nginx` directory.

However to view the logs of all the other containers (MySQL, PHP-FPM,...) you can run this:

```bash
docker-compose -p tms logs {container-name}
```








<br>
<a name="PHP"></a>






<a name="Install-PHP-Extensions"></a>
### Install PHP Extensions

Before installing PHP extensions, you have to decide whether you need for the `FPM` or `CLI` because each lives on a different container, if you need it for both you have to edit both containers.

The PHP-FPM extensions should be installed in `php-fpm/Dockerfile-XX`. *(replace XX with your default PHP version number)*.
<br>
The PHP-CLI extensions should be installed in `workspace/Dockerfile`.



<br>
<a name="Misc"></a>





<br>

<a name="Change-the-timezone"></a>
### Change the timezone

To change the timezone for the `workspace` container, modify the `TZ` build argument in the Docker Compose file to one in the [TZ database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

For example, if I want the timezone to be `New York`:

```yml
    workspace:
        build:
            context: ./workspace
            args:
                - TZ=America/New_York
    ...
```

We also recommend [setting the timezone in Laravel](http://www.camroncade.com/managing-timezones-with-laravel/).

<a name="CronJobs"></a>
### Adding cron jobs

You can add your cron jobs to `workspace/crontab/root` after the `php artisan` line.

```
* * * * * php /var/www/tms/artisan schedule:run >> /dev/null 2>&1

# Custom cron
* * * * * root echo "Every Minute" > /var/log/cron.log 2>&1
```

Make sure you [change the timezone](#Change-the-timezone) if you don't want to use the default (UTC).

<a name="Workspace-ssh"></a>
### Access workspace via ssh

You can access the `workspace` container through `localhost:2222` by setting the `INSTALL_WORKSPACE_SSH` build argument to `true`.

To change the default forwarded port for ssh:

```yml
    workspace:
		ports:
			- "2222:22" # Edit this line
    ...
```

<a name="MySQL-access-from-host"></a>
### MySQL access from host

You can forward the MySQL/MariaDB port to your host by making sure these lines are added to the `mysql` or `mariadb` section of the `docker-compose.yml` or in your [environment specific Compose](https://docs.docker.com/compose/extends/) file.

```
ports:
    - "3306:3306"
```

<a name="Use-custom-Domain"></a>
### Use custom Domain (instead of the Docker IP)

Assuming your custom domain is `laravel.dev`

1 - Open your `/etc/hosts` file and map your localhost address `127.0.0.1` to the `laravel.dev` domain, by adding the following:

```bash
127.0.0.1    laravel.dev
```

2 - Open your browser and visit `{http://laravel.dev}`


Optionally you can define the server name in the nginx config file, like this:

```conf
server_name laravel.dev;
```


## Credits

**Creator:**

- [Mahmoud Zalt](https://github.com/Mahmoudz)  [ [Twitter](https://twitter.com/Mahmoud_Zalt) | [Personal Site](http://zalt.me) | [Linkedin](https://www.linkedin.com/in/mahmoudzalt) ]

**Admins:**

- [Bo-Yi Wu](https://github.com/appleboy) (appleboy)
- [Philippe Trépanier](https://github.com/philtrep) (philtrep)

**Main Contributors:**

- [Francis Lavoie](https://github.com/francislavoie) (francislavoie)
- [luciano-jr](https://github.com/luciano-jr)
- [Zhqagp](https://github.com/zhqagp)
- [Tim B.](https://github.com/tjb328) (tjb328)
- [MidasCodeBreaker](https://github.com/midascodebreaker)
- [Larry Eitel](https://github.com/LarryEitel)
- [Suteepat](https://github.com/tianissimo) (tianissimo)
- [David](https://github.com/davidavz) (davidavz)
- [Lialosiu](https://github.com/lialosiu)
- [Eric Pfeiffer](https://github.com/computerfr33k) (computerfr33k)
- [Orette](https://github.com/orette)
- [Jack Fletcher](https://github.com/Kauhat) (Kauhat)
- [Amin Mkh](https://github.com/AminMkh)
- [Matthew Tonkin Dunn](https://github.com/mattythebatty) (mattythebatty)
- [Zhivitsa Kirill](https://github.com/zhikiri) (zhikiri)
- [Benmag](https://github.com/benmag)

**Other Contributors & Supporters:**

- [Contributors](https://github.com/LaraDock/laradock/graphs/contributors)
- [Supporters](https://github.com/LaraDock/laradock/issues?utf8=%E2%9C%93&q=)

## License

[MIT License](https://github.com/laradock/laradock/blob/master/LICENSE) (MIT)
