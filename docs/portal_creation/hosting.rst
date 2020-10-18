
Finding a place to Host your Portal
=====================================

The DivSeek Canada Portal is being designed to run within a Docker Compose deployed system of Docker containers when the application is run on a Linux server or virtual machine.

Platform Requirements:

 - An operating system able to install docker and run instances; Linux preferred.
 - The ability to expose ports to the outside world.
 - Enough physical space to house the website containing your data. You can choose to keep all of the following on a single partition of at least 720Gb or to separate into individual partitions as follows:
    - Root partition: at least 20Gb
    - Docker partition: at least 200Gb for docker
    - Each Crop partition: at least 500Gb
 - Enough processing power to quickly deliver pages (at least 4 core, 6 GB RAM) with additional processing power required for analysis completed via Galaxy.

 .. toctree::
    :maxdepth: 1
    :caption: Host-specific Instructions:

    computecanada-openstack-setup
