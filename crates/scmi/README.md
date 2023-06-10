# vhost-device-scmi

This program is a vhost-user backend for a VirtIO SCMI device.
It provides SCMI access to various entities on the host; not
necessarily only those providing an SCMI interface themselves.

It is tested with QEMU's `-device vhost-user-scmi-pci` but should work
with any virtual machine monitor (VMM) that supports vhost-user. See
the Examples section below.

The currently supported SCMI protocols are:

- base protocol

The currently supported SCMI connections on the host are:

- none

## Synopsis

**vhost-device-scmi** [*OPTIONS*]

## Options

.. program:: vhost-device-scmi

.. option:: -h, --help

  Print help.

.. option:: -s, --socket-path=PATH

  Location of the vhost-user Unix domain sockets.

You can set `RUST_LOG` environment variable to `debug` to get maximum
messages on the standard error output.

## Examples

The daemon should be started first:

::

  host# vhost-device-scmi --socket-path=scmi.sock

The QEMU invocation needs to create a chardev socket the device can
use to communicate as well as share the guests memory over a memfd:

::

  host# qemu-system \
      -chardev socket,path=scmi.sock,id=scmi \
      -device vhost-user-scmi-pci,chardev=vscmi,id=scmi \
      -machine YOUR-MACHINE-OPTIONS,memory-backend=mem \
      -m 4096 \
      -object memory-backend-file,id=mem,size=4G,mem-path=/dev/shm,share=on \
      ...

## License

This project is licensed under either of

- [Apache License](http://www.apache.org/licenses/LICENSE-2.0), Version 2.0
- [BSD-3-Clause License](https://opensource.org/licenses/BSD-3-Clause)