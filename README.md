# CheckMyIP (TelnetMyIP.com) ![CheckMyIP][logo]

A Telnet, SSH and Simple HTTP Based Public IP Address Lookup Service

-----------------------------------------

## USAGE

- TELNET: `telnet telnetmyip.com`
- SSH: `ssh sshmyip.com`
  - Your SSH client may require you to enter a username. You can use anything you want (`ssh -limrootbitch telnetmyip.com`)
- CURL: `curl telnetmyip.com`
- WGET: `wget -qO- telnetmyip.com`

-----------------------------------------

### VERSION

The version of CheckMyIP documented here is: **v1.3.0**

-----------------------------------------

### TABLE OF CONTENTS

1. [What is CheckMyIP](#what-is-checkmyip)
2. [How to Use](#how-to-use)
3. [Using the API](#using-the-api)
4. [Install Process](#install-process)
5. [1.0.0 TO 1.1.0 Updates](#updates-in-v100----v110)
6. [Contributing](#contributing)

-----------------------------------------

### WHAT IS CHECKMYIP

Everybody has used a service like [WhatIsMyIP.com](https://www.whatismyip.com/) before. If you are an IT engineer or even an amateur technology enthusiast, then you have probably had a reason to check to see your public IP address. This service works great when a browser is available, but at times it is not. We often find ourselves logged into a remote Linux machine or a network switch/router which has a command line and terminal clients (Telnet and SSH), but no browser. The CheckMyIP app and the **TelnetMyIP.com** and **SSHMyIP.com** public services were created with this in mind.

-----------------------------------------

### HOW TO USE

Using the public **TelnetMyIP.com** and **SSHMyIP.com** services is pretty easy: simply connect to them with a terminal client. You can use a telnet client with TCP port 23 (`telnet telnetmyip.com`), a SSH client with TCP port 22 (`ssh sshmyip.com`), or CURL (`curl telnetmyip.com`). The SSH connection requires no authentication, but your SSH client may require you to enter a username, you can use anything you want as it gets ignored anyways(`ssh -limrootbitch telnetmyip.com`).

You can also browse to the HTTP version of the service at [TelnetMyIP.com](http://telnetmyip.com/) which will return a JSON reply with your IP information.

To enable the use of this service as a simple API, the response is formatted as a JSON document. See the [Using the API](#using-the-api) section for information on how to leverage the API.

**Note:** _You can also connect to_ `ipv4.telnetmyip.com` _or_ `ipv6.telnetmyip.com` _if you want to check a specific IP stack._

**Note:** _The DNS records for_ `telnetmyip.com` _and_ `sshmyip.com` _point to the same services._

-----------------------------------------

### USING THE API

The CheckMyIP code contains the `CheckMyIP_Client` class which is an API client example which can be used to query a CheckMyIP server (like telnetmyip.com). Below is an example of how you can use it.

```python
from checkmyip import CheckMyIP_Client

client = CheckMyIP_Client()
ipdict = client.get()
print("\nMy IP is %s\n" % ipdict["ip"])
print("\nI used port number %s\n" % ipdict["port"])
```

-----------------------------------------

### INSTALL PROCESS

If you would rather set up your own private instance of CheckMyIP, then you can follow the below instructions to set it up for yourself.

Change Linux SSH Port to TCP 222 and reboot

```bash
sudo sed -i --follow-symlinks 's/#Port 22/Port 222/g' /etc/ssh/sshd_config

shutdown -r now
```

Install Dependencies

```bash
sudo apt install python3-pip
sudo -H pip3 install paramiko
sudo apt install python3-gssapi
```

Clone Repo

```bash
git clone https://github.com/PackeTsar/checkmyip.git
```

Create Service (`sudo nano /etc/systemd/system/checkmyip.service`)

```bash
[Unit]
Description=CheckMyIP Service
After=network-online.target
Wants=network-online.target

[Service]
Type=simple

PIDFile=/var/tmp/checkmyip.pid
WorkingDirectory=/home/ubuntu/checkmyip

ExecStart=/usr/bin/python3 checkmyip.py

Restart=on-failure
RestartSec=30
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

Finish and Start Up Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable checkmyip
sudo systemctl start checkmyip
sudo systemctl status checkmyip
```

-----------------------------------------

### UPDATES IN V1.0.0 --> V1.1.0

**NEW FEATURES:**

- Was seeing issues where SSH would be very slow to exchange. Likely related to log file sizes, so I change the logging function to turnover to new logging files every day.

-----------------------------------------

### UPDATES IN V1.1.0 --> V1.3.0

- README updated for install on Ubuntu instead of CentOS
- Small tweaks to support Python3

-----------------------------------------

### CONTRIBUTING

If you would like to help out by contributing code or reporting issues, please do!

Visit the GitHub page (<https://github.com/packetsar/checkmyip>) and either report an issue or fork the project, commit some changes, and submit a pull request.

[twitter-logo]: http://www.packetsar.com/wp-content/uploads/twitter-logo-35.png
[twitter]: https://twitter.com/TelnetMyIP
[logo]: http://www.packetsar.com/wp-content/uploads/checkmyip_icon-100.gif
[whatismyip]: https://www.whatismyip.com/
