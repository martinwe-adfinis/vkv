vkv
===

`vkv` is a tool for easily fetching secrets from a HashiCorp Vault (or
compatible) key-value (KV) store. It is primarily targeted at users of `pass(1)`
and similar tools.

`vkvmenu` is a dmenu wrapper for more interactively selecting (and optionally
auto-typing) a secret, similar to `passmenu`.


Dependencies
------------

 * POSIX-compliant `/bin/sh`
 * POSIX-compliant `cat`, `printf`, `test`/`[` and `true`/`false`
 * `getent` from GNU libc
 * `id` from GNU coreutils
 * HashiCorp Vault (or compatible drop-in replacement, e.g.
   [OpenBao](https://github.com/openbao/openbao)
 * `notify-send` from libnotify
 * `jq`

Further dependencies for optional functionality:

 * `xclip` (for `vkv show -c`)

Further dependencies for `vkvmenu`:

 * `dmenu`
 * `xclip` (for `vkvmenu` without `-t`)
 * `xdotool` (for `vkvmenu` with `-t`)


Installation
------------

`vkv` is a single POSIX sh script that can be installed to some place in `$PATH`
as-is (or simply run from this project directory).

`vkvmenu` is a single POSIX sh script that can be installed to some place in
`$PATH` as-is (or simply run from this project directory). However, it expects
`vkv` to be present in `$PATH`.

The manpages are typeset in Groff and can be installed to the system as-is (or
simply be read in this project directory with `man ./vkv.1` and `man
./vkvmenu.1`, respectively).


Usage
-----

Set `VAULT_ADDR` and other environment as required for accessing your HashiCorp
Vault instance.

    $ export VAULT_ADDR=https://vault.example.net

Get the `password` secret contained in the `mykv` KV instance at the path
`service/web.example.net/admin`:

    $ vkv show mykv/service/web.admin.net/admin password
    hunter2

Instead of writing it out to stdout, you can also copy it to the clipboard:

    $ vkv show -c mykv/service/web.admin.net/admin password

Verify that the username is indeed as expected:

    $ vkv show mykv/service/web.admin.net/admin password username=webmaster
    hunter2

vs.

    $ vkv show mykv/service/web.admin.net/admin password username=blabla
    KV secret contained differing value for key 'username'

List "bookmarked" secrets (used by `vkvmenu` to provide suggestions):

    $ vkv list
    mykv/service/web.admin.net/admin password username=webmaster
    mykv/service/db.admin.net/admin password username=dbmaster
    mykv/system/webbox01/root password username=root


Disclaimer
----------

vkv is not related to HashiCorp or to any of its products, both Vault and
otherwise.
