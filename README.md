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

## Now reboot

```
sudo reboot
```
