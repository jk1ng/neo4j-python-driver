#############################
Neo4j Python Driver |version|
#############################

.. warning::
    This API docs is not production ready!


The Official Neo4j Driver for Python.

Neo4j versions supported:

* Neo4j 4.0 - Using the Bolt Protocol Version 4.0
* Neo4j 3.5 - Using the Bolt Protocol Version 3

Python versions supported:

* Python 3.8
* Python 3.7
* Python 3.6
* Python 3.5


**Note:** Python 2.7 support has been dropped.

.. note::
   The driver may still work with older versions of python.

   The previous driver `Python Driver 1.7`_ supports older versions of python,
   the **Neo4j 4.0** will work in fallback mode (using Bolt Protocol Version 3) with that driver.

*************
Quick Example
*************

.. code-block:: python

    from neo4j import GraphDatabase

    uri = "bolt://localhost:7687"
    driver = GraphDatabase.driver(uri, auth=("neo4j", "password"))

    def print_friends_of(tx, name):
        for record in tx.run("MATCH (a:Person)-[:KNOWS]->(f) "
                             "WHERE a.name = {name} "
                             "RETURN f.name", name=name):
            print(record["f.name"])

    with driver.session() as session:
        session.read_transaction(print_friends_of, "Alice")

    driver.close()


************
Installation
************

To install the latest stable driver release, use:

.. code:: bash

    python -m pip install neo4j


**Note:** It is always recommended to install python packages for user space in a virtual environment.

.. code:: bash

    python -m venv sandbox

.. code:: bash

    source sandbox/bin/activate

.. code:: bash

    deactivate


****************
Breaking Changes
****************

Version Scheme Changes
======================

The version number have jumped from **Python Driver 1.7** to **Python Driver 4.0** to align with the Neo4j Database version scheme.


Namespace Changes
=================

.. code-block:: python

    import neo4j.v1

Have changed to

.. code-block:: python

    import neo4j


Secure Connection
=================

**Neo4j 4.0** is by default configured to use a **non secure connection**.

The driver configuration argument :code:`encrypted` is by default set to :code:`False`.

**Note:** To be able to connect to **Neo4j 3.5** set :code:`encrypted=True` to have it configured as the default for that setup.

.. code-block:: python

    from neo4j import GraphDatabase

    driver = GraphDatabase("bolt://localhost:7687", auth=("neo4j", "password"), encrypted=True)
    driver.close()


Bookmark Changes
================

Bookmarks is now a Bookmark class instead of a string.


Exceptions Changes
==================

The exceptions in :code:`neo4j.exceptions` have been updated and there is internal exceptions starting with the naming :code:`Bolt` that should be propagated into the exceptions API.


URI Scheme Changes
==================

**bolt+routing** have been renamed to **neo4j**.


Class Renaming Changes
======================

* :code:`BoltStatementResult` is now :code:`Result`
* :code:`StatementResultSummary` is now :code:`ResultSummary`
* :code:`Statement` is now :code:`Query`


Argument Renaming Changes
=========================

* :code:`statement` is now :code:`query`
* :code:`cypher` is now :code:`query`
* :code:`Session.run(cypher, ...` is now :code:`Session.run(query, ...`
* :code:`Transaction.run(statement, ...` is now :code:`Transaction.run(query, ...`
* :code:`StatementResultSummary.statement` is now :code:`ResultSummary.query`
* :code:`StatementResultSummary.statement_type` is now :code:`ResultSummary.query_type`
* :code:`StatementResultSummary.protocol_version` is now :code:`ResultSummary.server.protocol_version`


API Changes
=========================

* :code:`Result.summary()` have been replaced with :code:`Result.consume()`, this behaviour is to consume all remaining records in the buffer and returns the ResultSummary.


Dependency Changes
==================

The dependency :code:`neobolt` have been removed.


Configuration Name Changes
==========================

* :code:`max_retry_time` is now :code:`max_transaction_retry_time`


*****************
API Documentation
*****************

.. toctree::
   :maxdepth: 1

   driver
   errors
   results
   transactions
   usage_patterns
   types/core
   types/graph
   types/spatial
   types/temporal


*****************
Other Information
*****************

* `Neo4j Documentation`_
* `The Neo4j Drivers Manual`_
* `Neo4j Quick Reference Card`_
* `Example Project`_
* `Driver Wiki`_ (includes change logs)
* `Migration Guide - Upgrade Neo4j drivers`_

.. _`Python Driver 1.7`: https://neo4j.com/docs/api/python-driver/1.7/
.. _`Neo4j Documentation`: https://neo4j.com/docs/
.. _`The Neo4j Drivers Manual`: https://neo4j.com/docs/driver-manual/current/
.. _`Neo4j Quick Reference Card`: https://neo4j.com/docs/cypher-refcard/current/
.. _`Example Project`: https://github.com/neo4j-examples/movies-python-bolt
.. _`Driver Wiki`: https://github.com/neo4j/neo4j-python-driver/wiki
.. _`Migration Guide - Upgrade Neo4j drivers`: https://neo4j.com/docs/migration-guide/4.0/upgrade-driver/
