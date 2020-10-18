
Install Pre-requisites
========================

Installation of Docker
------------------------

To run Docker, you'll obviously need to `install Docker first <https://docs.docker.com/engine/installation/>`_ in your target Linux operating environment (bare metal server or virtual machine running Linux).

For our installations, we typically use Ubuntu Linux, for which there is an `Ubuntu-specific docker installation <https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-repository>`_ using the repository. Note that you should have 'curl' installed first before installing Docker:

.. code::

  sudo apt install curl

For other installations, please find instructions specific to your choice of Linux variant, on the Docker site.

Testing Docker
^^^^^^^^^^^^^^^^

In order to ensure that Docker is working correctly, run the following command:

.. code::

  sudo docker run hello-world

This should result in something akin to the following output:

.. code::

  Unable to find image 'hello-world:latest' locally
  latest: Pulling from library/hello-world
  ca4f61b1923c: Pull complete
  Digest: sha256:be0cd392e45be79ffeffa6b05338b98ebb16c87b255f48e297ec7f98e123905c
  Status: Downloaded newer image for hello-world:latest

  Hello from Docker!
  This message shows that your installation appears to be working correctly.

  To generate this message, Docker took the following steps:
   1. The Docker client contacted the Docker daemon.
   2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
      (amd64)
   3. The Docker daemon created a new container from that image which runs the
      executable that produces the output you are currently reading.
   4. The Docker daemon streamed that output to the Docker client, which sent it
      to your terminal.

  To try something more ambitious, you can run an Ubuntu container with:

    docker run -it ubuntu bash

    Share images, automate workflows, and more with a free Docker ID:
      https://cloud.docker.com/

    For more examples and ideas, visit:
      https://docs.docker.com/engine/userguide/

Installing Docker Compose
---------------------------

You will then also need to `install Docker Compose <https://docs.docker.com/compose/install/>`_ alongside Docker on your target Linux operating environment.

.. note::

  Note that under Ubuntu, you likely need to do a bit more preparation to avoid having to run docker (and docker-compose) as 'sudo'. See `here <https://docs.docker.com/install/linux/linux-postinstall/>`_ for details on how to fix this.

Testing Docker Compose
^^^^^^^^^^^^^^^^^^^^^^^^

In order to ensure Docker Compose is working correctly, issue the following command:

.. code::

  docker-compose --version
  docker-compose version 1.22.0, build f46880f

.. note::

  Note that your particular version and build number may be different than what is shown here. We don't currently expect that docker-compose version differences should have a significant impact on the build, but if in doubt, refer to the release notes of the docker-compose site for advice.

Installing the DivSeek Canada Portal code base
------------------------------------------------

This project resides in `this Github project repository <https://github.com/DivSeek-Canada/divseek-canada-portal>`_.

First, ensure that you have the git client installed (here again, we assume Ubuntu; '$' is the bash CLI prompt):

.. code::

  sudo apt update
  sudo apt install git

Next, you should configure git with your Git repository metadata and, perhaps, activate credential management (we use 'cache' mode here to avoid storing credentials in plain text on disk)

.. code::

  git config --global user.name "your-git-account"
  git config --global user.email "your-email"
  git config --global credential.helper cache

Then, you can clone the project. A convenient location for the code is in a folder under /opt/divseekcanada:

.. code::

  cd /opt/divseekcanada
  git clone https://github.com/DivSeek-Canada/divseek-canada-portal
