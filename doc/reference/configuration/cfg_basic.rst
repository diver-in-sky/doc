* :ref:`background <cfg_basic-background>`
* :ref:`coredump <cfg_basic-coredump>`
* :ref:`custom_proc_title <cfg_basic-custom_proc_title>`
* :ref:`listen <cfg_basic-listen>`
* :ref:`pid_file <cfg_basic-pid_file>`
* :ref:`read_only <cfg_basic-read_only>`
* :ref:`snap_dir <cfg_basic-snap_dir>`
* :ref:`vinyl_dir <cfg_basic-vinyl_dir>`
* :ref:`username <cfg_basic-username>`
* :ref:`wal_dir <cfg_basic-wal_dir>`
* :ref:`work_dir <cfg_basic-work_dir>`

.. _cfg_basic-background:

.. confval:: background

    Run the server as a background task. The :ref:`logger <cfg_logging-logger>`
    and :ref:`pid_file <cfg_basic-pid_file>` parameters must be non-null for
    this to work.

    | Type: boolean
    | Default: false
    | Dynamic: no

.. _cfg_basic-coredump:

.. confval:: coredump

    Deprecated. Do not use.

    | Type: boolean
    | Default: false
    | Dynamic: no

.. _cfg_basic-custom_proc_title:

.. confval:: custom_proc_title

    Add the given string to the server's :ref:`Process title
    <administration-proctitle>` (what’s shown in the COMMAND column for
    ``ps -ef`` and ``top -c`` commands).

    For example, ordinarily :samp:`ps -ef` shows the Tarantool server process
    thus:

    .. code-block:: console

        $ ps -ef | grep tarantool
        1000     14939 14188  1 10:53 pts/2    00:00:13 tarantool <running>

    But if the configuration parameters include ``custom_proc_title='sessions'``
    then the output looks like:

    .. code-block:: console

        $ ps -ef | grep tarantool
        1000     14939 14188  1 10:53 pts/2    00:00:16 tarantool <running>: sessions

    | Type: string
    | Default: null
    | Dynamic: yes

.. _cfg_basic-listen:

.. confval:: listen

    The read/write data port number or :ref:`URI <index-uri>` (Universal
    Resource Identifier) string. Has no default value, so **must be specified**
    if connections will occur from remote clients that do not use the
    :ref:`“admin port” <administration-admin_ports>`. Connections made with
    :samp:`listen = {URI}` are sometimes called "binary protocol" or
    "primary port" connections.

    A typical value is 3301. The listen parameter may also be set for local hot
    standby.

    .. NOTE::

        A replica also binds to this port, and accepts connections, but these
        connections can only serve reads until the replica becomes a master.

    | Type: integer or string
    | Default: null
    | Dynamic: yes

.. _cfg_basic-pid_file:

.. confval:: pid_file

    Store the process id in this file. Can be relative to :ref:`work_dir
    <cfg_basic-work_dir>`. A typical value is “:file:`tarantool.pid`”.

    | Type: string
    | Default: null
    | Dynamic: no

.. _cfg_basic-read_only:

.. confval:: read_only

    Put the server in read-only mode. After this, any requests that try to
    change data will fail with error :errcode:`ER_READONLY`.

    | Type: boolean
    | Default: false
    | Dynamic: yes

.. _cfg_basic-snap_dir:

.. confval:: snap_dir

    A directory where snapshot (.snap) files will be stored. Can be relative to
    :ref:`work_dir <cfg_basic-work_dir>`. If not specified, defaults to
    ``work_dir``. See also :ref:`wal_dir <cfg_basic-wal_dir>`.

    | Type: string
    | Default: "."
    | Dynamic: no

.. _cfg_basic-vinyl_dir:

.. confval:: vinyl_dir

    A directory where vinyl files or subdirectories will be stored. Can be
    relative to :ref:`work_dir <cfg_basic-work_dir>`. If not specified, defaults
    to ``work_dir``.

    | Type: string
    | Default: "."
    | Dynamic: no

.. _cfg_basic-username:

.. confval:: username

    UNIX user name to switch to after start.

    | Type: string
    | Default: null
    | Dynamic: no

.. _cfg_basic-wal_dir:

.. confval:: wal_dir

    A directory where write-ahead log (.xlog) files are stored. Can be relative
    to :ref:`work_dir <cfg_basic-work_dir>`. Sometimes ``wal_dir`` and
    :ref:`snap_dir <cfg_basic-snap_dir>` are specified with different values, so
    that write-ahead log files and snapshot files can be stored on different
    disks. If not specified, defaults to ``work_dir``.

    | Type: string
    | Default: "."
    | Dynamic: no

.. _cfg_basic-work_dir:

.. confval:: work_dir

    | A directory where database working files will be stored. The server
      switches to work_dir with :manpage:`chdir(2)` after start. Can be
      relative to the current directory. If not specified, defaults to
      the current directory. Other directory parameters may be relative to
      ``work_dir``, for example:
    | ``box.cfg{ work_dir = '/home/user/A', wal_dir = 'B', snap_dir = 'C' }``
    | will put xlog files in /home/user/A/B, snapshot files in /home/user/A/C,
      and all other files or subdirectories in /home/user/A.

    | Type: string
    | Default: null
    | Dynamic: no
