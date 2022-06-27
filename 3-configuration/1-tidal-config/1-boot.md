---
title: The Boot File 
id: boot-tidal
---

Every time you start Tidal, the software is reading from a configuration file usually named `BootTidal.hs`. Generally, this file will be attached to your text editor (check the plugin you are using). Save this file somewhere safe, you will have to tweak it sometimes to change options, adding new functionality, etc.

Some users went really far into customizing their setup: see [Jarmlib](https://github.com/jarmitage/jarmlib). You can take a look at their work to see how to extend your configuration file.

Here is the *vanilla* `BootTidal.hs` file used by the [Atom Package for Tidal](https://raw.githubusercontent.com/tidalcycles/atom-tidalcycles/master/lib/BootTidal.hs):

<!-- TODO: embed github? -->

## Controlling Latency

There are two configuration values which control overall latency: `frame timespan` and `latency`. To find the maximum total latency, add them together. Here's an example configuration:

```haskell
tidal <- startTidal (superdirtTarget {oLatency = 0.02}) (defaultConfig {cFrameTimespan = 1/20})
```

- Frame timespan

  This is the duration of Tidal's calculation window in seconds. The default is `0.05 seconds`, in other words a calculation rate of 20 frames per second. If you find Tidal is using too much CPU, increasing the frame timespan will probably help. 

- Latency

  If you get late messages in supercollider, you can increase the latency by increasing this from its default value (which at the time of writing is `0.02`).


## SuperDirt running in another host

If you're running **SuperDirt** in another host (perhaps, in a multi-user setup), you need to define this in a similar fashion as with the latency, except in this case the keyname is `oAddress`:

```haskell
tidal <- startTidal (superdirtTarget {oAddress = "192.168.0.23", oPort = 57120}) defaultConfig
```

In case you need to alter multiple settings for `superdirtTarget`, just separate them by a comma:

```haskell
{oAddress = "1.2.3.4", oLatency = 0.04}
```

Note that in Emacs (and possibly other editor) configuration files, you'll need to escape the quotation marks.

You will also need to configure **SuperDirt** to accept messages coming from another host. For example, starting Dirt like this will tell listen for OSC messages on all network interfaces:

```haskell
~dirt.start(57120, [0, 0], NetAddr("0.0.0.0"));
```
