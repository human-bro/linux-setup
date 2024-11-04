# linux-setup
## Contains different setup like automatic start vnc in background in a raspberry pi also starting ssh by default (at startup ) , and opens a website which gives us the ip address of the raspberry pi

---

# Starting a vnc server when the raspberry pi is connected to internet 

## Creating .service file to use systemmd
```
sudo nano /etc/systemd/system/vncserver@1.service
```

## Code for the .service file

```
[Unit]
Description=Start VNC server when network is online
After=network-online.target
Wants=network-online.target

[Service]
Type=forking
User=pi
Group=pi
WorkingDirectory=/home/pi
ExecStart=/usr/bin/vncserver :1
ExecStop=/usr/bin/vncserver -kill :1

[Install]
WantedBy=multi-user.target
```

## Save

## Open raspi-config tool
```
sudo raspi-config
```

### System Options --> Boot/AutoLogin --> B4 Desktop/AutoLogin

## To enable the service

```
sudo systemctl enable vncserver@1
```

## To Start the service

```
sudo systemctl start vncserver@1
```
> ### In case you made any changes you should run this commands
> ```
> sudo systemctl daemon-reload
> ```
> ### Then enable the service
> ```
> sudo systemctl enable vncserver@1
> ```

## Now reboot

```
sudo reboot
```
---

# To start ssh with system boot

> ## create ssh file with no extension in the sd card directly in the sd card not inside any folder like overlays doing this will start a ssh server by boot
> ## The password is the device password
> ## ssh pi@ip_address

---

# Setting a Automated script that automatically writes the local ip to a text which can be accessed from anywhere by opening a website in the default browser for ssh or vnc server

## Automatic ip grabber of raspberry pi(it opens a website with ipaddress as the parameter which is stored in a website )

## Create a .service file 

```
sudo nano /etc/systemd/system/chromium.service
```
## Code
```
[Unit]
Description=Start Chromium Browser with dynamic IP at boot
After=graphical.target

[Service]
Environment=DISPLAY=:0
ExecStart=/bin/bash -c '/usr/bin/chromium-browser --no-sandbox --start-fullscreen "https://storingfiles.000.pe/putip.php?ip=$(hostname -I | awk \'{print $1}\')"'
Restart=on-failure
User=pi # here pi is the username of the raspberry pi

[Install]
WantedBy=default.target
```
## To enable the service
```
sudo systemctl enable chromium.service
```

## To start the service
```
sudo systemctl start chromium.service
```
## To see if the service is running or not
```
sudo systemctl status chromium.service
```
## Reboot the system so that the script is loaded and applied correctly to machine
```
sudo reboot
```

