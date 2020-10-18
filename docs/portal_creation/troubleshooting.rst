Troubleshooting
=================

ElasticSearch
---------------

During the creation of the ElasticSearch indexing container in the Docker Tripal system, one may run up against another resource limit, reported by the following error message:

.. code::

  max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

This solution to this is to add this line:

.. code::

  vm.max_map_count=262144

in your /etc/sysctl.conf file on the host system and run

.. code::

  sudo sysctl -p

to reload configuration with new value.
