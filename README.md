# Ansible Groupware Server Role

An [Ansible][1] role that manages a mail, contacts and calendar server.

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
    - [Variables](#variables)
    - [Example](#example)
    - [Notes](#notes)

## Features

- Sets up a full-featured mail server using [iRedMail][2].
- Sets up a CalDAV/CardDAV server.
- Manages groupware accounts.

## Requirements

- Python Passlib on control host
- At least 4GB RAM[¹][3] on managed host
- Debian GNU/Linux 10 (Buster) on managed host
- TLS certificate

## Installation

Use [Ansible Galaxy][4] to install `tobygiacometti.groupware_server`. Check out the Ansible Galaxy [content installation instructions][5] if you need help.

## Usage

To get general guidance on how to use Ansible roles, visit the [official documentation][6].

### Variables

- `groupware_server_weekly_reload`: Boolean indicating whether the groupware server configuration should be reloaded once a week. This is especially useful if TLS certificates are being managed automatically using a service like Let's Encrypt. The reload is executed gracefully to not impact any clients. Defaults to `true`.
- `groupware_server_tls_cert_file`: Path to the TLS certificate that is used to encrypt connections between clients and groupware services.
- `groupware_server_tls_key_file`: Path to the TLS certificate private key. Please note: The CalDAV/CardDAV server is started directly as a non-root user. This role will create ACL entries that give the CalDAV/CardDAV server user permission to read the private key file and list files in the containing directories.
- `groupware_server_accounts`: Groupware accounts that should be created and managed. This variable takes a list of dictionaries with following key-value pairs:
    - `email`: Email address that is assigned to the account.
    - `password`: Password for the account.
    - `email_aliases`: List of [email aliases][7] for the account.

### Example

```yaml
- hosts: groupware.domain.example
  vars_prompt:
    - name: example_account_password
      prompt: Password for example account
      unsafe: true
  roles:
    - role: tobygiacometti.groupware_server
      vars:
        groupware_server_tls_cert_file: /etc/certs/groupware.domain.example/cert
        groupware_server_tls_key_file: /etc/certs/groupware.domain.example/key
        groupware_server_accounts:
          - email: user@domain.example
            password: "{{ example_account_password }}"
            email_aliases:
              - postmaster@domain.example
              - abuse@domain.example
```

### Notes

- This role currently focuses on managing a simple setup for a personal groupware server. More advanced functionality (self-service for users, webmail, etc.) might get implemented in the future.
- Since this role sets up a MySQL server, please make sure that no MySQL server is already running on the managed host.
- Consult the [iRedMail DNS guide][8] to get help with setting up the required DNS records.
- Emails are stored in the directory `/var/vmail/vmail1`. Calendar and contacts data is stored in the directory `/var/lib/radicale`. In case you want to back up these directories, following files can be excluded: `.Radicale.cache`, `.Radicale.lock` and `.Radicale.tmp-*`.
- To access groupware accounts, use the email address as the username.
- Consult the [Radicale documentation][9] to get help with setting up the CalDAV/CardDAV clients.

---

<a name="ansible-groupware-server-role-footnotes"></a>

1. Mostly used for the virus scanner. To cut costs for a low-traffic personal server, you can get away with using swap to cover a big part of the memory needs.

[1]: https://www.ansible.com
[2]: https://www.iredmail.org
[3]: #ansible-groupware-server-role-footnotes
[4]: https://galaxy.ansible.com
[5]: https://galaxy.ansible.com/docs/using/installing.html
[6]: https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html
[7]: https://en.wikipedia.org/wiki/Email_alias
[8]: https://docs.iredmail.org/setup.dns.html
[9]: https://radicale.org/2.1.html#documentation/clients
