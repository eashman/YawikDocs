Upgrade
=======


.. _backup-database:

1) backup your mongo data with the mongodump_ command. This will create a directory ``dump``
containing all your mongo databases. You can restore these databases with the mongorestore_ command.

YAWIK creates all needed mongo indexes automatically. But this only works, if an index is not already available. Since
some indexes have changed in the past, it might be required to drop all indexes, so YAWIK will be able to create all
needed indexes.

To drop all indexes, go to your mongo shell and type:

.. code-block:: sh

  set1:PRIMARY> db.users.dropIndexes();
  set1:PRIMARY> db.applications.dropIndexes();
  set1:PRIMARY> db.jobs.dropIndexes();


.. _mongodump: https://docs.mongodb.org/manual/reference/program/mongodump/
.. _mongorestore: https://docs.mongodb.org/manual/reference/program/mongorestore/


2) Move your YAWIK Installation to a new location, so you are able to undo the upgrade any time.

3) Install the new Version. Eighter via ``git`` or unpack the latest ZIP/TGZ Package from sourceforge. In contrast to
a fresh installation, you do not access your updated YAWIK via a Browser. Copy all ``config/autoload/*`` files of your
moved old YAWIK installation into to ``config/autoload`` directory of your new installation.

4) Now you can access your new YAWIK via a Browser.




