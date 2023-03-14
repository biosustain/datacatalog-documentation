# [WiP] How to run ckan 2.9.4 on M1 Mac
**NOTE: While this guide is aimed mostly toward M1-based Macbooks, most of it can be als applied to older, x86-based Macbooks.**
## Background
There are multiple ways to install ckan and it's dependencies:
1. Host all of it locally on your machine - only Linux is officially supported by ckan
2. Use a Linux-based VM to host it, if you work on e.g. Windows or MacOS
3. Host all of it in docker

Not every dependency of ckan has a native/supported version for M1 CPUs. And even if, it's difficult to configure and
orchestrate them, that's why option 1 is not really viable.
Option 2 is automatically rejected as M1 doesn't support virtualization, therefore cannot run VMs.
Option 3 seems ideal, however due to nature of docker on M1, we don't have easy access to the docker volumes, which
is problematic when you want to edit the ckan config file, or the secrets file.

There is a 4th option, which is not mentioned in the documentation, but it seems to combine the best traits of options
1 and 3. That is - host the dependencies as docker containers (e.g. with docker-compose) and ckan itself on the machine.
This guide will try to describe how to do it.
## Prerequisites
* Docker
* python3 (I use 3.6.15 for ckan - some newer versions were not working correctly)
## Preparation
1. Clone ckan repo (this example is with ssh, feel free to use html if you prefer):
```
cd /path/to/my/projects
git clone git@git-rdm.ait.dtu.dk:ckan/ckan.git
git checkout tags/ckan-2.9.4
```

2. Copy and paste `contrib/docker/docker-compose.yml` to a location of your choice (or leave it as it is and just edit).
3. Edit it. Our goal is to remove ckan from it (the plan is to host it locally), but leave out all the other dependencies as they are. To achieve it, the following steps should be taken:
* remove the ckan_* volumes from the **volumes** section
* remove the whole **ckan** section

(Maybe it doesn't apply in your case) I had issues with redis when building the images, and a fix was to
* change **redis:latest** to **redis:6.2**.

4. Build the images
```
docker-compose up -d --build
```
5. Set-up ckan

    1. Follow [point 2 of the official guide](https://docs.ckan.org/en/2.9/maintaining/installing/install-from-source.html#install-ckan-into-a-python-virtual-environment) to set-up the *.env* file and ckan's virtual environment.

        NOTE: I had issues with openssl when installing the requirements, the solution was to run `brew install openssl` in terminal, and then exporting two env variables:
        
        ```
        export CPPFLAGS="-I/opt/homebrew/opt/openssl@1.1/include"
        export LDFLAGS="-L/opt/homebrew/opt/openssl@1.1/lib"
        ```

    2. Follow [point 3 of the official guide](https://docs.ckan.org/en/2.9/maintaining/installing/install-from-source.html#setup-a-postgresql-database) to setup a PostgreSQL database.
    
        NOTE: Instead of **sudo -u postgres [...]**, as in the guide, use **docker-compose -f <path_to_yml> exec db [...] -U ckan**. The **-U ckan** is important!

    3. Follow [point 4 of the official guide](https://docs.ckan.org/en/2.9/maintaining/installing/install-from-source.html#create-a-ckan-config-file) to create a CKAN config file.
    
        I had to add **?sslmode=disable** to postgres urls, otherwise I was getting errors.

        I put **http://localhost:5000** for ckan.site_url and **default** for ckan.site_id.
    
    4. Follow [point 6 and 7 of the official guide](https://docs.ckan.org/en/2.9/maintaining/installing/install-from-source.html#link-to-who-ini) to link *whi.ini* and create database tables.

    4. Execute the built-in setup script against the db container:
    ```
    ckan -c <path_to_ckan_ini> datastore set-permissions | docker-compose -f <path_to_yml> exec -T db psql -U ckan --set ON_ERROR_STOP=1
    ```

    After this step, the datastore database is ready to be enabled in the ckan.ini.

    5. Add **datastore** and **datapusher** to ckan.plugins, set **/var/lib/ckan** for ckan.storage_path.

    Additionally set ckan.datastore.write_url and ckan.datastore.read_url, same way as in point 5.3, but specifying *datastore* as the name of the database.

    6. Run ckan:
    ```
    ckan -c <path_to_ckan_ini> run
    ```

    7. Add admin account:
    ```
    ckan -c <path_to_ckan_ini> sysadmin add <admin_account_name>
    ```

## References:
* [Installing CKAN with Docker Compose](https://docs.ckan.org/en/2.9/maintaining/installing/install-from-docker-compose.html)
* [Installing CKAN from source](https://docs.ckan.org/en/2.9/maintaining/installing/install-from-source.html)