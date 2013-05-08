=================
SQL Alchemy Notes
=================

Now that we did all the work in plain SQL lets look at using SQL Alchemy. The first steps we need to setup SQLAlchemy to connect to our database.

.. code-block:: python

    from sqlalchemy import create_engine

    engine = create_engine('mysql://USERNAME:PASSWORD@localhost/DATABASE', echo=True)

    # For Sqlite
    # engine = create_engine('sqlite:///FILENAME', echo=True)

The echo=True line turns on the logging feature of SQL Alchemy. It uses the python logging module. It can log all queries.

Basically this is the connection string to the database. simply creating this does nothing except for creates the pointer to connect.

Setup Mappings
--------------

SQLAlchemy is very powerful and allows you to connect to your database in a pythonic way. One of the first steps we need to do is tell sqlalchemy about our database. There are more than one way to do this in SQLAlchemy but we are going to use the declarative system. I chose this becuase it explicity discribes what our database looks like.

.. code-block:: python

    from sqlalchemy.ext.declarative import declarative_base
    Base = declarative_base()

The declarative_base class is the class that all other classes must inherit from.

.. code-block:: python

    from sqlalchemy import Column, Integer, String, DateTime

    class User(Base):
        __tablename__ = 'users'
        id = Column(Integer, primary_key=True)
        first = Column(String)
        last = Column(String)
        pw = Column(String)
        email = Column(String)

        def __init__(self, first, last, pw, email):
            self.first = first
            self.last = last
            self.pw = pw
            self.email = email

        def __repr__(self):
            return "<User('%s %s')>" % (self.first, self.last)

    class login(Base):
        __tablename__ = 'logins'
        id = Column(Integer, primary_key=True)
        userid = Column(Integer)
        logindate = Column(DateTime)
        message = Column(String)

Now we have everything defined we are ready to start talking to the database. First we need to create one more small thing. This is the database session that we actually use to talk to the database.

.. code-block:: python

    from sqlalchemy.orm import sessionmaker
    Session = sessionmaker(bind=engine)
    DBSession = Session()

    records = DBSession.query(User)

    for record in records:
        print "%s %s" % (record.first, record.last)


Now lets only return email address

.. code-block:: python

    records = DBSession.query(User).order_by(User.last)

    for record in records:
        print "%s %s" % (record.first, record.last)

Lets add a method to the users class to retreive users by last name

.. code-block:: python

    class User(Base):
        __tablename__ = 'users'
        id = Column(Integer, primary_key=True)
        first = Column(String)
        last = Column(String)
        pw = Column(String)
        email = Column(String)

        def __init__(self, first, last, pw, email):
            self.first = first
            self.last = last
            self.pw = pw
            self.email = email

        def __repr__(self):
            return "<User('%s %s')>" % (self.first, self.last)

        @classmethod
        def by_last(cls, last):
            return DBSession.query(User).filter(User.last = last).all()

        #Because we used the @classmethod decorator we can do the query
        #without having to instanciate the object

        records = User.by_last('Banks')

        for record in records:
            print "%s %s" % (record.first, record.last)
