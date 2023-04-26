############
Introduction
############

Repository Service for TUF
##########################

RSTUF Components
================

Repository Service for TUF (RSTUF) is a combination of micro-services provided
as containers:

 * :ref:`guide/repository-service-tuf-api/index:Repository Service for TUF API`
   (RSTUF API ``repository-service-tuf-api``)
 * :ref:`guide/repository-service-tuf-worker/index:Repository Service for TUF Worker`
   (RSTUF Worker ``repository-service-tuf-worker``)

Repository Service for TUF (RSTUF) also provides a Command Line Interface tool
to manage your RSTUF deployment.

 * :ref:`guide/repository-service-tuf-cli/index:Repository Service for TUF CLI`
   (RSTUF CLI ``repository-service-tuf-cli``)

Repository Service for TUF API (RSTUF API)
------------------------------------------

The RSTUF API servers an HTTP REST API for integration and administration.

It publishes new tasks to the message queue/broker and uses the Redis server
to get information about the task status and check the RSTUF settings.

Repository Service for TUF Worker (RSTUF Worker)
------------------------------------------------

The RSTUF Worker is a task consumer from the message queue/broker. It processes
the tasks updating TUF metadata using the Postgres database.
During it operations uses the online key from the Key Vault backend service,
publishes the TUF metadata in the Storage backend service and uses the
Redis server for TUF settings and store the task results.

Repository Service for TUF Command Line
---------------------------------------

The Repository Service for TUF Command Line Interface
(``repository-service-tuf-cli``) is a CLI Python application to manage the
Repository Service for TUF.

The CLI supports the initial setup, termed a ceremony, where the first repository
metadata are signed and the service is configured, generating tokens to be used
by integration (i.e., CI/CD tools).


The TUF Metadata
================

Repository Service for TUF (RSTUF) secures downloads with signed repository
metadata using a design based on Python's `PEP 458 – Secure PyPI downloads
with signed repository metadata <https://peps.python.org/pep-0458/>`_.

.. image:: /_static/1_1_rstuf_metadata.png

Some configurations are possible during the
:ref:`guide/repository-service-tuf-cli/index:Ceremony (``ceremony\`\`)`, such
as the number of keys, thresholds, expiration, etc.

New targets (file, package, update, installer) can be added to the signed
repository metadata using REST API calls, making it easy to integrate into
CI/CD tools.

You can deploy Repository Service for TUF as a single server or a distributed
service (to scale for more active repositories) at the edge, on-premises, or
in cloud environments.

Check out the :ref:`how to deploy <guide/deployment/index:Deployment>`.

.. note::

    If you provide users with download or update tools, you need to add
    functionality to your tools to check the signed metadata.


