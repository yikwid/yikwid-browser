# Yikwid Browser Source Repository

This repository contains all the patches and theming that make up Yikwid Browser, as well as scripts and a Makefile to build it. There also is the [Settings repository](https://github.com/yikwid/yikwid-browser-settings), which contains the browser preferences.

## Yikwid Browser build instructions

The Yikwid Browser is built from source using the provided `ybb` script (Yikwid Browser Builder), but commands are availble via `make` for convenience. Run `make help` (or `./ybb help`) for more info.

### Building from Source

First, clone this repository with Git:

```bash
git clone https://github.com/yikwid/yikwid-browser.git yikwid-browser
cd yikwid-browser
```

Then you have to bootstrap your system to be able to build it. You only have to do this one time. It is done by running the following commands:

```bash
# Dependencies
sudo apt install python3 python3-dev python3-pip bash findutils gzip libxml2 m4 make perl tar unzip watchman
# Init dev environment
make init
```

Finally you can build the browser and then package or run it with the following commands:
```bash
# Build browser
make build
# Run the build
make run
# Package the build
make package
# Build dist
make dist-deb
```

## Development Notes

More info to come later.

