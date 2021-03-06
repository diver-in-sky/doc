.. _console-module:

-------------------------------------------------------------------------------
                                   Module `console`
-------------------------------------------------------------------------------

The console module allows one Tarantool server to access another Tarantool
server, and allows one Tarantool server to start listening on an :ref:`admin port <administration-admin_ports>`.

.. module:: console

.. _console-connect:

.. function:: connect(uri)

    Connect to the server at :ref:`URI <index-uri>`, change the prompt from
    '``tarantool>``' to ':samp:`{uri}>`', and act henceforth as a client
    until the user ends the session or types ``control-D``.

    The console.connect function allows one Tarantool server, in interactive
    mode, to access another Tarantool server. Subsequent requests will appear
    to be handled locally, but in reality the requests are being sent to the
    remote server and the local server is acting as a client. Once connection
    is successful, the prompt will change and subsequent requests are sent to,
    and executed on, the remote server. Results are displayed on the local
    server. To return to local mode, enter ``control-D``.

    If the Tarantool server at :samp:`uri` requires authentication, the
    connection might look something like:
    ``console.connect('admin:secretpassword@distanthost.com:3301')``.

    There are no restrictions on the types of requests that can be entered,
    except those which are due to privilege restrictions -- by default the
    login to the remote server is done with user name = 'guest'. The remote
    server could allow for this by granting at least one privilege:
    ``box.schema.user.grant('guest','execute','universe')``.

    :param string uri: the URI of the remote server
    :return: nil

    Possible errors: the connection will fail if the target Tarantool server
    was not initiated with ``box.cfg{listen=...}``.

    **Example:**

    .. code-block:: tarantoolsession

        tarantool> console = require('console')
        ---
        ...
        tarantool> console.connect('198.18.44.44:3301')
        ---
        ...
        198.18.44.44:3301> -- prompt is telling us that server is remote

.. _console-listen:

.. function:: listen(uri)

    Listen on :ref:`URI <index-uri>`. The primary way of listening for incoming
    requests is via the connection-information string, or URI, specified in
    ``box.cfg{listen=...}``. The alternative way of listening is via the URI
    specified in ``console.listen(...)``. This alternative way is called
    "administrative" or simply :ref:`"admin port" <administration-admin_ports>`.
    The listening is usually over a local host with a Unix domain socket.

    :param string uri: the URI of the local server

    The "admin" address is the URI to listen on. It has no default value, so it
    must be specified if connections will occur via an admin port. The parameter
    is expressed with URI = Universal Resource Identifier format, for example
    "/tmpdir/unix_domain_socket.sock", or a numeric TCP port. Connections are
    often made with telnet. A typical port value is 3313.

    **Example:**

    .. code-block:: tarantoolsession

        tarantool> console = require('console')
        ---
        ...
        tarantool> console.listen('unix/:/tmp/X.sock')
        ... main/103/console/unix/:/tmp/X I> started
        ---
        - fd: 6
          name:
            host: unix/
            family: AF_UNIX
            type: SOCK_STREAM
            protocol: 0
            port: /tmp/X.sock
        ...

.. _console-start:

.. function:: start()

    Start the console on the current interactive terminal.

    **Example:**

    A special use of ``console.start()`` is with :ref:`initialization files
    <index-init_label>`. Normally, if one starts the tarantool server with
    :samp:`tarantool {initialization file}` there is no console. This can be
    remedied by adding these lines at the end of the initialization file:

    .. code-block:: lua

        local console = require('console')
        console.start()

.. _console-ac:

.. function:: ac([true|false])

   Set the auto-completion flag. If auto-completion is `true`, and the user is
   using tarantool as a client or the user is using tarantool via
   ``console.connect()``, then hitting the TAB key may cause tarantool to
   complete a word automatically. The default auto-completion value is `true`.
