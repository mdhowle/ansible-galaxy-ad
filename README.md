# Ansible Collection - mdhowle.ad

Active Directory dynamic inventory plugin for Ansible

## Requirements

  * ansible
  * [ldap3](https://github.com/cannatag/ldap3)
  * Optional: [dnspython](https://github.com/rthalley/dnspython) for automatic server lookup
  * Optional: [gssapi](https://github.com/pythongssapi/python-gssapi) for Kerberos authentication


## Installation
You can install this collection with the Ansible Galaxy CLI:

```
ansible-galaxy collection install mdhowle.ad
```

You can also install it by creating a requirements.yml file with the contents below and install it with `ansible-galaxy collection install -r requirements.yml`

```
---
collections:
  - name: mdhowle.ad
```

## Usage

```yaml
plugin: mdhowle.ad.ad_inventory
server: dc.example.com
port: 636
base: DC=example,DC=com
username: EXAMPLE\ExampleUser  # or distinguishedname
password: hunter2
filter: "(operatingSystem=Debian GNU/Linux)"
ansible group: Debian
```

Run `ansible-playbook -i ad.yml playbook.yml`


**NOTE**: Quotations are not required for the values. If you do quote the values, you will need to escape backslashes (e.g. `username: "EXAMPLE\\ExampleUser"`).


## Configuration
| Attribute | Type | Required | Choices/Default | Description |
|--|--|--|--|--|
| plugin | `str`| Yes | `mdhowle.ad.ad_inventory` |  Marks this as an instance of the 'ad\_inventory' plugin |
| server | `str` | Yes[^1] | `null` | Active Directory server name or list of server names |
| port | `int` | No | `389` | LDAP Server Port; using port 636 enables SSL |
| ssl | `bool` | No | False | Connect to server with SSL |
| starttls | `bool` | No | True | Connect to server with STARTTLS |
| base | `str` | No | `null` | Starting distinguished name for the search. If unspecified, the default naming context will be used. |
| filter | `str` | No | `''` | LDAP query filter. Note `objectClass=computer` is automatically appended. |
| scope | `str` | No | choices: `['base', 'level', subtree']`; default: `subtree` | Scope of the search |
| hostname var | `str` | No | `name` | LDAP attribute to use as the inventory hostname |
| username | `str` | Yes | `null` | Username to bind as. It can the distinguished name of the user, or SHORTDOMAIN\user.  If unspecified, Kerberos authentication will be used.
| password | `str` | No | `null` | Username's password. Must be defined if username is also defined. |
| ansible group | `str` | No | N/A | Ansible group name to assign hosts to |
| var attribute | `str` | No | `null` | LDAP attribute to load as YAML for host-specific Ansible variables. |


[^1]: If the dnspython package is installed and the server is not specified, DNS will be queried for the Active Directory server.
