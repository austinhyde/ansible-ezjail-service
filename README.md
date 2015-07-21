# ansible-ezjail-service

This is a simple Ansible module for controlling [ezjail](https://www.freebsd.org/doc/handbook/jails-ezjail.html)-managed FreeBSD jails similarly to the built-in [service](https://docs.ansible.com/ansible/service_module.html) module.

This is an alternative to using `service: name=ezjail args=my-jail`, providing:

* The ability to start/stop/restart/enable/disable multiple jails at once
* Error messages pertaining to specific jails

## Dependencies (Managed Node)

* FreeBSD
* [sysutils/ezjail](https://www.freshports.org/sysutils/ezjail/)
* existing **ezjail-managed** jails
  * *this module does not create jails, and does not work with non-ezjail jails*

## Installation

Copy or link the `ezjail_service` file into your global Ansible library (usually `/usr/share/ansible`) or into the `./library` folder alongside your top-level playbook, or anywhere in your configured module load path.

## Usage

Similar to the built-in [service](https://docs.ansible.com/ansible/service_module.html) module:

### Options

* `name` - required, the names (comma separated) of the jails to operate on **as referenced by ezjail**
  * this is what you gave `ezjail-admin create` unless you renamed it.
* `state` - optional, may be "started", "stopped", or "restarted", with the same semantics as the `service` module.
* `enabled` - optional boolean, defines whether the jail is started on boot or not. Note that ezjail jails are enabled by default after creation if the ezjail service itself is enabled.

You must specify at least one of `state` and `enabled`.

### Examples

```yaml
- ezjail_service: name=my-jail state=started
- ezjail_service: name=defunct-jail1,defunct-jail2 state=stopped enabled=no
- ezjail_service: name=reconfigured-jail state=restarted
```

## Todo

* Add inline documentation
* Fix support for jails with dots in them and subjails, along the lines of [austinhyde/ansible-packer PR#2](https://github.com/austinhyde/ansible-sshjail/pull/2)
  * This gets tricky though because sshjail only operates on live jails...

Have other ideas? Better way of doing something? Doesn't work for you? Open an issue or a pull request!
