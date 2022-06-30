## Systemd

Each package that contains software that wants/needs to start a traditional service at boot **MUST** have a systemd unit file. KMonad obeys this rule, so simply executing `systemctl enable kmonad.service` enables starting at boot, and the unit file is located at `/usr/lib/systemd/system/kmonad.service`.

> Basically, files that ships in packages downloaded from distribution repository go into /usr/lib/systemd/. Modifications done by system administrator (user) go into /etc/systemd/system/. [Ref](https://unix.stackexchange.com/a/208352)

`%E` in the unit file means **configuration directory root**.
> It is either `/etc/` (for the system manager) or the path `$XDG_CONFIG_HOME` resolves to (for user managers). [Ref](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)

## Config

```lisp
~/.config/kmonad/config.kbd
---------------------------
;; Layout Template with a Column Width of 8 Characters
;; (defsrc
;;   _      _      _      _      _      _      _      _      _      _      _      _      _      _      _      _
;;   _      _      _      _      _      _      _      _      _      _      _      _      _      _             _
;;   _      _      _      _      _      _      _      _      _      _      _      _      _      _             _
;;   _      _      _      _      _      _      _      _      _      _      _      _      _                    _
;;   _      _      _      _      _      _      _      _      _      _      _      _                    _      _
;;   _      _      _                    _                    _      _      _                    _      _      _
;; )

(defcfg
  ;; For Linux
  input  (device-file "/dev/input/by-id/usb-Keychron_Keychron_K2-event-kbd")
  output (uinput-sink "Keychron K2")

  fallthrough true
  allow-cmd   false
)

(defsrc
  esc    f1     f2     f3     f4     f5     f6     f7     f8     f9     f10    f11    f12    ins    prnt   del
  grv    1      2      3      4      5      6      7      8      9      0      -      =      bspc          pgup
  tab    q      w      e      r      t      y      u      i      o      p      [      ]      \             pgdn
  caps   a      s      d      f      g      h      j      k      l      ;      '      ret                  home
  lsft   z      x      c      v      b      n      m      ,      .      /      rsft                 up     end
  lctl   lalt   lmet                 spc                  rmet   cmp    rctl                 left   down   rght
)

(defalias 
  fn    (around (layer-toggle function) cmp)
  cmkdh (layer-switch colemak-mod-dh)
  us    (layer-switch united-states)
  esctl (tap-next esc lctl)
)

;; Default Layout
(deflayer colemak-mod-dh
  esc    f1     f2     f3     f4     f5     f6     f7     f8     f9     f10    f11    f12    @us    prnt   del
  grv    1      2      3      4      5      6      7      8      9      0      -      =      bspc          pgup
  tab    q      w      f      p      b      j      l      u      y      ;      [      ]      \             pgdn
  @esctl a      r      s      t      g      m      n      e      i      o      '      ret                  home
  lsft   x      c      d      v      z      k      h      ,      .      /      rsft                 up     end
  lctl   lalt   lmet                 spc                  rmet   @fn    rctl                 left   down   rght
)

(deflayer function
  _      brdn   brup   _      _      _      _      _      _      _      mute   vold   volu   _      _      _
  _      _      _      _      _      _      _      _      _      _      _      _      _      _             _
  _      _      _      _      _      _      _      _      _      _      _      _      _      _             _
  _      _      _      _      _      _      _      _      _      _      _      _      _                    _
  _      _      _      _      _      _      _      _      _      _      _      _                    _      _
  _      _      _                    _                    _      _      _                    _      _      _
)

(deflayer united-states
  esc    f1     f2     f3     f4     f5     f6     f7     f8     f9     f10    f11    f12    @cmkdh prnt   del
  grv    1      2      3      4      5      6      7      8      9      0      -      =      bspc          pgup
  tab    q      w      e      r      t      y      u      i      o      p      [      ]      \             pgdn
  @esctl a      s      d      f      g      h      j      k      l      ;      '      ret                  home
  lsft   z      x      c      v      b      n      m      ,      .      /      rsft                 up     end
  lctl   lalt   lmet                 spc                  rmet   @fn    rctl                 left   down   rght
)
```