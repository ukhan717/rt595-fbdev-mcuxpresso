# rt595-fbdev-mcuxpresso

The NXP `fbdev_freertos` example for the EVK-MIMXRT595, exported from MCUXpresso
IDE as a standalone project and pointed at a MIPI DSI panel.

The example moves a rectangle around the screen through the fbdev component,
which hides the differences between LCD controllers behind one API and owns the
frame buffers.

## What is ours

One line. `board/display_support.h` defaults `DEMO_PANEL` to
`DEMO_PANEL_RK055AHD091` instead of NXP's `DEMO_PANEL_TFT_PROTO_5`, because this
board drives an RK055HDMIPI4M over MIPI rather than the MikroE FlexIO display.

Everything else matches MCUXpresso SDK 2.13 for the EVK-MIMXRT595.
`board/display_support.c` in particular is stock.

## Unlike the other two, this one is self-contained

An MCUXpresso IDE export bundles what it needs, so `CMSIS/`, `freertos/`,
`drivers/` and `device/` are all in the tree. There is no separate SDK to unpack.

    .cproject .project    MCUXpresso IDE project
    *.mex                 pin and clock configuration for Config Tools
    CMSIS/                CMSIS core headers
    device/ startup/      MIMXRT595S headers, linker script, vector table
    drivers/              fsl_* peripheral drivers
    board/                board init, pin mux, clocks, display_support
    video/ lcdc/          MIPI panel drivers and LCD controller glue
    component/ utilities/ serial manager, debug console
    freertos/             kernel and heap_4
    flash_config/         FlexSPI config block
    source/               the application
    doc/readme.txt        NXP's description of the example

## Building

Import into MCUXpresso IDE as an existing project and build. There are no
armgcc, IAR or MDK projects here.

## Choosing a panel

`DEMO_PANEL` near the top of `board/display_support.h`:

| Value | Panel |
|---|---|
| `DEMO_PANEL_TFT_PROTO_5` | MikroE TFT Proto 5", FlexIO 8080 |
| `DEMO_PANEL_RK055AHD091` | RK055HDMIPI4M, 720x1280 MIPI, default here |
| `DEMO_PANEL_RK055IQH091` | 540x960 MIPI |
| `DEMO_PANEL_RM67162` | G1120B0MIPI circular smart panel |
| `DEMO_PANEL_RK055MHD091` | RK055MHD091A0-CTG, 720x1280 MIPI |

## Licence

NXP's. See `LA_OPT_NXP_Software_License.txt` and `COPYING-BSD-3` in the SDK
archive.
