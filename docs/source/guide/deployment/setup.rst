#############
Service Setup
#############

Repository Service for TUF (RSTUF) has two specific processes as part of the
initial setup: Ceremony and Bootstrap.

.. note::
    * The setup and configuration requirements:

      - Set of root key(s) and online key for signing
      - RSTUF service deployed


Ceremony and Bootstrap
######################

.. note::
    * It is a one-time process to setup the RSTUF service. If this process is
      completed during the :ref:`deployment <guide/deployment/index:Deployment>`
      do not run it again.

RSTUF Command Line Interface provides a guided process for the Ceremony and
Bootstrap.

To make this process easier,
the :ref:`guide/repository-service-tuf-cli/index:Repository Service for TUF CLI`
provides an interactive guided process to perform the
:ref:`Ceremony <guide/repository-service-tuf-cli/index:Ceremony (``ceremony\`\`)>`.

.. note::
    Required RSTUF CLI installed
    (See :ref:`guide/repository-service-tuf-cli/index:Installation`)

.. code::

    ❯ rstuf admin ceremony -h


Ceremony
========

The Ceremony defines the RSTUF settings/configuration and generates the initial
signed TUF root metadata.

It does not activate the RSTUF service. It generates the required JSON payload
to bootstrap the RSTUF service.

The Ceremony can be done :ref:`guide/deployment/setup:Connected` as a specific
step or :ref:`guide/deployment/setup:Disconnected`, combined with
:ref:`guide/deployment/setup:Bootstrap`.

This process generates the initial metadata and defines some settings of the
TUF service.

.. collapse:: See settings details

    * Root metadata expiration policy

        Defines how long this metadata is valid, for example, 365 days (year).
        This metadata is invalid when it expires.

    * Root number of keys

        It is the total number of root keys (offline keys) used by the TUF Root
        metadata.
        The number of keys implies that the number of identities is the TUF
        Metadata’s top-level administrator.

        .. note::
          * Updating the Root metadata with new expiration, changing/updating keys or
            the number of keys, threshold, or rotating a new online key and sign
            requires following the :ref:`guide/general/usage:Metadata Update`
            process.

        .. note::
            RSTUF requires all Root key(s) during the
            :ref:`guide/deployment/setup:Ceremony`.

        .. note::
            RSTUF requires at least the threshold number of Root key(s) for
            :ref:`guide/general/usage:Metadata Update`.


    * Root key threshold

        The minimum number of keys required to update and sign the TUF Root
        metadata.

    * Targets, BINS, Snapshot, and Timestamp metadata expiration policy

        Defines how long this metadata is valid. The metadata is invalid when it
        expires.

    * Targets number of delegated hash bin roles

        The target metadata file might contain a large number of artifacts.
        The target role delegates trust to the hash bin roles to
        reduce the metadata overhead for clients.

    * Signing

        This process will also require the Online Key and Root Key(s) (offline) for
        signing the initial root TUF metadata.

The settings are guided during :ref:`Ceremony <guide/repository-service-tuf-cli/index:Ceremony (``ceremony\`\`)>`.

Disconnected
------------

The disconnected Ceremony will only generate the required JSON payload
(``ceremony-payload.json``) file. The :ref:`guide/deployment/setup:Bootstrap`
requires the payload.

.. note::
    The payload (``ceremony-payload.json``) contains only public data, it does
    not contain any private keys.

This process is appropriate when performing the Ceremony on a disconnected computer
to RSTUF API to perform the :ref:`guide/deployment/setup:Bootstrap` later as a
separate step.

.. code::

    ❯ rstuf admin ceremony -s
    Saved result to 'ceremony-payload.json'

If the Ceremony is done disconnected, the next step is to perform the bootstrap.


Connected
---------

.. note::

    Apollogies, this feature has been refactored and is not available in the
    current version of the RSTUF CLI.
    Please use the Ceremony :ref:`guide/deployment/setup:Disconnected`.


Bootstrap
=========

.. If a Ceremony :ref:`guide/deployment/setup:Connected` is complete, skip this,
.. as the RSTUF service is ready.

To perform the boostrap you require the payload generated during the
:ref:`guide/deployment/setup:Bootstrap`.

You can do it using the rstuf admin-legacy

.. code::

    ❯ rstuf admi-legacy ceremony -b -u -f ceremony-payload.json --api-url https://rstuf-api-url
    Starting online bootstrap
    Bootstrap status: ACCEPTED (c1d2356d25784ecf90ce373dc65b05c7)
    Bootstrap status:  STARTED
    .Bootstrap status:  SUCCESS
    Bootstrap completed using `ceremony-payload.json`. 🔐 🎉

Alternatively, you can use the refactored `rstuf admin` and use curl to send the payload to the RSTUF API.

.. code::

    ❯ curl -X POST -d @ceremony-payload.json https://rstuf-api-url/api/v1/bootstrap
