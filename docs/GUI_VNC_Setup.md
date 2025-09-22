# GUI + VNC Setup (Ubuntu / Jetson)

This guide shows multiple reliable ways to enable a desktop GUI and remote access via VNC (and optional RDP) on Ubuntu 22.04/Jetson (JetPack 6.x). Choose the option that fits your needs and device resources.

- Option A: x11vnc (share the existing desktop on display :0, port 5900)
- Option B: TigerVNC (virtual desktop on display :1, port 5901)
- Option C: GNOME Remote Desktop (RDP on 3389; Wayland-friendly)
- Extras: Autostart via systemd, firewall, verification, troubleshooting

> Tip: For Jetson devices, TigerVNC (virtual desktop) or GNOME RDP are lighter than running full GNOME with VNC.

---

## Prerequisites

Update packages:
```bash
sudo apt update
sudo apt upgrade -y
```

Install common tools (optional):
```bash
sudo apt install -y lsof net-tools curl
```

---

## Option A — x11vnc (share the current desktop :0)

Best when you already use a local desktop session and want to mirror it over VNC.

1) Ensure Xorg session (x11vnc cannot mirror Wayland):
```bash
sudo sed -i 's/^#\?WaylandEnable=.*/WaylandEnable=false/' /etc/gdm3/custom.conf
sudo reboot
```

2) Install and set a VNC password:
```bash
sudo apt install -y x11vnc
x11vnc -storepasswd
```

3) Create a systemd service:
```ini
[Unit]
Description=x11vnc on :0
After=display-manager.service network-online.target
Wants=network-online.target

[Service]
Type=simple
User=jetson
ExecStart=/usr/bin/x11vnc -display :0 -auth guess -forever -loop -noxdamage -rfbauth /home/jetson/.vnc/passwd -rfbport 5900 -shared
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
Save as `/etc/systemd/system/x11vnc.service`, then:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now x11vnc.service
```

4) (If UFW is active) allow the port:
```bash
sudo ufw allow 5900
```

Connect using a VNC client to `vnc://<jetson-ip>:5900`.

---

## Option B — TigerVNC (virtual desktop :1)

Creates a separate desktop session (not mirroring the console). Works on Wayland too.

1) Install a lightweight desktop (XFCE recommended) and TigerVNC:
```bash
sudo apt install -y xfce4 xfce4-goodies tigervnc-standalone-server
```

2) Initialize VNC for your user (first run sets password):
```bash
vncserver :1
vncserver -list
```

3) Configure the session startup to launch XFCE:
```bash
mkdir -p ~/.vnc
cat > ~/.vnc/xstartup << 'EOF'
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
startxfce4 &
EOF
chmod +x ~/.vnc/xstartup
```

4) Restart VNC and allow remote connections:
```bash
vncserver -kill :1
vncserver -localhost no :1
```

5) Create a systemd unit for autostart (optional):
```ini
[Unit]
Description=TigerVNC Server on :1
After=network.target

[Service]
Type=forking
User=jetson
ExecStart=/usr/bin/vncserver :1 -localhost no -geometry 1280x800 -depth 24
ExecStop=/usr/bin/vncserver -kill :1

[Install]
WantedBy=multi-user.target
```
Save as `/etc/systemd/system/tigervnc@1.service`, then:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now tigervnc@1.service
```

6) (If UFW is active) allow the port:
```bash
sudo ufw allow 5901
```

Connect to `vnc://<jetson-ip>:5901`.

---

## Option C — GNOME Remote Desktop (RDP, Wayland-friendly)

Works well with Wayland; integrates with GNOME Settings.

```bash
sudo apt install -y gnome-remote-desktop
systemctl --user enable --now gnome-remote-desktop.service
# Then open Settings > Sharing > Remote Desktop, enable and set credentials
# If UFW is active:
sudo ufw allow 3389
```

Connect using an RDP client to `<jetson-ip>:3389`.

---

## Verify & Diagnose

Check services:
```bash
systemctl status x11vnc.service
systemctl status tigervnc@1.service
```

Check listening ports:
```bash
ss -tuln | grep -E ':5900|:5901|:3389'
```

From another machine:
```bash
nc -vz <jetson-ip> 5900   # or 5901 / 3389
```

Logs:
```bash
journalctl -u x11vnc.service -e --no-pager
journalctl -u tigervnc@1.service -e --no-pager
```

---

## Notes
- Replace `jetson` with your actual username if different.
- For TigerVNC, XFCE is lighter than full GNOME and performs better over VNC.
- If you changed display manager settings, a reboot may be required.
- UFW is optional; if inactive, ports are not blocked by it.
