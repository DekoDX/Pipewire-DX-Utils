# Pipewire-Utils-DX
## These configuration files will not work on your system without changing some values
These are my Pipewire dotfiles which I found useful enough to share.
Only use the echo cancellation sink if you have speakers or open back headphones.

If you want a gui or want the hesuvi `.wav` virtual surround format instead you should use [EasyEffects](https://github.com/wwmm/easyeffects) and [virtual-surround-manager](https://github.com/Berny23/virtual-surround-manager)

# Requirements
Fedora users specifically need the `pipewire-module-filter-chain-sofa` package for this to function.
If you are using the files in `filter-chain.conf.d` start/enable the `filter-chain.service` user service

# Setup
You should change `node.target` in the files.
You can list devices with `wpctl status` and get the `node.target` value with `wpctl status <device number>`.

## Filter Chains
### eq.conf
Replace the eq-path with a supported `.wav` file. My recommended source for these is https://autoeq.app
### surround-*.conf
Replace the paths in the virtual surround files with a `.sofa` file. My recommended source for these is [here](https://airtable.com/appayGNkn3nSuXkaz/shruimhjdSakUPg2m).
Quirks:
- Fedora requires `pipewire-modules-filter-chain-sofa` to be installed with dnf
- Other distributions should have pipewire built with libmysofa.
- Ubuntu does not build pipewire with libmysofa
- Gentoo does not have a `sofa` use flag. This should be able to be contributed easily.
### nc.conf
Remove the node.target value if not using `ec.conf`.
Requires [noise-cancellation-for-voice](https://github.com/werman/noise-suppression-for-voice/releases)
Packages:
- Arch Linux: `noise-suppression-for-voice`
- Fedora: `ladspa-noise-suppression-for-voice` from [Audinux](https://copr.fedorainfracloud.org/coprs/ycollet/audinux).
- Distributions that do not package should download the ladspa and point the configuration to it.
### ec.conf
Only required if you have speakers or headphones that leak out audio.
The 2 `node.target` values should be adjusted to your microphone and headphones/speakers.

## Other
### pipewire.conf.d/allowed-rates.conf
You should change `default.clock.allowed-rates` to the rates your dac supports. You can list supported formats with `cat /proc/asound/card*/stream0`.
`default.clock.rate` shouldn't need changed as pipewire will change sample rates as needed.
Remove the quantum values if you face issues. These won't work for everyone and simply reduce latency on capable hardware.
### The rest should need no changes, are optional, or are described in the files.
