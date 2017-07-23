# ip_lockout
Script for detecting break-in attemption and automatic blocking

NISHI, Takao <zophos@koka-in.org>

## Features

 * Detecting break-in attemption and automatic blocking associate with iptables 
 * Distributed hosts can manage as aggregated subnets.
 * No any gems is required

## Requirements

 * iptables
 * Ruby 1.9.3 or higher

**This script requires root privilege to read system logs and operate iptables**

## Installation

 1. edit cofig file "ip_lockout.rc" and copy it to /usr/local/etc or /etc
 2. create directory for lockout.db where you defined in config file (defualt: /var/lib/ip_lockout)
 3. test config ` # ip_lockout --dry-run`
 4. registrate to system or root crontab; eg)

    */2 * * * * /usr/local/sbin/ip_lockout


### Usage

    -c [file], --config-file=[file]:
          run with specified config file
    -d, --dry-run: dry run (not operate iptables and db)
    
    --ignore-db-entry: don't read existing lockout.db
    --ignore-iptables-entry: don't read existing iptables entries
    
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
2-clause BSDL (see the file LICENCE) for details.

Comments, patches, and glasses/bottles/barrels of beer are welcome :)
