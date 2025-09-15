<div align="center">
<img src="https://github.com/diyhue/hassio-addon/blob/master/images/diyhue-hassio.png">
<h1>Home Assistant Add-on: diyHue</h1>
<br>
<p>Run <a href="">diyHue</a> as a Home Assistant Add-on</p>
<a href="https://github.com/babadam2002/diyhue-HA-addon-AmbilightTV-mod"><img src="https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg"></a>
</div>

## Support the real diyHue!
[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/diyhue)

![diyHue ecosystem][img-ecosystem]

## Installation

The installation process is pretty easy and straight forward, like for any other third-party Home Assistang Add-on.

Add the repository URL under **Supervisor → Add-on store → ⋮ → Manage add-on repositories**:

    https://github.com/diyhue/hassio-addon](https://github.com/babadam2002/diyhue-HA-addon-AmbilightTV-mod


## Usage
- Set these options in the Home Assistant addon UI under "Configuration".
- `min_brightness_threshold`: Black level for AmbilightTV + Hue. Minimum 35 recommended. AmbilightTv have one problem, that the backgound minimum light is always on, this will turn it off.
- `Entertainment_Area_Extender_udp_ip`: IP address for the plugin. Usually your HA IP without port. You need to install the Entertainment Area Extender addon  for diyhue-HA-addon-AmbilightTV-mod addon.

- `Entertainment_Area_Extender_udp_port`: UDP port. Default is 12345.


[img-ecosystem]: https://raw.githubusercontent.com/diyhue/diyhue.github.io/master/assets/images/hue-map.png
