- [/home/eridan](#homeeridan)
  - [.xserverrc](#xserverrc)
  - [.Xresources](#xresources)
  - [.ssh](#ssh)
    - [.ssh/config](#sshconfig)

# /home/eridan

## [.xserverrc](.xserverrc)
- For rootless Xorg, we need to add the virtual terminal where we start our X session, hence `vt$XDG_VTNR`
- Since we're also running Xorg rootless, to keep logs locally, we'll put `-keeptty` before our virtual temrinal.
- For extra security, we will disable abstract (UNIX) sockets of X11 using `-nolisten local`. See [X11 security isolation](https://tstarling.com/blog/2016/06/x11-security-isolation/)

## [.Xresources](.Xresources)
- While I don't use URxvt anymore (switched to Alacritty), the options I previously used are there.

## [.ssh](.ssh)

### [.ssh/config](.ssh/config)
Only important thing there is the `github.com` block so I can use Git over SSH (on github.com) instead of HTTPS.