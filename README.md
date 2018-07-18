# napalm-edgeos

Community NAPALM driver for EdgeOS on Ubiquiti Edgerouters

Forked from the [Community NAPALM driver for the VyOS](https://github.com/napalm-automation-community/napalm-vyos)

General support matrix
----------------------


 |                    | EdgeOS         |
 |--------------------|----------------|
 |**Module Name**     |  napalm-edgeos |
 |**Driver Name**     |  edgeos        |
 |**Structured data** |  Yes           |
 |**Minimum version** |  1.1.6         |
 |**Backend library** |  netmiko       |



Configuration support matrix
----------------------------

|                     |  EdgeOS |
| ------------------- | ------- |
| **Config. replace** |  Yes    |
| **Config. merge**   |  Yes    |
|**Compare config**   |  Yes    |
| **Atomic Changes**  |  Yes    |
| **Rollback**        |  Yes    |



Optional arguments
------------------

NAPALM supports passing certain optional arguments to some drivers. To do that you have to pass a dictionary via the
:code:`optional_args` parameter when creating the object::

    >>> from napalm import get_network_driver
    >>> driver = get_network_driver('edgeos')
    >>> optional_args = {'my_optional_arg1': 'my_value1', 'my_optional_arg2': 'my_value2'}
    >>> device = driver('192.168.76.10', 'vagrant', 'this_is_not_a_secure_password', optional_args=optional_args)
    >>> device.open()

List of supported optional arguments
____________________________________

* :code:`port` (edgeos) - Allows you to specify a port other than the default.
* :code:`key_file` (edgeos) - Netmiko/Paramiko argument, path to a private key file (default: 'False').



Prerequisites
-------------


EdgeOS has no native HTTP API or NETCONF capability.
We are using Netmiko for ssh connections and scp file transfers.
Having Netmiko installed in your working box is a prerequisite.

EdgeOS in version 1.10.x (tested 1.10.3)

napalm==2.*
paramiko
netmiko>=1.1.0
vyattaconfparser



Configuration file
------------------

Currently EdgeOS driver supports two different configuration formats:
* load_replace_candidate - Full config file (with brackets) like in /config/config.boot
Due to the OS nature,  we do not support a replace using
a set-style configuration format.
* load_merge_candidate - Currently driver supports set-style configuration format.
Example

`set system login banner pre-login "test"`

EdgeOS require configuration file (load_replace) to contain comment like following

`/* Warning: Do not remove the following line. */
/* === vyatta-config-version: "cluster@1:config-management@1:conntrack-sync@1:conntrack@1:cron@1:dhcp-relay@1:dhcp-server@4:firewall@5:ipsec@4:nat@4:qos@1:quagga@2:system@6:vrrp@1:wanloadbalance@3:webgui@1:webproxy@1:zone-policy@1" === */
/* Release version: VyOS 1.1.7 */`

Otherwise EdgeOS reject `load` operation

Notes
------------------
* The NAPALM-edgeos driver supports all Netmiko arguments as either standard arguments (hostname, username, password, timeout) or as optional_args (everything else).

* The NAPALM-edgeos driver supports authentication with ssh key. Please specify path to a key in optional_args
`'optional_args': {'key_file': '/home/user/ssh_private_key'}`

Configuration Examples
----------------------
* Vyos as IPSec VPN endpoint and BGP router -   https://github.com/DreamLab/ansible-vyos
* CI Demo for vyos+junos (Polish only) https://github.com/ppieprzycki/plnog2016

VyOS Community
------------------
Slack Channel https://slack.vyos.io