# Fluidd-Belt

Fluidd-Belt is a free and open-source Klipper web interface for managing your Conveyor Belt Style 3d printer.

## Using this on a normal printer will VOID your warranty! This is a HARD FORK!

This modified version will add the ability to change, view, and save the Y Axis offset for the belt printer, since Z Axis offset is not needed.

Add these to your printer.cfg file:

``` Macros
[save_variables]
filename: ~/printer_data/config/variables.cfg

[gcode_macro Y_OFFSET_SAVE_CONFIG]
description: Save current Y offset and restart Klipper
gcode:
    {% set y_offset = printer.gcode_move.homing_origin[1] %}
    SAVE_VARIABLE VARIABLE=saved_y_offset VALUE={y_offset}
    { action_respond_info("Y offset %.3fmm will be saved. Klipper will restart..." % y_offset) }
    SAVE_CONFIG

[delayed_gcode RESTORE_Y_OFFSET]
initial_duration: 2.0
gcode:
    {% set svv = printer.save_variables.variables %}
    {% if 'saved_y_offset' in svv %}
        {% set saved_offset = svv.saved_y_offset %}
        SET_GCODE_OFFSET Y={saved_offset} MOVE=0
        { action_respond_info("Restored Y offset: %.3fmm" % saved_offset) }
    {% endif %}
```
Upload the modified Fluidd files to your printer.

<img width="917" height="303" alt="Screenshot 2025-11-02 17 24 06" src="https://github.com/user-attachments/assets/eb1382be-6ba2-456f-a5a4-2b75a2dbadbb" />

You can find the latest release of the Fluidd-Belt here: [Fluidd-Belt](https://github.com/xboxhacker/fluidd-belt/releases/tag/v1.34.4_belt)

# !!Updating Fluidd from the main-core branch will break these Y Offset modifications!!
---

![Fluidd](/docs/assets/images/preview_sliced.png "Fluidd")

## Features

- Responsive UI, supports desktop, tablets and mobile
- Customizable layouts. Move any panel where YOU want
- Built-in color themes
- Manage multiple printers from one Fluidd install
- [See our docs for more!](https://docs.fluidd.xyz)

## Support & Documentation

See our [Docs](https://docs.fluidd.xyz).
Join our [Discord!](https://discord.gg/GZ3D5tqfcF).

## How to use?

Fluidd can be easily installed via [KIAUH](https://github.com/dw-0/kiauh), along with Klipper, Moonraker, and all of the required dependencies.

Please see the [docs](https://docs.fluidd.xyz) for help with installation and configuration.

## Where to download?

You can download the latest release [here](https://github.com/fluidd-core/fluidd/releases/latest).

Older releases can be found [here](https://github.com/fluidd-core/fluidd/releases).

## Docker support

We have an [official docker image](https://github.com/fluidd-core/fluidd/pkgs/container/fluidd), serving Fluidd by default on port 80.

For those who have specific security requirements and need/want to run an unprivileged container, we also have an [unprivileged docker image](https://github.com/fluidd-core/fluidd/pkgs/container/fluidd-unprivileged) available, serving Fluidd by default on port 8080.

You can override the default port where Fluidd will be served by setting the `PORT` environment variable when starting the docker container.

Both of these docker images are updated for each release and on each commit.

## Official sponsors

[![LDO](/docs/assets/images/logo_ldo.svg "LDO")](https://ldomotors.com/)

LDO, Excellence in Motion. LDO is an official sponsor of Fluidd.

## Supporting Fluidd

Fluidd development is driven by passionate volunteers who dedicate their time to improving and expanding its capabilities.

Your sponsorship can help us enhance Fluidd, introduce new features, and ensure it remains accessible to all Klipper users.

Your support can make a significant impact on the evolution of Fluidd. Please consider [sponsoring Fluidd](https://github.com/sponsors/fluidd-core).

## Credits

A big thank you to:

- the [Voron Community](http://vorondesign.com/)
- Kevin O'Connor for [Klipper](https://github.com/Klipper3d/klipper)
- Eric Callahan for [Moonraker](https://github.com/Arksine/moonraker)
- Dominik Willner for [KIAUH](https://github.com/dw-0/kiauh)
- Ray for [MainsailOS](https://github.com/raymondh2/MainsailOS)

## Misc

This project is tested with BrowserStack
