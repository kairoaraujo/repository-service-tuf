========================================
Repository Service for TUF Documentation
========================================

.. include:: ../../README.rst
  :end-before: rstuf-image-high-level

.. image:: /_static/1_1_rstuf.png

Project background and motivation
=================================

`TUF`_ provides a flexible framework and specification that developers can
adopt and an excellent Python Library (`python-tuf`_) that provides two APIs
for low-level Metadata management and client implementation.

Implementing `TUF`_ requires sufficient knowledge of `TUF`_ to design how to
integrate the framework into your repository and hours of engineering work to
implement.

RSTUF was born as a consequence of working on the implementation of `PEP 458
<https://peps.python.org/pep-0458/>`_ in the `Warehouse
<https://warehouse.pypa.io>`_ project which powers the `Python Package Index
(PyPI) <https://pypi.org>`_.

Due to our experience with the complexity and fragility of deep integration into
an intricate platform, we began designing how to implement a reusable TUF platform
that is flexible to integrate into different flows and infrastructures.

Repository Service for TUF's goal is to be an easy-to-use tool for Developers,
DevOps, and DevOpsSec teams working on the delivery process.


.. _TUF: https://theupdateframework.io
.. _python-tuf: https://pypi.org/project/tuf/


Documentation List
==================

.. toctree::
   :maxdepth: 1

   guide/index
   devel/index


.. _TUF: https://theupdateframework.io
.. _python-tuf: https://pypi.org/project/tuf/
.. _ROADMAP: devel/roadmap.html
.. _Redis: https://redis.io
