.. RAPID WEB APP documentation master file, created by
   sphinx-quickstart on Tue Sep 27 13:01:49 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to RAPID WEB APP's Setup!
=========================================
Welcome to the complete beginner's guide to RAPID WEB APP setup.

Prequisites
===========
Before installation you must have the following softwares installed in your machine

- For Local Development - Without Docker
    - Python 3.8 and above
    - PIP3
    - venv or pyenv (Virtual Environment)
    - git
    - PostgreSQL > 12
    
- For Test/Production Deployment - With Docker
    - Docker CE >= 20.10
    - Docker Compose >= 1.29
    - git

---------------------------------------------------------------


Setup Process in Local Without Docker
=====================================

Follow the below steps to setup rapid app in your local:

**Step 1**

- Use the following syntax in your terminal

.. code-block:: sh

	git clone https://github.com/devkitinn/dev-rapid (make sure you have all the access to this repository)
	git checkout feature/docker-setup

**Step 2**

- Create python virtualenvironment and activate it. For example, in linux operating system follow the below syntax

.. code-block:: sh

    python3 -m venv /path/to/venv    
    source /path/to/venv/bin/activate

**Step 3**

- Go to project root folder
- Install all the app packages from this folder (docker/packages/dev.txt). You can use the below syntax

.. code-block:: sh

    pip install -r docker/packages/dev.txt
    
**Step 4**

- Setup PostgreSQL, you can follow the below syntax in linux

.. code-block:: sh

	sudo -u postgres psql
	CREATE DATABASE webrapidtest;
	CREATE USER ki WITH PASSWORD 'rapid@23456';
	ALTER ROLE ki SET client_encoding TO 'utf8';
	ALTER ROLE ki SET default_transaction_isolation TO 'read committed';
	ALTER ROLE ki SET timezone TO 'UTC';
	GRANT ALL PRIVILEGES ON DATABASE webrapidtest TO ki;

**Step 5**
    
- Create environment variable tag ".env" with below data

.. admonition:: Remember!

	While copy pasting the below entire code in .env file make sure there is no indent at the begining, otherwise indendation error will be thrown while migrating


.. code-block:: sh

	SECRET_KEY=OxOqffUpTiMcKKCbhDHzXTQcYvfQxIjdUulIQjAF
	DEBUG=1
	DJANGO_ALLOWED_HOSTS="localhost 127.0.0.1 [::1]"

	SQL_ENGINE=django.db.backends.postgresql
	SQL_DATABASE=webrapidtest
	SQL_USER=ki
	SQL_PASSWORD=rapid@23456
	SQL_HOST=localhost
	SQL_PORT=5432

	DJANGO_SETTINGS_MODULE=rapid.settings
	DJANGO_EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
	DJANGO_EMAIL_HOST=smtp.gmail.com
	DJANGO_EMAIL_PORT=587
	DJANGO_EMAIL_HOST_USER=kitcheninnovations.rapid@gmail.com
	DJANGO_EMAIL_HOST_PASSWORD=fvoenkmakmjzzlag
	DJANGO_DEFAULT_FROM_EMAIL=rapid-email@kitcheninnovations.com.au
	DJANGO_EMAIL_USE_TLS=True

	DJANGO_ACCOUNT_ALLOW_REGISTRATION=False

	CONSUMER_KEY=a8fdd0ab8e5865c804fd3ca6bf1318c39db679ecc6902855c5850d163b62ba92
	CONSUMER_SECRET=7959943d2b26f64ecccd4722f21b5c2276df6ff80fea9a093d5839f2c94775d9
	TOKEN_KEY=bd612b0147cc8c8bee13af86d0f37e8e7df953c6b537068d266b91d8de59a5f0
	TOKEN_SECRET=e556cdce1bc7d78eff3d94e531a87f006946fb4d9dcd8add530421855e58e90d

	# Not in use

	NSDB_NAME=netsuite
	NSDB_USER=ki
	NSDB_PASSWORD=Link3061
	NSDB_HOST=172.16.1.46
	NSDB_PORT=5432
	PORT=5443
  
**Step 6**

- Enter the below syntax before migrations

.. code-block:: sh

	set -o allexport; source .env; set +o allexport 
        
**Step 7**

- Run migrations to initiate the process

.. code-block:: sh

    python3 manage.py migrate

**Step 8**

- Restore with backup data if there is any e.g, sample cmds

.. code-block:: sh

    cat backup.json | python3 manage.py loaddata --format=json -
    python3 manage.py loaddata backup.json or backup.json.gz

**Step 9**

- Create superuser and run server on any port

.. code-block:: sh

    python3 manage.py createsuperuser
    python3 manage.py runserver 8001

-----------------------------------------------------------------------------

Test Environment Setup with Docker
==================================

Follow the below steps to setup rapid app in your local,

**Step 1**

- Use the following syntax in your terminal

.. code-block:: sh

	git clone https://github.com/devkitinn/dev-rapid (make sure you have all the access to this repository)
	git checkout feature/docker-setup

**Step 2**

- Go to project root folder
- Add a host entry to /etc/hosts for your domain e.g 0.0.0.0 mydomain.com
- HTTPS support, Create self-signed ssl certificate or if you own any ssl certificates place them in your host dir - /ki-docker/nginx/certs/. it should have .crt and .key files.

**Follow the below example**

.. code-block:: sh

    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /ki-docker/nginx/certs/kitcheninnovations_com_au.key -out /ki-docker/nginx/certs/kitcheninnovations_com_au.crt

**Step 3**

- Create environment variable tag ".env" with below data

.. admonition:: Remember!

	While copy pasting the below entire code in .env file make sure there is no indent at the begining, otherwise indendation error will be thrown while migrating

.. code-block:: sh

	SECRET_KEY=OxOqffUpTiMcKKCbhDHzXTQcYvfQxIjdUulIQjAF        
	DEBUG=1        
	DJANGO_ALLOWED_HOSTS=<HOST-NAMES-SEPARATED-BY-SPACE>
	
	SQL_ENGINE=django.db.backends.postgresql        
	SQL_DATABASE=rapid-stage        
	SQL_USER=rapid-stage        
	SQL_PASSWORD=rapid@23456        
	SQL_HOST=db-test        
	SQL_PORT=5432

	DJANGO_SETTINGS_MODULE=rapid.settings        
	DJANGO_EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend        
	DJANGO_EMAIL_HOST=smtp.gmail.com
	DJANGO_EMAIL_PORT=587
	DJANGO_EMAIL_HOST_USER=kitcheninnovations.rapid@gmail.com
	DJANGO_EMAIL_HOST_PASSWORD=fvoenkmakmjzzlag
	DJANGO_DEFAULT_FROM_EMAIL=rapid-email@kitcheninnovations.com.au
	DJANGO_EMAIL_USE_TLS=True

	DJANGO_ACCOUNT_ALLOW_REGISTRATION=False

	CONSUMER_KEY=a8fdd0ab8e5865c804fd3ca6bf1318c39db679ecc6902855c5850d163b62ba92
	CONSUMER_SECRET=7959943d2b26f64ecccd4722f21b5c2276df6ff80fea9a093d5839f2c94775d9
	TOKEN_KEY=bd612b0147cc8c8bee13af86d0f37e8e7df953c6b537068d266b91d8de59a5f0
	TOKEN_SECRET=e556cdce1bc7d78eff3d94e531a87f006946fb4d9dcd8add530421855e58e90d

	# Not in use
  
	NSDB_NAME=netsuite  
	NSDB_USER=ki  
	NSDB_PASSWORD=Link3061  
	NSDB_HOST=172.16.1.46  
	NSDB_PORT=5432

	PORT=5443

**Step 4**

- Create environment variable file ".env.test.db" in "docker/env/" with below data

.. code-block:: sh

    POSTGRES_DB=rapid-stage    
    POSTGRES_USER=rapid-stage    
    POSTGRES_PASSWORD=rapid@23456

**Step 5**

- Init and setup environment - run bash script inside builds/

*# test is an argument and it will create docker image and containers for test environments*

.. code-block:: sh

    bash builds/init.sh test
    

*# Django DB & files migrations and superuser creations*
    
*# test and web-test are arguments*

.. code-block:: sh

    bash builds/migrate.sh test web-test

**Step 6** 
   
- Server will run on port 9001

    https://mydomain.com:9001

-----------------------------------------------------------------------------------

Test Environment Deployment
===========================

Before apply your code changes including DB migrations, must follow the below steps. Run bash script inside builds/ folder.

- Get backup from the docker though we have configured persist db with docker volumes


# Django DB backup
# test and web-test are arguments
    
.. code-block:: sh

    bash builds/backup.sh test web-test

- Rebuild by Init and setup environment


# test is an argument and it will create docker image and containers for test environments

.. code-block:: sh

    bash builds/init.sh test

# Django DB & files migrations and superuser creations
# test and web-test are arguments

.. code-block:: sh

    bash builds/migrate.sh test web-test
    
----------------------------------------------------------------------------------------------------------

Production Development and Deployment
=====================================

- Follow the same pattern of test environment setup and replace the following
    - test to prod
    - web-test to web
    - Create .env.prod and .env.prod.db in docker/env
    - remove test/stage word in env files
- E.g, Init and setup environment - run bash script inside builds/


# test is an argument and it will create docker image and containers for test environments

.. code-block:: sh

    bash builds/init.sh test

# Django DB & files migrations and superuser creations
# test and web-test are arguments

.. code-block:: sh

    bash builds/migrate.sh prod web

- Server will run on port 9000

.. toctree::
   :maxdepth: 12
   :caption: Contents:




