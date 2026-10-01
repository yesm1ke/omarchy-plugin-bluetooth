# Bluetooth for the Omarchy bar, hidden in the tray drawer

The stock Omarchy Bluetooth widget, unchanged, except that its bar icon hides
together with the system tray drawer: it stays collapsed until you hover the
tray's `‹` chevron, then slides out next to the tray icons.

Nothing of the stock widget is copied. `Panel.qml` imports the packaged
`omarchy.bluetooth` panel from `/usr/share/omarchy` and only overrides the
size of its bar icon, so every Omarchy update to the Bluetooth widget applies
here as is.

## Install

```sh
omarchy plugin add https://github.com/yesm1ke/omarchy-plugin-bluetooth --enable
omarchy bar move io.github.yesm1ke.bluetooth --after omarchy.tray   # or io.github.yesm1ke.tray
omarchy plugin disable omarchy.bluetooth   # the stock widget it replaces
```

Requires Omarchy 4 and a Bluetooth adapter.

## Behaviour

- Hover the tray chevron: the drawer opens and the icon slides out with it.
- While the pointer is on the icon, or its panel is open, the drawer stays
  open; move away and everything collapses with the tray's own animation.
- Other widgets that use the same helper (the ZeroTier and Tailscale plugins
  from the same author) form one group with it; place them all right after
  the tray.
- Opening the panel by keybinding or IPC reveals the icon as well. The IPC
  target keeps the stock name: `omarchy-shell omarchy.bluetooth toggle`.

## Settings

| Setting | Default | |
|---|---|---|
| `hideWithTray` | `true` | `false` keeps the icon always visible |

```sh
omarchy bar set io.github.yesm1ke.bluetooth hideWithTray false --json
```

## How it works

`TrayFollower.qml` finds the tray (`io.github.yesm1ke.tray` or the stock
`omarchy.tray`) among its sibling bar slots
and mirrors the tray's `expanded` state. That relies on bar internals
(`ModuleSlot.moduleName` / `activeItem` / `hovered`, `Tray.expanded`); if a
future Omarchy changes them, the helper cannot find the tray and the icon
simply stays visible. The wrapper also relies on the stock panel living at
`/usr/share/omarchy/shell/plugins/panels/bluetooth/`.

The stock `omarchy.tray` hides itself, chevron included, while no app has a
tray icon; the icon then has nothing to reveal it and stays visible. Use
[omarchy-plugin-tray](https://github.com/yesm1ke/omarchy-plugin-tray) in
place of `omarchy.tray` to keep the chevron in that case.

## License

MIT, see [LICENSE](LICENSE).
