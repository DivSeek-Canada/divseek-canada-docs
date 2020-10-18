Deployment of the Portal System
===================================

the DivSeek Canada Portal customizes a git fork of the `Galaxy Genome Annotation "Dockerized GMOD" code base <https://github.com/galaxy-genome-annotation/dockerized-gmod-deployment>`_. The core of the customization is in the Docker Compose build file (docker-compose.yml) on the divseek-canada-build branch. Prior to running the build, however, some configuration tasks need to be completed.

Docker Compose Parameter Setting
----------------------------------

The docker-compose.yml is parameterized for (crop) site specific site deployment using environment variables defined in a .env file, derived from the available template.env file, which needs to be copied into .env then customized to point to your actual public host particulars. For example, you can change the admin account particulars, i.e.

.. code::

  DC_ADMIN_USER=divseek_admin
  DC_ADMIN_EMAIL=admin@divseekcanada.ca

or perhaps, the site crop, title, hostname, http protocol and Tripal path:

.. code::

  DC_CROP=Sunflower
  DC_SITE_NAME="DivSeek Canada"
  DC_SITE_BASE_HOSTNAME=sunflower.divseekcanada.ca
  DC_BASE_URL_PROTO=https://

Docker Compose Preliminaries
------------------------------

The general project launch steps noted in the `original GMOD deployment project README <https://github.com/galaxy-genome-annotation/dockerized-gmod-deployment/README.md>`_ are otherwise followed, albeit with the divseek-canada-build customized docker-compose.yml file. To start off which, we can pre-load our Docker system with the required pre-built images, as follows:

.. code::

  docker-compose pull  # Pulls in the required service Docker images

NGINX Proxy Configuration
---------------------------

The original dockerized-gmod-deployment includes a 'proxy' service that runs the NGINX web server software in a container. To configure this package, the project specifies an NGINX configuration under a subfolder nginx. Unfortunately, most realistic site deployments (e.g. with https:// SSL configuration, particular hostnames, etc.) generally necessitates the creation of a customized NGINX file which, although taking the docker compose system into account, needs to also include additional elements, the composition of which this project cannot foresee and hard code (nor parameterize directly, since NGINX doesn't allow for that). The compromise we've taken here, in the divseek-canada-build branch, is to convert the default NGINX into a template, then provide some suggestions here on how to customize and properly deploy your copy of the template for use in the system. The following protocol is simply one that worked for us; those of you with deeper knowledge can likely converge on your own solution to the NGINX configuration.

1. In the divseek-canada-portal (a.k.a. dockerized-gmod-deployment) project, go into the nginx project subfolder and copy over the nginx/default.conf-template into nginx/default.conf.

2. Editing the nginx/default.conf file, rename the name my-divseek-portal-server of the server_name parameter and everywhere else that it is found inside the server block, to the DC_SITE_BASE_HOSTNAME hostname which you set in your .env file (e.g. sunflower.divseekcanada.ca). You should make sure that your DNS is properly set up to point to your cloud server IP address (usually with an A record) before proceeding to the next (certbot) step of the configuration. You may need to wait a short while for your DNS entry to propagate through the internet before attempting to run the configuration.

3. Configure https:// SSL certificate configuration. Using the `certbot tool <https://certbot.eff.org/>`_ of the free certificate `LetsEncrypt initiative <https://letsencrypt.org/>`_ is a nice way forward here, but you need to be a bit clever to achieve this since certbot generally requires that you specify the web server and operating system you are using so it can make reasonable assumptions about where things should go. This task is facilitated somewhat by using a `Docker image for Certbox <https://hub.docker.com/r/certbot/certbot/>`_ plus `available instructions on how to set things up by a clever developer named 'Philipp' <https://medium.com/@pentacent/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71>`_. We've partly applied the required certbot customizations to the docker-compose.yml file, but you'll need to apply the NGINX configuration edits and run the indicated procedure for the initial generation of SSL certificates for insertion into the configuration. For convenience, Philipp's init-letsencrypt.sh has been customized (to read the host name set in your .env file) and embedded in our project. If you have set your .env file correctly, then You may therefore run it as follows:

.. code::

  sudo ./init-letsencrypt.sh

4. You should now go back into the nginx/default.conf file and uncomment the directive include ./services.conf to enable inclusion of the full set of NGINX service proxy redirections.

Now we are set to build the system and fire it up.

Running the Docker-Compose Build
-----------------------------------

It is recommended to first start the databases (first making sure that you are in the root project directory for the code):

.. code::

  cd /opt/divseekcanada/divseek-canada-portal
  docker-compose up -d apollo_db chado # Launches the database containers

In a new terminal, in the same folder, you can run docker-compose logs -f in order to follow the build process and decypher errors.

.. code::

  # Wait for tripal to come up and install Chado.
  docker-compose up -d --build tripal

  # It takes a few minutes until you see an apache start-up notification.
  # Then, run a non-specific compose build to bring up the rest of the services.
  docker-compose up -d
