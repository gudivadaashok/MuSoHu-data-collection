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


Here are all the commands mentioned in the video:

* `apt update`: Used to update the package lists.
* `apt upgrade`: Used to upgrade installed packages.
* `apt install tasksel`: Installs the tasksel utility, which allows for installing collections of software at once.
* `apt install slim`: Installs the slim display manager.
* `apt install lightdm`: Installs the lightdm display manager.
* `cat /etc/X11/default-display-manager`: Checks the default display manager on the system.
* `tasksel`: Used to install Ubuntu desktop.
* `apt install tigervnc-standalone-server`: Installs the TigerVNC standalone server.
* `adduser tony`: Creates a new user named 'tony'.
* `su tony`: Switches to the 'tony' user.
* `vncserver -localhost no`: Sets up a VNC server.
* `vncserver -list`: Lists active VNC servers.
* `vncserver -kill :1`: Kills the VNC server running on display 1.