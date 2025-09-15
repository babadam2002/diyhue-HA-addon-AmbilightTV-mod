<div align="center">
<img src="https://github.com/diyhue/hassio-addon/blob/master/images/diyhue-hassio.png">
<h1>Home Assistant Add-on: diyHue</h1>
<br>
<p>Run <a href="">diyHue</a> as a Home Assistant Add-on</p>
<a href="https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fdiyhue%2Fhassio-addon"><img src="https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg"></a>
</div>

## About

diyHue provides a Ecosystem for several Smart Home Solutions, eliminating the need for vendor specific Bridges and Hardware. Written in Python and Open Source, you are now able to import and control all your Lights and Sensors into one System. Lightweight and resource friendly, to run on small
devices like the RPi .... 24/7

The Best part? No Cloud connection by Design!

Enjoy your diyHue enlighted Home.

If you've found the Add-on helpful or useful, then please consider throwing a coffee my way to help support my work. As i am a student and would like to invest more time and effort in this project this would really help me:

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/diyhue)

![diyHue ecosystem][img-ecosystem]

## Installation

The installation process is pretty easy and straight forward, like for any other third-party Home Assistang Add-on.

Add the repository URL under **Supervisor → Add-on store → ⋮ → Manage add-on repositories**:

    https://github.com/diyhue/hassio-addon


## Options

- `min_brightness_threshold`: Black level for AmbilightTV + Hue. Minimum 35 recommended. AmbilightTv have one problem, that the backgound minimum light is always on, this will turn it off.
- `Entertainment_Area_Extender_udp_ip`: IP address for the plugin. Usually your HA IP without port. You need to install the Entertainment Area Extender addon  for diyhue-HA-addon-AmbilightTV-mod addon.

- `Entertainment_Area_Extender_udp_port`: UDP port. Default is 12345.

## Usage

- Set these options in the Home Assistant addon UI under "Configuration".

## Contribute

diyHue is Opensource and maintained by volunteers in their free time. You are welcome to contribute and become a recognised member of the diyHue community. Feel free to add PR and Commits to our Dev Branch. If you are experienced in

-    Webdesign
-    Python
-    Arduino
-    Coding in general

We highly appreciate your support, making diyHue even better!

## Credits

- The Addon code was originally written by Max ([@Maxbec](https://github.com/maxbec)) and has been adopted into the diyhue project officially. Thank you very much Max for your contribution!

-    Ben ([@cheesemarathon](https://github.com/cheesemarathon)) All fancy github integrations
-    [Stephan van Rooij](https://github.com/svrooij) - zigbee2mqtt integration
-    [@avinashraja98](https://github.com/avinashraja98) - Hue Entertainment server
-    Federico Zivolo ([@FezVrasta](https://github.com/FezVrasta)) Internal WebGUI
-    [@J3n50m4t](https://github.com/J3n50m4t) - Yeelight integration
-    Martin Černý ([@mcer12](https://github.com/mcer12)) - Yeelight color bulb
-    probonopd https://github.com/probonopd/ESP8266HueEmulator
-    sidoh https://github.com/sidoh/esp8266_milight_hub
-    StefanBruens https://github.com/StefanBruens/ESP8266_new_pwm
-    Cédric @ticed35 for linkbutton implementation
-    [@cheesemarathon](https://github.com/cheesemarathon) - Help with Docker images
-    [@Mevel](https://github.com/Mevel) - 433Mhz devices
-    [@Nikfinn99](https://github.com/Nikfinn99) - PCB designs
-    [@crankyoldgit](https://github.com/crankyoldgit) - IR Remote library

## Additional Projects and Ideas

Hue living color light project for 3D printing: [Thingiverse 2773413](https://www.thingiverse.com/thing:2773413)

## License

[![license](https://img.shields.io/badge/license-GPLv3%2FApache%202.0%2FCC%20BY--SA%204.0-blue.svg)](https://github.com/diyhue/diyHue/blob/master/LICENSE.md)

[changelog]: https://github.com/MaxBec/hassio-diyHue/blob/master/diyhue/CHANGELOG.md
[img-ecosystem]: https://raw.githubusercontent.com/diyhue/diyhue.github.io/master/assets/images/hue-map.png
