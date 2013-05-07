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

.. code-block:: sql

    SELECT * from users

Maybe we only want to select email addresses

.. code-block:: sql

    SELECT email from users

We can add modifiers to change the output we get. In this example we will sort by last name in reverse alphebitical order.

.. code-block:: sql

    SELECT * FROM users ORDER BY last DESC

Same as above but in assending order.

.. code-block:: sql

    SELECT * FROM users ORDER BY last ASC

We can also filter out results and only get the results that we want. Lets show all users with the last name of Banks

.. code-block:: sql

    SELECT * FROM users WHERE last = 'Banks'

We can also sort this shorter list as well.

.. code-block:: sql

    SELECT * FROM users WHERE last = 'Banks' ORDER BY first ASC

Inserting Data
--------------

Inserting data is really easier than selecting data. The basic syntax is

**INSERT INTO TABLENAME(COLUMN1, COLUMN2, ...) VALUES (VALUE1, VALUE2);**

Lets insert a new user into the users table.

.. code-block:: sql

    INSERT INTO users(first, last, email, pw) VALUES ('Kellan', 'Jacobs', 'not@telling.com', 'MyS@fePa&&w0rd');

    SELECT * FROM users where first='Kellan' AND last='jacobs'

You do not need to add any value you don't want too unless the database has a NOT NULL flag set on the column. Also you don't have to update the primary key for this table because it is set to auto increment.

Update Data
-----------

To update data it is a mix of select and insert statement.

**UPDATE TABLENAME SET column1=value, column2=value WHERE COLUMN=value;*

So lets change the password for the user we just created.

.. code-block:: sql

    UPDATE users set pw='N3wPa$$w0rd' where id=101;
    SELECT * FROM users where ID=101;

Delete data
-----------

Deleting data is also as easy.

**DELETE FROM table_name WHERE some_column = some_value**

Lets delete the new users I created.

.. code-block:: sql

    DELETE FROM users WHERE id = 101;
    SELECT * FROM users where ID=101;

Joins
-----

Often you have to get data from more than one table. I am going to go over a couple of basic join statements. Joins is where SQL can get complicated. I am going to only cover some basics, because we want to actually get to the SQLAlchemy part.

Also there are two ways we can do a join. First you can use the proper JOIN keyword or you can use there were clause. Because we are going to join more than one table we need to by more explicit on what columns we want to select.

**Join using where**

I will admit these are the ones I do most of the time because I don't have to to lookup the syntax.

.. code-block:: sql

    select u.id, u.first, u.last, l.message from users as u, logins as l where u.id=l.userid

Wait a minute what is with that u and l stuff. Well what I did to make my sql shorter was alias the tables. by using the *AS* keyword. I can for this query only make shorter alias for anything. That way I dont have to type out the full table name for each column.

Now lets do the same thing using the join syntax

.. code-block:: sql

    select users.id, users.first, users.last, logins.message
    from users
    JOIN logins on users.id=logins.userid;

We can also chain more than one filter

.. code-block:: sql

    select users.id, users.first, users.last, logins.message
    from users JOIN logins on users.id=logins.userid
    AND logins.message = 'Attempts';
