

Python Decouple
================

:Authors:
    Wendyam YAMEOGO <yameogoivan10@gmail.com>
:Version: 2025-08

source: https://pypi.org/project/python-decouple/


That is a new way of handling either your environment variables or your configuration files.

Python Decouple helps you to separate settings from code. It allows you to define your settings in environment variables .env or µ.ini files,
and then access them in your Python code.

Example of .env file:

.. code-block:: python

    DEBUG=True
    TEMPLATE_DEBUG=True
    SECRET_KEY=ARANDOMSECRETKEY
    DATABASE_URL=mysql://myuser:mypassword@myhost/mydatabase
    PERCENTILE=90%
    #COMMENTED=42 <- this is a commented line

Example of a settings.ini file:

.. code-block:: python

    [settings]
    DEBUG=True
    TEMPLATE_DEBUG=%(DEBUG)s
    SECRET_KEY=ARANDOMSECRETKEY
    DATABASE_URL=mysql://myuser:mypassword@myhost/mydatabase
    PERCENTILE=90%%
    #COMMENTED=42 <- this is a commented line



To use Python Decouple, you need to install it first. You can do this using pip:

.. code-block:: bash

    pip install python-decouple

Then, you can use it in your Python code like this:

.. code-block:: python

    from decouple import config, Csv

    DEBUG = config('DEBUG', default=False, cast=bool)
    TEMPLATE_DEBUG = config('TEMPLATE_DEBUG', default=DEBUG, cast=bool)
    SECRET_KEY = config('SECRET_KEY')
    DATABASE_URL = config('DATABASE_URL')
    PERCENTILE = config('PERCENTILE', default=90, cast=int)
    ALLOWED_HOSTS = config('ALLOWED_HOSTS', default='localhost', cast=Csv())
    TIMEOUT = config('TIMEOUT', default=5.0, cast=float)


Focus on the cast parameter
---------------------------

It allows you to specify the type of the variable you want to retrieve.

Example of usage of the cast parameter:

.. code-block:: rst

    >>> os.environ['DEBUG'] = 'False'
    >>> config('DEBUG', cast=bool)
    False

    >>> os.environ['EMAIL_PORT'] = '42'
    >>> config('EMAIL_PORT', cast=int)
    42

    >>> os.environ['ALLOWED_HOSTS'] = '.localhost, .herokuapp.com'
    >>> config('ALLOWED_HOSTS', cast=lambda v: [s.strip() for s in v.split(',')])
    ['.localhost', '.herokuapp.com']

    >>> os.environ['SECURE_PROXY_SSL_HEADER'] = 'HTTP_X_FORWARDED_PROTO, https'
    >>> config('SECURE_PROXY_SSL_HEADER', cast=Csv(post_process=tuple))
    ('HTTP_X_FORWARDED_PROTO', 'https')

    >>> os.environ['LIST_OF_INTEGERS'] = '1,2,3,4,5'
    >>> config('LIST_OF_INTEGERS', cast=Csv(int))
    [1, 2, 3, 4, 5]

    >>> os.environ['COMPLEX_STRING'] = '%virtual_env%\t *important stuff*\t   trailing spaces   '
    >>> csv = Csv(cast=lambda s: s.upper(), delimiter='\t', strip=' %*')
    >>> csv(os.environ['COMPLEX_STRING'])
    ['VIRTUAL_ENV', 'IMPORTANT STUFF', 'TRAILING SPACES']


Choices Helper
--------------

Allows for cast and validation based on a list of choices. For example:

.. code-block:: rst

   >>> from decouple import config, Choices
    >>> os.environ['CONNECTION_TYPE'] = 'usb'
    >>> config('CONNECTION_TYPE', cast=Choices(['eth', 'usb', 'bluetooth']))
    'usb'

    >>> os.environ['CONNECTION_TYPE'] = 'serial'
    >>> config('CONNECTION_TYPE', cast=Choices(['eth', 'usb', 'bluetooth']))
    Traceback (most recent call last):
    ...
    ValueError: Value not in list: 'serial'; valid values are ['eth', 'usb', 'bluetooth']

    >>> os.environ['SOME_NUMBER'] = '42'
    >>> config('SOME_NUMBER', cast=Choices([7, 14, 42], cast=int))
    42

.. important::

    Why use Python Decouple and not just os.environ ?

    .. note::
       | os.environ <--- returns strings only, so you have to convert them to the right type manually.
       | Python Decouple <--- allows you to specify the type of the variable directly in the config function using the cast parameter.
