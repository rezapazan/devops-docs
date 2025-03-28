# `apt` Command

All of `apt` configs are located in `/etc/apt`.

## Add Proxy

In `/etc/apt/apt.conf.d` create a file, named `01proxy`. Use the following:

```config
Acquire::http::Proxy "http://[address]:[port]";
Acquire::https::Proxy "http://[address]:[port]";
```

## List GPG Keys

All keys are stored in `/etc/apt/trusted.gpg.d`.

## Repositories

All repos are located in `/etc/apt/sources.list.d`.
