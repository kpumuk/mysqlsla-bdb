mysqlsla-bdb version 2.03
=========================
This version of mysqlsla has been modified to work with huge (multi-gigabyte)
slow logs by using BerkeleyDB instead of plain perl hashes. This version
is about 2 times slower and we did not test it with anything but slow logs
(but it should work, so submit [your issues] [4], I'll try to fix). Patches
are welcome too.

[Replays] [5] work too in this patch. There a difference: it stores
several files instead of a single one. If your relay file called "mylog.relay",
mysqlsla-bdb will create several files "mylog.relay.\*.tmp", where it will store
BDB files and additional hashes. Please note: I have removed *third limitation:
total users (all log-wide unique "user@host IP") does not work with replays.*

This package contains both old (unpatched) version of mysqlsla, and the patched
one, called mysqlsla-bdb. All arguments are the same.

Here are some stats: 70 Gb slow log file took 7 hours to build a report.
Memory usage is about 20-30 Mb.

mysqlsla version 2.03
=====================
mysqlsla parses, filters, analyzes and sorts MySQL slow, general, binary
and microslow patched slow logs in order to create a customizable report
of the queries and their meta-property values.

Since these reports are customizable, they can be used for human consumption
or be fed into other scripts to further analyze the queries.

For example, to profile with mk-query-profiler (a script from Baron Schwartz's
Maatkit) every unique `SELECT` statement using database foo from a slow log:

    mysqlsla -lt slow slow.log -R print-unique -mf "db=foo" -sf "+SELECT" | \
    mk-query-profiler -separate -database foo

In brief, mysqlsla is a liaison allowing other scripts easy access to queries
from a MySQL log.


Installation
============
See included INSTALL file.


Dependencies
============
The following core Perl modules are used which should already be installed
on your system:

    Time::HiRes
    File::Temp
    Data::Dumper
    DBI
    Getopt::Long
    Storable
    BerkeleyDB

If available, `Term::ReadKey` will also be used.

mysqlsla v2 uses an "internalized" version of [MySQL::Log::ParseFilter] [2].
You do _not_ need to install this module separately.


Web sites
=========
1. [mysqlsla] [1]
2. [MySQL::Log::ParseFilter] [2]
3. [Maatkit] [3]


Copyright and Licence
=====================
Copyright 2007-2008 Daniel Nichter

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation; either version 2
of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

The GNU General Public License is available at:
http://www.gnu.org/copyleft/gpl.html

  [1]: http://hackmysql.com/mysqlsla  "mysqlsla"
  [2]: http://hackmysql.com/mlp       "MySQL::Log::ParseFilter"
  [3]: http://www.maatkit.org/        "Maatkit"
  [4]: http://github.com/kpumuk/mysqlsla-bdb/issues "Issues"
  [5]: http://hackmysql.com/mysqlsla_replays "mysqlsla v2 Replays"
