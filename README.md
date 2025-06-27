# Corne/Chocofi Keyboard Firmware (with custom art)

This is the firmware source for both the **Corne** (6x3) and **Chocofi** (5x3) keyboard, a split ergonomic keyboard powered by an nice!nano microcontroller. It features **mouse movement and scrolling support** and **Nice!View OLED** for status display with custom images.

---

## Features

- ✅ Split ergonomic layout (chocofi or corne style)
- ✅ Nice!nano MCU (compatible with ZMK)
- ✅ Nice!view OLED support (with custom art)
- ✅ Bluetooth Low Energy (BLE) supported
- ✅ Mouse movement supported


---

## Local build commands:

```bash
cd ~/zmk/app/ && workon zmk
```
```bash
west build -d build/left -p -b nice_nano_v2 -- -DSHIELD="corne_left nice_view_adapter nice_view"
```
```bash
west build -d build/right -p -b nice_nano_v2 -- -DSHIELD="corne_right nice_view_adapter nice_view"
```

---

## Getting Started


## Custom Art on Nice!View Displays

In the latest version of ZMK you can load custom 1-bit art to display on the right keyboard half. The below steps assume you're comfortable building ZMK from source. The steps are as follows:
- First, get the latest ZMK code using git pull
- Find a piece of 1-bit art to use or make your own and scale to a resolution of 68x140.
- Rotate the art 90 degrees clockwise and save as a monochrome bitmap.
- Load the bitmap into the online [LVGL Image Converter](https://lvgl.io/tools/imageconverter) using color format of CF_INDEXED_1BIT and outputting as a C array.
- Open the resulting C file and copy everything AFTER the ```#ifndef LV_ATTRIBUTE_MEM_ALIGN``` block.
- Paste code you copied into the bottom of ```app\boards\shields\nice_view\widgets\art.c```
- Find ```<file name>_map[] = {``` and replace the two color index lines with the following copied from the "mountain" or "balloon" images where ```"<file name>"``` is the name you put into the Image Converter:
```c
#if CONFIG_NICE_VIEW_WIDGET_INVERTED
        0xff, 0xff, 0xff, 0xff, /*Color of index 0*/
        0x00, 0x00, 0x00, 0xff, /*Color of index 1*/
#else
        0x00, 0x00, 0x00, 0xff, /*Color of index 0*/
        0xff, 0xff, 0xff, 0xff, /*Color of index 1*/
#endif
```
- Open the file ```app\boards\shields\nice_view\widgets\peripheral_status.c``` and add a line for your new art like ```LV_IMG_DECLARE(<file name>);``` near the top.
- In the same file, search for ```lv_img_set_src(art, random ? &balloon : &mountain);``` and edit to be ```lv_img_set_src(art, &<file name>);```
- Now rebuild and reflash the firmware.

### ✅ Requirements

- [ZMK Github](https://github.com/zmkfirmware/zmk)
- [Zephyr SDK](https://docs.zephyrproject.org/latest/develop/toolchains/zephyr_sdk.html#zephyr-sdk-installation)
- [ZMK Dev Setup](https://zmk.dev/docs/development/setup)
- [ZMK Building and Flashing](https://zmk.dev/docs/development/build-flash)
- [Nice!view Getting Started](https://nicekeyboards.com/docs/nice-view/getting-started/)



### ✅ References
- [ZMK CLI](https://zmk.dev/docs/keymaps)

