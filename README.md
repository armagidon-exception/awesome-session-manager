# awesome-session-manager

Systemd user session management for AwesomeWM, modelled after
[xdbob/sway-services](https://github.com/xdbob/sway-services).

# Provided targets

- `x11-session.target`
- `x11-session-pre.target`
- `awesome-session.target`
- `awesome-session-pre.target`

`x11-session{,-pre}.target` map directly onto systemd's
`graphical-session{,-pre}.target` and are meant for X11 only services.
`awesome-session{,-pre}.target` are the AwesomeWM session layers.

# Provided services

- `awesome.service`

# Installation

1. From `systemd` put all files into a systemd user unit directory, e.g.
   `$XDG_CONFIG_HOME/systemd/user`, `/etc/systemd/user`, `/usr/lib/systemd/user/`.
2. From `bin` put `awesome-session` in `/usr/bin/`. If you want to use another
   path, you have to edit the xsession file.
3. From `xsessions` put the file into `/usr/share/xsessions/`.
4. Reload the user manager: `systemctl --user daemon-reload`.
5. (Optional) Enable XDG autostart, so applications that drop a desktop file in
   an autostart directory are started automatically:

       systemctl --user add-wants awesome-session.target xdg-desktop-autostart.target

# Adding services to the session

A service that should run inside the graphical session must be stopped together
with it. Use `PartOf=` for that and pull it in with `WantedBy=`:

    [Unit]
    Description=My session service
    PartOf=awesome-session.target
    After=awesome-session.target

    [Service]
    ExecStart=/usr/bin/my-service

    [Install]
    WantedBy=awesome-session.target

Then `systemctl --user enable my-service.service`.

Use `PartOf=x11-session.target` (and `WantedBy=x11-session.target`) for services
that only make sense on X11, and `PartOf=graphical-session.target` for services
that should work on any graphical session. Do **not** use
`Requires=graphical-session.target` to tie a service to the session: an active
service that `Requires` the target prevents the target from ever being stopped,
so the session can never be torn down.

# How it works

`awesome.service` bound to `awesome-session.target`; the session targets are
`BindsTo=` the next layer up to `graphical-session.target`. Every target is
`StopWhenUnneeded=yes`, so when AwesomeWM exits the session targets become
unneeded and stop, taking every `PartOf=` service with them. On the next login
`awesome-session` resets any leftover failed session units and starts
`awesome.service` again, which pulls the whole chain back up from a clean state.

# Inspirations

This project is pretty much a rip off from
[https://github.com/xdbob/sway-services](sway-services).
