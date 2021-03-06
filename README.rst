LIOX, the Linux distribution for LIO2014 and BOI2014
----------------------------------------------------

Linux distribution for Lithuania Informatics Olympiad 2014 (used: 0.0.5) and
Baltics Informatics Olympiad 2014.

Latest ISOs
-----------

For your convenience pre-built images are available. You may find them here:
http://iso.lmio.lt/. Always pick the latest version.

Building the ISO
----------------

If you want to modify the configuration and build the ISO yourself, this
section is for you. To generate the ``liox-VERSION.iso``, type::

    $ make

You will need root for ``live build``. Note: this will take a while and download
>800MB from the internet.

Run it::

    $ kvm -boot d -cdrom liox-*.iso

You can also run it via qemu, VirtualBox, VMWare, Xen, etc. Just run it as a
bootable CD.

To generate a virtual machine image out of the ISO, run::

    $ make vm

Alternatively, you can select "fully automated installation" in the menu if you
are happy with the defaults (uses dangerous preseed script, be careful), or
"install" for standard debian installation.

Special kernel parameters
-------------------------

A number of parameters can be passed on the kernel command line to facilitate
booting from loop-mounted drives. ``looproot`` and ``loopsrc`` are mandatory
if you wish to perform such a boot. If a loop-mounted boot is performed,
``root=/dev/mapper/loop0p1`` should also be specified.

- ``looproot=`` specifies the block device which contains the drive image, this device
  will be mounted
- ``loopsrc=`` specifies a path on the looproot to the loop drive image, it should be
  a file ending in ``.raw``
- ``loopget=`` specifies a URL which will be fetched to the looproot. It must be a file
  ending in ``.raw.xz``, ``.raw.gz`` or just ``.raw``. If the file is compressed, it is
  decompressed into the looproot and the source file is kept on-disk. If ``loopsrc``
  already exists, then ``loopget`` is ignored.
- ``lioxwipe=`` cleans up files on the looproot before performing any further operations.
  Possible values are:
    - 1 - deletes the pattern ``liox-*.raw``
    - 2 - deletes the pattern ``liox-*.raw*``
    - 3 - deletes the pattern ``liox-*.raw*`` and drops to a shell instead of continuing boot

A ``.raw`` file is a complete drive image with partition tables.

Contributing
------------

Please test and contribute by opening the pull request. General
rules/guidelines:

1. Everything that is supposed to be generated should be generated.
2. Keep 80 character limit in non-generated files (READMEs, Makefiles, scripts).
3. Keep it all English.
4. Write `good commit messages`_.

.. _`good commit messages`: https://github.com/erlang/otp/wiki/Writing-good-commit-messages


Altering the VM
---------------

It is possible to alter the virtual machine image without starting it. For example,
to delete the default user::

    $ sudo ./changeimg setup NewVirtualDisk1.vdi
    Setup successful
    $ sudo ./changeimg shell  # gives chroot'ed shell
    # deluser user
    ...
    # adduser lioadmin
    ...
    # exit
    $ sudo ./changeimg cleanup
    Cleanup successful

You can also easily run executables in the chroot using the same script. For
more features, see help::

    $ ./changeimg --help

For organizers: actual production scripts are in internal SVN under folder
``TC``.

Changelog
---------

* 0.3.0 - change to Jessie base image
* 0.1.2 - add geany-plugins (for debugger support in geany IDE).
* 0.1.1 - switch back to sysvinit due to systemd incompleteness (see git log).
  Testable version.
* 0.1.0 - everything translated to english. Moved to systemd init.
* 0.0.5 - lithuanian keymap. Used in LIO2014.
* 0.0.4 - add kdevelop4, Xorg drivers, management scripts.
* 0.0.3 - change to SMP kernel (requires PAE), enable networking in preseed.
* 0.0.2 - first usable version.
