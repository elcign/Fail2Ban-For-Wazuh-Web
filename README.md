# Fail2Ban-For-Wazuh-Web
A simple custom Fail2Ban filter for the Wazuh website.

I spent some time tracking down some log that would work, and it isn't pretty, but hopefully it will help someone else out there.  It was last tested on Wazuh 4.8.0.

First, install Fail2ban.

Then, on Debian I was able to add this to /etc/fail2ban/jail.local near the bottom:
```
[wazuh]
enabled	= true
filter = wazuh
port = http,https
logpath = /var/log/messages
```
You'll possibly need to modify the logpath for whatever is doing the general syslogs, and you may have to enable writing to that output in addition to journal.

And then, to make the filter:
```
sudo nano /etc/fail2ban/filter.d/wazuh.conf
```

Filter contents:
```
[Definition]
failregex = ^.*opensearch-dashboards.*"remoteAddress":"<HOST>".*POST \/auth\/login 401.*$
```

Followed by something like this for good measure:
```
sudo systemctl enable fail2ban && sudo systemctl start fail2ban && sudo systemctl restart fail2ban && sudo systemctl status fail2ban
```
