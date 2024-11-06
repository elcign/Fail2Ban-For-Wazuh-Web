# Fail2Ban-For-Wazuh-Web
A simple Fail2Ban filter for Wazuh web management when using journald.

This was last tested on Wazuh 4.9.1.

First, install Fail2ban.

This probably needs to be somewhere in the default section of /etc/fail2ban/jail.local:
```
[DEFAULT]
backend = systemd
```

Then, I was able to add this near the bottom:
```
[wazuh]
enabled = true
port = https,http
filter = wazuh
```

And then, to make the filter:
```
sudo nano /etc/fail2ban/filter.d/wazuh.conf
```

Filter contents (not sure if INCLUDES is required):
```
[INCLUDES]
before = common.conf

[Definition]
failregex = ^.*opensearch-dashboards.*"remoteAddress":"<HOST>".*POST \/auth\/login 401.*$
journalmatch = _SYSTEMD_UNIT=wazuh-dashboard.service
```

Followed by something like this for good measure:
```
sudo systemctl enable fail2ban && sudo systemctl start fail2ban && sudo systemctl restart fail2ban && sudo systemctl status fail2ban
```
