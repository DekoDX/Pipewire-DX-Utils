# Pipewire-Utils-DX
A tunable configuration file example for a virtual 7.1.4 surround sound setup on linux.
Windows Games with dolby atmos through proton should output vertical channels(Tested with Helldivers 2).
This repo also provides a convolution eq sink and a noise cancelling sink powered by Khip.

# Requirements
Fedora users specifically need the `pipewire-module-filter-chain-sofa` package for this to function.
Systemd users should have the filter-chain.service user service enabled after properly setting this up.
Run either `systemctl --user enable --now filter-chain.service` or `systemctl --global enable filter-chain.service # Enable filter-chain support for all users` to enable this.
Dinit+Turnstile users might have a filter-chain user service but I can not guarantee this.

# Setup
replace `sofa-path` with a sofa file of your choosing. You can download many of them from https://kutt.it/BinauralHRTF. A good baseline is EAC_Default.
If you use the Equalizer Sink be sure to replace `eq-path` with your convulution eq. If you don't already have one, it can be obtained from https://autoeq.app or https://peqdb.com/

If you decide to use the primary noise cancelling sink you either need to get the ladspa from https://codeberg.org/khip/khip and update the ladspa path or use the aur package `khip`
A tuneable noise cancelling sink using noise-suppression-for-voice is also provided. Requires a valid `noise-suppression-for-voice` package. Arch Linux has it in their repos and Fedora Linux users can grab it from the [Audinux Copr Repository](https://audinux.github.io/) as `ladspa-noise-suppression-for-voice`

I personally recommend placing them in `/home/$USER/.local/share/pipewire/eq/`, `/home/$USER/.local/share/pipewire/hrir/` and `/home/$USER/.ladspa/`.
Move the files to `~/.config/pipewire/filter-chain.conf.d/`.
You should then run `systemctl --user enable --now filter-chain.service` or `sudo systemctl --global enable filter-chain.service # You may need to log out and log back in after running these`.
Those without systemd must either autostart `/usr/bin/pipewire -c filter-chain.conf` after pipewire is started or write a userspace filter-chain service for their distribution calling that command that depends on your pipewire service.
You can reload the configuration files by either running `systemctl --user restart filter-chain.service` or `/usr/bin/pipewire -c filter-chain.conf`.
After this is all done you should make sure that the sinks are pointed to the right devices with pavucontrol or pavucontrol-qt. You should optionally make sure your virtual surround sink outputs to the equalizer sink.

If you enable the eq sink, you should ensure the virtual surround sink is outputting to that and not vice versa.

# Special Thanks
[3DJ](https://github.com/ThreeDeeJay) for providing contributions to the 3d audio space and an excellent amount of resources for 3D Audio enthusiasts.

https://codeberg.org/khip/khip for providing the primary noise cancellation ladspa.

https://github.com/werman/noise-suppression-for-voice for providing the alternative noise cancellation ladspa

Razer for inspiring me to upgrade my virtual surround sink to match and exceed the capabilities of some new software for one of their headsets.

[The Binaural Audio Discord](https://discord.gg/B4n97X7dG4) by 3DJ for more inspiration. Feel free to ask me any questions there in regards to this and I'll answer them to the best of my ability.
