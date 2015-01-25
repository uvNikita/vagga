.. _commands:

========
Commands
========


Command parameters:

``container``
    The container to run command in

``command``
    The command to run. It's either a string (which is treated as executable)
    or a list or command and arguments. If ``wrapper-script`` is defined in
    container, it prefixes this command.

``run``
    The command to run using a shell. Prefixed by shell defined in container
    (usually ``/bin/sh -c``)

``supervise``, ``supervise-mode``
    The set of processes to supervise and mode. See :ref:`supervision` for more
    info

``accepts-arguments``
    Denotes whether command accepts additional arguments. Defaults to ``false``
    for shell commands, and ``true`` for regular commands.

``environ``
    The mapping of environment to pass to command. This overrides environment
    specified in container on value by value basis.

``inherit-environ``
    The list of variables that will be inherited from user environment, when
    running a command. These variables override both ``environ`` in command
    and container's environment only if is set in user environment (including
    set to empty string). Inheriting variables is in generally discouraged
    because this makes reproducing environment harder.

``description``
    Description that is printed in when vagga is runned without arguments

``banner``
    The message that is printed before running process(es). Useful for
    documenting command behavior.

``banner-delay``
    The seconds to sleep before printing banner. For example if commands run
    a web service, banner may provide a URL for accessing the service. The
    delay is used so that banner is printed after service startup messages not
    before.  Note that currently vagga sleeps this amount of seconds even
    if service is failed immediately.

``epilog``
    The message printed after command is run. It's printed only if command
    returned zero exit status. Useful to print further instructions, e.g. to
    display names of build artifacts produced by command.

``pid1mode``
    This denotes what is run as pid 1 in container. It may be ``wait``,
    ``wait-all-children`` or ``exec``. The default ``wait`` is ok for most
    regular processes. See :ref:`pid1mode` for more info.

``write-mode``
    The parameter specifies how container's base file system is used. By
    default container is immutable (corresponds to the ``read-only`` value of
    the parameter), which means you can only write to the ``/tmp`` or
    to the ``/work`` (which is your project directory).

    Another option is ``transient-hard-link-copy``, which means that whenever
    command is run, create a copy of the container, consisting of hard-links to
    the original files, and remove the container after running command. Should
    be used with care as hard-linking doesn't prevent original files to be
    modified. Still very useful to try package installation in the system. Use
    ``vagga _build --force container_name`` to fix base container if that was
    modified.