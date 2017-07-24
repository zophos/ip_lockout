# ip_lockout
Script for detecting break-in attemption and automatic blocking

NISHI, Takao <zophos@koka-in.org>

## Features

 * Detecting break-in attemption and automatic blocking associate with iptables
 * Supports multiple log files and protocols
 * Distributed hosts can manage as aggregated subnets
 * No any gems is required
 * Supports stand-alone mode and filter mode

## Requirements

 * iptables
 * Ruby 1.9.3 or higher

**The stand alone mode of this script requires root privilege to read system logs and operate iptables**

## Installation

 1. edit cofig file "ip_lockout.rc" and copy it to /usr/local/etc or /etc
 2. create directory for lockout.db where you defined in config file (defualt: /var/lib/ip_lockout)
 3. test config ` # ip_lockout --dry-run`
 4. registrate to system or root crontab; eg)


    */2 * * * * /usr/local/sbin/ip_lockout

### Filter mode

The filter mode reads all data from STDIN, and writes results to STDOUT.
This mode does not requires root privilege.


    $ (sudo /sbin/iptables -L INPUT -n) |\
      cat - /var/log/auth.log | \
      ip_lockout -F | (want to exec cmd ...)


Output format are:

    OP ADDR status remain/max_count last update
    
      OP:        'I' (start blocking) or 'D' (stop blocking)
      ADDR:      host or subnet IP address
      status:    host/subnet status in DB. 'blocked' or 'expired'
      remain:    count number until expire
      max_count: count number at blocking start
      last:      timestamp of last attemption detected
      update:    timestamp of  DB entry last updated



### Usage

    -c [file], --config-file=[file]:
          run with specified config file
    -d, --dry-run: dry run (not operate iptables and db)
    -F, --filter-mode: read data from STDIN and write results to STDOUT
    
    --ignore-db-entry: don't read existing lockout.db
    --ignore-iptables-entry: don't read existing iptables entries
    
    --now="timestamp": force set now as gaven timestamp

    -h, --help: show this message and quit

### Customize

TBW

See ip_lockout.rc comments.

## Files

  * README.md
  * LICENCE
  * ip_lockout : executable script
  * ip_lockout.rc : sample configuration file

## Licece
Copyright (c) 2017, NISHI Takao <zophos@koka-in.org>
All rights reserved.

THIS SOFTWARE COMES WITH ABSOLUTELY NO WARRANTY.

You can redistribute it and/or modify it under either the terms of the
2-clause BSDL (see the file LICENCE for details).

Comments, patches, and glasses/bottles/barrels of beer are welcome :)
