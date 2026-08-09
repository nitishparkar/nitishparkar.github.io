---
title: "Swapping Ctrl and Alt in Ubuntu Without Breaking Ctrl+Tab"
---

I use a MacBook at work and like my personal Ubuntu laptop to have similar keybindings, so switching between the two feels as seamless as possible. In particular, I like to swap the left Ctrl and Alt keys in Ubuntu:

- Physical Left Ctrl should behave like Left Alt
- Physical Left Alt should behave like Left Ctrl

There is one exception: switching between applications. I like to do that using Physical Left Alt + Tab, so that it is closer to the Mac.

This worked well in Ubuntu 24.04. After upgrading to Ubuntu 26.04, however, I noticed an unexpected difference. After installing gnome-tweaks and selecting “Swap Left Alt with Left Ctrl”, pressing physical Ctrl+Tab switched between applications instead of physical Alt+Tab.

## What was different in Ubuntu 26.04?

The Ctrl/Alt swap itself was not the problem.

In both Ubuntu 24.04 and 26.04, GNOME Tweaks uses the same XKB option:

```text
ctrl:swap_lalt_lctl
```

The difference was in GNOME's keyboard shortcuts.

Ubuntu 24.04 had:

```text
switch-applications = ['<Primary>Tab']
```

while Ubuntu 26.04 had:

```text
switch-applications = ['<Super>Tab']
```

With the Ctrl/Alt swap enabled, this resulted in the unexpected application/window switching behavior.

## The fix

I restored my Ubuntu 24.04 application-switching shortcuts:

```bash
gsettings set org.gnome.desktop.wm.keybindings switch-applications "['<Primary>Tab']"

gsettings set org.gnome.desktop.wm.keybindings switch-applications-backward "['<Shift><Primary>Tab']"
```

And verifed the settings with:

```bash
gsettings get org.gnome.desktop.wm.keybindings switch-applications
gsettings get org.gnome.desktop.wm.keybindings switch-applications-backward
```

They returned:

```text
['<Primary>Tab']
['<Shift><Primary>Tab']
```

With the Ctrl/Alt swap still enabled in GNOME Tweaks, this restored the keyboard behaviour I had in Ubuntu 24.04:

* `Physical Alt+C` -> Copy
* `Physical Alt+V` -> Paste
* `Physical Alt+Z` -> Undo
* `Physical Alt+Tab` -> Switch applications

No additional keyboard remapping software such as `keyd` is required.

This issue may not be with Ubuntu 26. I have had this machine for 7+ years and have been upgrading from LTS to LTS. This time, I decided to do a fresh install to get rid of all the accumulated cruft. The process of fresh installation and fixing small issues like this was significantly simplified and faster because of LLMs. I had a ChatGPT session open and was driving it all the way. It wasn't perfect, though. It kept pushing for keyd, but I did not want to have another dependency since the setup was working fine earlier. I hope this helps someone if they are facing a similar issue.
