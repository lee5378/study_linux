# Logging

## Syslog

Syslog protocol severity values

|Code |Keyword 
--- | --- 
|0|emerg
|1|alert
|2|crit
|3|err
|4|warning
|5|notice
|6|info
|7|debug

## Rsyslog

Rsyslog uses all features of original syslog protocol. Settings are stored in `/etc/rsyslog.conf`
- Logs all events of a chosen severity and higher

`logger` can be used to log your own application events. Use `tail` to see the most recent log entries in `var/log/syslog`.

```
ghost@practice1:~$ logger Pigs can fly
ghost@practice1:~$ tail /var/log/syslog
Jul  5 01:14:35 practice1 ghost: Pigs can fly
```
Note that `rsyslogd` added the timestamp automatically.

Most applications will output logs to `/var/log` and then the application will have its own folder, e.g. `/var/log/apache2` for an Apache web server.

`tail -f` displays the last few lines int eh log file but then monitors the file for any new entries and displays them too.

## Journaling

`systemd-journald` is a journal utility belonging to the Systemd services package. Configuration takes place in `/etc/systmd/journald.conf`.
