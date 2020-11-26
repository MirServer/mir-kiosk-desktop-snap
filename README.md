# mir-kiosk-desktop

This provides the boilerplate for using mir_kiosk_x11 for a confined desktop session.

It is intended for use as a `stage-snap` and provides the essentials to create a confined desktop session with a single fullscreen application. The resulting snap needs registering with the desktop manager by running:
```plain
/snap/NAME_OF_SNAP/current/desktop/setup
``` 
And also connecting the `login-session-control` and 'hardware-observe' interfaces:
```plain
snap connect NAME_OF_SNAP:login-session-control
snap connect NAME_OF_SNAP:hardware-observe
``` 

There are certain elements that needed to be included in the final snap:

```yaml
parts:
  mir-kiosk-desktop:
    plugin: nil
    stage-snaps: [mir-kiosk-desktop]
    override-build: |
      snapcraftctl build
      sed s/\$\{SNAPCRAFT_PROJECT_NAME}/${SNAPCRAFT_PROJECT_NAME}/g --in-place $SNAPCRAFT_PART_INSTALL/desktop/setup
      sed s/\$\{SNAPCRAFT_PROJECT_NAME}/${SNAPCRAFT_PROJECT_NAME}/g --in-place $SNAPCRAFT_PART_INSTALL/desktop/mir-kiosk-desktop.desktop

layout:
  /usr/share/X11:
    bind: $SNAP/usr/share/X11
  /usr/bin/xkbcomp:
    symlink: $SNAP/usr/bin/xkbcomp
  /usr/share/icons:
    bind: $SNAP/usr/share/icons
  /usr/share/fonts:
    bind: $SNAP/usr/share/fonts
  /etc/fonts:
    bind: $SNAP/etc/fonts

plugs:
  opengl:         # For Mir
  wayland:        # For Mir
  network-bind:   # For Mir (to serve X11)
  login-session-control: # To start a desktop session
  hardware-observe: # To start a desktop session
```