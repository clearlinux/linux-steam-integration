linux-steam-integration
-----------------------

Linux Steam* Integration is a helper system to make the Steam Client and Steam
games run better on Linux. In a nutshell, LSI automatically applies various workarounds
to get games working, and fixes long standing bugs in both games and the client.

![screenshot](https://raw.githubusercontent.com/clearlinux/linux-steam-integration/master/.github/LSI_Settings.png)

In many cases this will involve controlling which libraries are allowed to be used
at any given time, and these libraries may be overriden for any of the following
reasons:

 - Security
 - Compatibility
 - Performance

This project, and by extension the platforms it runs on, are not officially endorsed by, or affiliated with, Steam, or its parent company, Valve*.
This fork of linux-steam-integration is now being maintained separately from the original project, to suit the cadence and requirements of the
Clear Linux* Project for Intel Architecture.

This project will retain backwards compatibility with the original project at the time of forking to alleviate maintainer concerns, whilst focusing
on improvements to match a wider audience than the original project, and ensuring native runtimes continue to work unimpeded.

### Linking compatibility

With LSI, you don't need to worry any more about manually mangling your Steam installation
just to make the open source drivers work, or manually creating links and installing
unsupported libraries. LSI is designed to take care of all of this for you.

Many library names are redirected through the main "intercept" module, which ensures
games will (where appropriate) use the updated system libraries. Additionally the
module can override how games and the Steam client are allowed to make use of
vendored libraries. This will help with many launch failures involving outdated
libraries, or indeed the infamous `libstdc++.so.6` vendoring which breaks open
source graphics drivers on systems compiled with the new GCC C++ ABI.


### Apply path based hotfixes to games

The redirect module contains some profiles to allow us to dynamically fix some
issues that would otherwise require new builds of the games to see those issues
resolved.

 - Project Highrise: Ensure we don't `mmap` a directory as a file (fixes invalid prefs path)
 - ARK Survival Evolved: Use the correct shader asset from TheCenter DLC to fix broken water surfaces.

### Unity3D Black Screen Of Nope

Older builds of Unity3D had (long since fixed) issues with launching to a black
screen when defaulting to full screen mode. This is commonly addressed by launching the
games with `-screen-fullscreen 0`, and is due to an invalid internal condition clamping
the renderer size to 0x0 after setting the fullscreen (borderless) window size.

Note - updating these games to newer versions of Unity will fix this bug on Linux, however
LSI currently ships a workaround. This workaround will abstract access to the configuration
file in `$XDG_CONFIG_HOME/.config/unity3d/*/prefs` through the Linux `/dev/shm` system,
and will provide initial game configuration whilst also masking the harmful fullscreen
setting.

Net result - all Unity3D games using this pref path (the older generation) will start
in windowed mode always. They can be fullscreened from inside the game, and this will
help with making sure games **actually launch**.

### Notes

Note that LSI will not modify your Steam installation, and instead makes use of two
modules, `liblsi-redirect.so` and `liblsi-intercept.so`, to dynamically apply all of the
workarounds at runtime, which in turn is set up by the main LSI `shim` binary.

For a more in depth view of what LSI is, and how to integrate it into your distribution,
please check the [technical README document](https://github.com/clearlinux/linux-steam-integration/blob/master/TECHNICAL.md).


## License

Copyright © 2016-2018 Ikey Doherty

Copyright © 2018-2019 Intel Corporation

linux-steam-integration is available under the terms of the `LGPL-2.1` license.

See the accompanying `LICENSE.LGPL2.1` file for more details


`data/lsi-steam.desktop`:

        This file borrows translations from the official `steam.desktop` launcher.
        These are copyright of Valve*.

`* Some names may be claimed as the property of others.`
