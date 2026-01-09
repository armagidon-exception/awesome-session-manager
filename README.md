# awesome-session-manager

Systemd session management of awesomewm

# Installation

1. From `systemd` put all files into systemd user unit directory e.g. `$XDG_CONFIG_HOME/systemd/user`, `/etc/systemd/user`, `/usr/lib/systemd/user/`
2. Enable awesome-session.target with `systemctl --user daemon-reload && systemctl enable --user awesome-session.target`
3. From `bin` put executable in `/usr/bin/`. If you want to use another path, you have to edit xsession file.
4. From `xsessions` put file into `/usr/share/xsessions/`
5. (Optional) Enable xdg-autostart with `systemctl --user enable xdg-desktop-autostart.target`. (See man systemd.special(7))
   Now you have autostarting capabilities without dex or startup scripts if the application exports desktop file into autostart directory.

# Why?
I don't know. I kind of makes the system well integrated without some stupid hacks.
My usecase was that I can enable opentabledriver and it will launch automatically at start, like it's documented in opentabledriver docs.

# Tested?
I am using this setup on my ArchLinux machine. So far it's working fine.

# I found a bug!
Great! You can open issue or submit a bugfix.
