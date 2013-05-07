===========
MySQL Notes
===========

* Selecting data
* Inserting data
* Updating data
* Deleting data
* Joins

Selecting data
--------------

The basic sql search patern is

SELECT <columns> from <table> MODIFIERS

If we want to select all the records and all the columns from the user table we would use the following.

.. code-block:: console

    SELECT * from users

Maybe we only want to select email addresses

.. code-block:: console

    SELECT email from users

We can add modifiers to change the output we get. In this example we will sort by last name in reverse alphebitical order.

.. code-block:: console

    SELECT * FROM users ORDER BY last DESC

Same as above but in assending order.

.. code-block:: console

    SELECT * FROM users ORDER BY last ASC

We can also filter out results and only get the results that we want. Lets show all users with the last name of Banks

.. code-block:: console

    SELECT * FROM users WHERE last = 'Banks'

We can also sort this shorter list as well.

.. code-block:: console

    SELECT * FROM users WHERE last = 'Banks' ORDER BY first ASC