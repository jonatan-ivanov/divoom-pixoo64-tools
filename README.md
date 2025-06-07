# Divoom Pixoo 64 Tools

Tools to interact with the Divoom Pixoo 64 pixel art display.

## Usage Examples

### Help and Version info

```sh
p64 --help
p64 --version
```

### Get List of Devices

```sh
p64 devices
```

If `CACHE_FILE` (`.cache/.devices`) does not exists, this will call Divoom's Find Device API (you need internet access). If you want to avoid that API call, see how to use the device cache as a [Config File](#config-file).

### Generate Devices Cache

```sh
p64 devices-cache
```

This will always call Divoom's Find Device API (you need internet access). If you want to avoid that API call, but you want a cache, see how to create one manually using a [Config File](#config-file) as the cache.

### Get the Device IP

```sh
p64 device-ip
p64 device-ip --name 'Pixoo64'
```

Uses the `CACHE_FILE` if it exists, otherwise Divoom's Find Device API (you need internet access) will be called.

### Get Health

Checks if it can connect to the device.

```sh
p64 health
p64 health --name 'Pixoo64'
p64 health --ip '192.168.0.100'
```

If the check was successful, it exits with `0` and no output, otherwise you get a non-zero error code and an error message on `stderr`.
If IP is not specified, this uses `device-ip` (see above, please notice the note about the `CACHE_FILE`).

### Reboot

[Reboot](https://docin.divoom-gz.com/web/#/5/26) the device.

```sh
p64 reboot
p64 reboot --name 'Pixoo64'
p64 reboot --ip '192.168.0.100'
```

If the command is successful, it will display the JSON response from the device, otherwise you will get a non-zero error code and an error message on `stderr`.
If IP is not specified, this uses `device-ip` (see above, please notice the note about the `CACHE_FILE`).

### Get Clock Info

Asks the [Clock Info](https://docin.divoom-gz.com/web/#/5/30) from the device.

```sh
p64 clock-info
p64 clock-info --name 'Pixoo64'
p64 clock-info --ip '192.168.0.100'
```

If the command is successful, it will display the JSON response from the device, otherwise you will get a non-zero error code and an error message on `stderr`.
If IP is not specified, this uses `device-ip` (see above, please notice the note about the `CACHE_FILE`).

### Draw Image

Asks the Device to [display the specified image](https://docin.divoom-gz.com/web/#/5/57).
The image will be loaded and cropped to 64x64 before sending. Image type does not matter as long as imagemagick supports it.

```sh
p64 draw-image test.png
p64 draw-image test.png --name 'Pixoo64'
p64 draw-image test.png --ip '192.168.0.100'
```

If the command is successful, it will display the JSON response from the device, otherwise you will get a non-zero error code and an error message on `stderr`.
If IP is not specified, this uses `device-ip` (see above, please notice the note about the `CACHE_FILE`).

### Reset PicID

Asks the device to [Reset PicID](https://docin.divoom-gz.com/web/#/5/56). Should not be needed, it resets internal picture counter of the device.

```sh
p64 reset-pic-id
p64 reset-pic-id --name 'Pixoo64'
p64 reset-pic-id --ip '192.168.0.100'
```

If the command is successful, it will display the JSON response from the device, otherwise you will get a non-zero error code and an error message on `stderr`.
If IP is not specified, this uses `device-ip` (see above, please notice the note about the `CACHE_FILE`).


## Dependencies

- [`curl`](https://curl.se/)
- [`jq`](https://jqlang.org/)
- [imagemagick (`magick`)](https://imagemagick.org/)
- [GNU `getopt`](https://github.com/util-linux/util-linux)

macOS provides BSD getopt instead of GNU getopt, you need to install it:
```sh
brew install gnu-getopt
# Then add this to your .zshrc
export PATH="/opt/homebrew/opt/gnu-getopt/bin:$PATH"
```

## Config File

You can use the `CACHE_FILE` as a config file, if you put something like this to `.cache/.devices` so that you don't need to call Divoom's Find Device API nor specify the IP address every time:
```json
{
  "DeviceList": [
    {
      "DeviceName": "Pixoo64",
      "DevicePrivateIP": "192.168.0.100"
    }
  ]
}
```

## Store your favorite images

If you use the script by cloning this git repo, since the `.cache` folder is in `.gitignore`, you can use that folder to save images you frequently use, for example:
```sh
p64 draw-image .cache/test.png
```
