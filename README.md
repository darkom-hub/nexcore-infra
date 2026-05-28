# NexCore Infrastructure Automation Project

## Overview

This project implements a fully automated enterprise infrastructure deployment using Ansible.

The environment was designed according to the provided business requirements and includes:

* Internal DNS infrastructure
* Apache web hosting
* WordPress deployment
* MediaWiki deployment
* MariaDB database services
* Shared NFS storage
* AutoFS automount configuration
* Centralized logging
* Enterprise email infrastructure
* Automated maintenance and patching
* Automated database backups
* SELinux and firewalld security hardening

The entire infrastructure is deployed through a single idempotent `site.yml` playbook.

---

## Infrastructure Layout

| Hostname    | Purpose                                                                  |
| ----------- | ------------------------------------------------------------------------ |
| servera     | DNS, Apache, WordPress, MediaWiki, Postfix, Dovecot, centralized logging |
| serverb     | MariaDB, NFS storage, database backups                                   |
| workstation | Ansible control node                                                     |

---

## Implemented Roles

| Role      | Purpose                                         |
| --------- | ----------------------------------------------- |
| users     | User provisioning, SSH keys, sudo configuration |
| security  | SELinux validation, SSH hardening, firewalld    |
| storage   | LVM provisioning and shared filesystem          |
| automount | NFS exports and AutoFS configuration            |
| database  | MariaDB installation and database provisioning  |
| dns       | Internal BIND DNS infrastructure                |
| web       | Apache virtual hosts and application deployment |
| email     | Postfix and Dovecot IMAP configuration          |
| logging   | Centralized rsyslog configuration               |
| patching  | Automated maintenance, cron jobs, logrotate     |
| backup    | Automated nightly database backups              |

---

## Deployment

Run the complete infrastructure deployment:

```bash
ansible-playbook site.yml
```

Run individual roles using tags:

```bash
ansible-playbook site.yml --tags dns
ansible-playbook site.yml --tags web
ansible-playbook site.yml --tags database
```

---

## Security Features

* SELinux enforced on all managed nodes
* SSH password authentication disabled
* Root SSH login disabled
* firewalld enabled on all systems
* Least-privilege firewall rules
* SELinux web context management

---

## Centralized Logging

* serverb forwards logs to servera using rsyslog over TCP port 514
* Logs stored at:

```bash
/var/log/remote/serverb.log
```

---

## Automated Maintenance

Implemented scheduled maintenance tasks:

* Daily health checks
* Weekly package cleanup
* Automated package updates
* Automatic reboot after patching
* Daily log rotation with compression

---

## Bonus Features

### Automated Database Backups

Nightly MariaDB backups are automatically stored in:

```bash
/var/backups/nexcore/
```

using date-stamped directories and filenames.

### Dovecot IMAP Support

Dovecot IMAP was integrated with Postfix to allow programmatic mailbox access through Maildir.

---

## Validation

The infrastructure was validated using:

* Full idempotent playbook execution
* DNS resolution tests
* Apache virtual host testing
* Centralized logging verification
* Email delivery testing
* Database backup validation
* IMAP service validation

---

## Technologies Used

* Ansible
* RHEL / Rocky Linux
* Apache HTTP Server
* MariaDB
* BIND DNS
* Postfix
* Dovecot
* rsyslog
* NFS / AutoFS
* LVM
* SELinux
* firewalld
