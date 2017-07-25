# ip_lockout

Script for detecting break-in attemption and automatic blocking

NISHI, Takao <zophos@koka-in.org>

https://github.com/zophos/ip_lockout

## Features

 * Detecting break-in attemption and automatic blocking associate with iptables
 * Supports multiple log files and protocols
 * Distributed hosts can manage as aggregated subnets
 * Supports standalone and filter modes
 * No any extra gems is required

## Requirements

 * Ruby 1.9.3 or higher
 * iptables (on standalone mode) 

**Standalone mode of this script requires root privilege to read
system logs and operate iptables.**


## Files

  * README.md
  * LICENCE
  * ip_lockout : executable script
  * ip_lockout.rc : sample configuration file


## Installation

 1. edit cofig file "ip_lockout.rc" and copy it to /usr/local/etc or /etc
 2. create a directory for lockout.db where you defined in config file
(defualt: /var/lib/ip_lockout)
 3. test config ` # ./ip_lockout --dry-run`
 4. registrate to system or root crontab; eg)

`*/2 * * * * /usr/local/sbin/ip_lockout  # run every 2 min with standalone mode`


## Running

### Usage

    -c [file], --config-file=[file]:
            run with specified config file
    -F, --filter-mode: read data from STDIN and write results to STDOUT
    -S, --show-status: Show current status
    
    -d, --dry-run: don't update db and iptables entry
    
    --ignore-db-entry: don't read existing lockout.db
    --ignore-iptables-entry: don't read existing iptables entries
    
    --now="timestamp": force set now as gaven timestamp
    
    -h, --help: show this message and quit

### Standalone mode

If `--filter-mode` nor `--show-status` option gaven, ip_lockout runs
as standalone mode.
This mode read existing iptables entries, inspect log files, and
add/delete iptables entries.

Default log files to inspect are

 * /var/log/auth.log for sshd
 * /var/log/mail.log for postfix SASL auth and imapd (dovecot)

Those can be customized in confiuration file.

Standalone mode requires root privilege to read system logs and operate
iptables.


### Filter mode

The filter mode that run with `-F` or `--filter-mode` option, reads
all data from STDIN, and writes results to STDOUT.
This mode does not require root privilege.


    $ (sudo /sbin/iptables -L INPUT -n) |\
      cat - /var/log/auth.log |\
      ip_lockout -F | (want to exec cmd ...)


Default output format are

    updated OP ADDR status remain/max_count last
    
      updated:   Timestamp of last DB entry updated
      OP:        'I' (start blocking) or 'D' (stop blocking)
      ADDR:      host or subnet IP address
      status:    host/subnet status in DB. 'blocked' or 'expired'
      remain:    count number until expire
      max_count: count number at blocking start
      last:      timestamp of last attemption detected

You can customize the output format.
See [ip_lockout.rc](./ip_lockout.rc) for details.


### Description mode

In case of `--show-status` option gaven, ip_lockout runs
as description mode.

This mode shows current DB status, summary of logs and schedules of
trig up/down iptables entry.
Any iptables/DB entries were NOT updated.


### Customize

TBW

See [ip_lockout.rc](./ip_lockout.rc) comments.


## Licece
Copyright (c) 2017, NISHI Takao <zophos@koka-in.org>
All rights reserved.

THIS SOFTWARE COMES WITH ABSOLUTELY NO WARRANTY.

You can redistribute it and/or modify it under either the terms of the
2-clause BSDL (see the file [LICENCE](LICENCE) for details).

Comments, patches, and glasses/bottles/barrels of beer are welcome :)
