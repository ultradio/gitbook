---
description: Auto Complete, Pylance
---

# Visual Studio Code

### Auto Complete

1\) Extensions > Pylance 설치

2\) 설정

{% code title="File > Preperences > Settings" %}
```
{
  "python.languageServer": "Pylance",
}
```
{% endcode %}

System has not been booted with systemd as init system (PID 1). Can't operate

```
sudo -e /etc/wsl.conf

[boot]
systemd=true

wsl --shutdown

sudo systemctl status
```

