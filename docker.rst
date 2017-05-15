############
Docker
############


1. Install Docker
""""""""""""""""""
|   Head to https://www.docker.com/community-edition#/download and download and install docker for your specfic plateform, docker is supported by Windows, Mac Os, Linux.
|   Once installed you'll have access to docker command (docker and dock-compose) using the command line.

::

    docker --version
    output : Docker version 17.03.1-ce, build c6d412e

::

    docker-compose --version
    output : docker-compose version 1.12.0, build b31ff33

2. Clone installation package
"""""""""""""""""""""""""""""
::

    git clone git@github.com:Verteego/vds.git

3. Install Verteego DS
""""""""""""""""""""""

3.1 using docker-compose on a single machine
............................................

- Using the command line, go to the directory deployment/docker inside the cloned repository from step 2.

::

    docker-compose up

- Navigate to http://localhost:33330

4. Sign in
""""""""""

For your first sign in you can use the following credentials. For security reasons, remember to change them or delete the default user after your first login.

- Username: vds-user

- Password: verteego