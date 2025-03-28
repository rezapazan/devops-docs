# SSH (Secure Shell) Service

In `/etc/ssh/sshd_config`, we can:

- Config users & user groups that can use SSH with a machine.
- Change default port of SSH.
- Define IP addresses allowed to use SSH.
- Prohibit password login.

## Sockets

- TCP sockets: HTTP, HTTPS, FTP
- UDP: DNS, NTP
- Unix: MySQL

## Commands

```bash
$ ssh-keygen
# for generating ssh key
$ scp
$ rsync
# copy over SSH
$ sshd -t
# verifies the SSH config file
$ systemctl status sshd
```
