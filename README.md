# ip_lockout

Script for detecting break-in attempts and automatic blocking

NISHI, Takao <zophos@koka-in.org>

https://github.com/zophos/ip_lockout

## Features

 * Detecting break-in attempts and automatic blocking associate with iptables
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
  * LICENSE
  * ip_lockout : executable script
  * ip_lockout.rc : sample configuration file


## Installation

 1. edit cofig file "ip_lockout.rc" and copy it to /usr/local/etc or /etc
 2. create a directory for lockout.db where you defined in config file
(default: /var/lib/ip_lockout)
 3. test config ` # ./ip_lockout --dry-run`
 4. register to system or root crontab; eg)

`*/2 * * * * /usr/local/sbin/ip_lockout  # run every 2 min with standalone mode`


## Running

### Usage

    ip_lockout [options]
    
    options:
    -c [file], --config-file=[file]:
            run with specified config file
    -F, --filter-mode: read data from STDIN and write results to STDOUT
    -S, --show-status: show current status
    
    -d, --dry-run: don't update db and iptables entries
    
    --ignore-db-entry: don't read existing lockout.db
    --ignore-iptables-entry: don't read existing iptables entries
    
    --now="timestamp": force set now as given timestamp
    
    -h, --help: show this message and quit

### Standalone mode

If NOT `--filter-mode` nor `--show-status` option is given,
ip_lockout runs as standalone mode.
This mode reads existing iptables entries, inspects log files,
and adds/deletes iptables entries.

Default log files to inspect are

 * /var/log/auth.log for sshd
 * /var/log/mail.log for postfix SASL auth and imapd (dovecot)

Those can be customized in the configuration file.

Standalone mode requires root privilege to read system logs and operate
iptables.


### Filter mode

The filter mode that runs with `-F` or `--filter-mode` option,
reads all data from STDIN, and writes results to STDOUT.
This mode does not require root privilege.


    $ (sudo /sbin/iptables -L INPUT -n) |\
      cat - /var/log/auth.log |\
      ip_lockout -F | (want to exec cmd ...)


Default output format is `[updated] OP ADDR status remain/max_count last`.

Where:
    
    updated:   Timestamp of last DB entry updated
    OP:        'I' (start blocking) or 'D' (stop blocking)
    ADDR:      host or subnet IP address
    status:    host/subnet status in DB. 'blocked' or 'expired'
    remain:    count number until expire
    max_count: count number at blocking start
    last:      timestamp of last attempt detected

You can customize the output format.
See [ip_lockout.rc](./ip_lockout.rc) for details.


### Description mode

In case of `--show-status` option is given, ip_lockout runs
as description mode.

This mode shows current DB status, summary of logs and schedules of
trig up/down iptables entry.
Any iptables/DB entries are NOT updated.

### Command name short-cut

ip_lockout changes it's behavior by command name.

 * filter-ip_lockout: always runs with --filter-mode option
 * show-ip_lockout: always runs with --show-status option

### Customize

TBW

See [ip_lockout.rc](./ip_lockout.rc) comments.


## License
Copyright (c) 2017, NISHI, Takao <zophos@koka-in.org>
All rights reserved.

THIS SOFTWARE COMES WITH ABSOLUTELY NO WARRANTY.

You can redistribute it and/or modify it under either the terms of the
2-clause BSDL (see the file [LICENSE](LICENSE) for details).

Comments, patches, and glasses/bottles/barrels of beer are welcome :)
