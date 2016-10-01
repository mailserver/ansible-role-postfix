Ansible Role: Postfix
=====================

[Postfix](https://www.postfix.org/) mail transfer agent (MTA)

This role is part of the [Mailserver](https://github.com/mailserver) project.

MySQL Schema
------------

- ToDo

Examples
--------

### Postfix MTA with MySQL backend and multiple milters

```yaml
- role: postfix
  postfix_hostname: "mail.example.com"
  postfix_config:
    local_catchall_alias: "postmaster@mail.example.com"
    ssl:
      key: "/etc/letsencrypt/live/mail.example.com/privkey.pem"
      certificate: "/etc/letsencrypt/live/mail.example.com/fullchain.pem"
      dhparams:
        size: 2048
    daemon_user: "postfix"
    milter_group: "milter" 
    milters:
      submission:
        - name: opendkim
          socket: "milters/opendkim.sock"
        - name: clamav
          socket: "milters/clamav-milter.ctl"
      incoming_recipient_restriction:
        - name: sqlgrey
          socket: "milters/sqlgrey.sock"
      incoming_smtpd:
        - name: spamassassin
          socket: "milters/spamass.sock"
        - name: clamav
          socket: "milters/clamav-milter.ctl"
    lmtp_socket: 
  postfix_mysql:
    host: "127.0.0.1"
    port: 3306
    user: "postfix"
    password: "correct horse battery staple"
    database: "maildb"
```
