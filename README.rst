aiosmtplib
==========

|travis|

------------


Introduction
------------

Aiosmtplib is an implementation of the python stdlib smtplib using asyncio, for
use in asynchronous applications.

Basic usage:

.. code-block:: python

    import asyncio
    import aiosmtplib

    loop = asyncio.get_event_loop()
    smtp = aiosmtplib.SMTP(hostname='localhost', port=1025, loop=loop)
    loop.run_until_complete(smtp.connect())

    async def send_a_message():
        sender = 'root@localhost'
        recipient = 'somebody@localhost'
        message = "Hello World"
        await smtp.sendmail(sender, [recipient], message)

    loop.run_until_complete(send_a_message())



Connecting to an SMTP server
----------------------------

Initialize a new ``aiosmtplib.SMTP`` instance, then run its ``connect``
coroutine. Unlike the standard smtplib, initializing an instance does not
automatically connect to the server.

Sending messages
----------------

Use ``SMTP.sendmail`` to send raw messages. Allowed arguments are:

``sender``
    The address sending this mail.
``recipients``
    A list of addresses to send this mail to.  A bare string will be treated as a list with 1 address.
``message``
    The message string to send.
``mail_options``
    List of options (such as ESMTP 8bitmime) for the mail command.
``rcpt_options``
    List of options (such as DSN commands) for all the rcpt commands.

Use ``SMTP.send_message`` to send ``email.message.Message`` objects.

Using SMTP over SSL
-------------------

Simply use ``SMTP_SSL`` or use ``SMTP`` with ``ssl`` option set to ``True``.

Using asynchronous context manager
----------------------------------

Async context manager will ensure the connection is established before the block and it is quited after the block.

.. code-block:: python

    import asyncio
    import aiosmtplib

    loop = asyncio.get_event_loop()

    async def send_a_message():
        sender = 'root@localhost'
        recipient = 'somebody@localhost'
        message = "Hello World"
        async with aiosmtplib.SMTP(hostname='localhost', port=1025, loop=loop) as smtp:
            await smtp.sendmail(sender, [recipient], message)


    loop.run_until_complete(send_a_message())


.. |travis| image:: https://travis-ci.org/cole/aiosmtplib.svg?branch=master
           :target: https://travis-ci.org/cole/aiosmtplib
           :alt: "aiosmtplib TravisCI build status"
