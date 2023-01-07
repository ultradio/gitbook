---
description: Auto Complete, Pylance
---

# Visual Studio Code

### VSCode Extensions

Remote Development



### Windows SSH

Windows > 설정 > 선택적 기능 > OpenSSH 서버 설치



### WSL2 systemctl

System has not been booted with systemd as init system (PID 1). Can't operate

```
sudo -e /etc/wsl.conf

[boot]
systemd=true

wsl --shutdown

sudo systemctl status
```



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
