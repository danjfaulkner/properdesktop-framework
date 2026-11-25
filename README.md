# Proper Desktop Framework

Shared system APIs for Proper Desktop components and applications, on the
[properdesktop-toolkit](https://github.com/danjfaulkner/properdesktop-toolkit) base
library:

- `clients/`: clients of system services (accounts, appearance, audio,
  BlueZ, display, NetworkManager, power profiles, time and date, UPower), one
  library each, in the `ProperDesktopClients` package.
- `files/`: file listing, MIME types, operations, trash, volumes and
  thumbnails, in the `ProperDesktopFiles` package.
- `pdf/`: PDF documents (pages, rendering, text, search, links, outline)
  read with [PDFium](https://pdfium.googlesource.com/pdfium/), in the
  `ProperDesktopPdf` package.

## Dependencies

### Debian/Ubuntu

```shell
sudo apt install cmake pkg-config build-essential git curl \
    libicu-dev libpng-dev \
    libpipewire-0.3-dev libspa-0.2-dev libgtest-dev \
    libfreetype-dev libharfbuzz-dev liblcms2-dev libopenjp2-7-dev libjpeg-dev zlib1g-dev \
    libhwy-dev libabsl-dev
```

### Arch Linux

```shell
sudo pacman -S --needed base-devel cmake pkgconf git curl \
    icu libpng pipewire gtest \
    freetype2 harfbuzz lcms2 openjpeg2 libjpeg-turbo zlib highway abseil-cpp
```

and [properdesktop-toolkit](https://github.com/danjfaulkner/properdesktop-toolkit).

## Build and Install

PDFium is fetched at a pinned revision and built into `properdesktop-pdf`:

```shell
scripts/fetch-pdfium.sh
mkdir build
cd build
cmake ..
make
sudo make install
```

## License

BSD-3-Clause; see [LICENSE](LICENSE). PDFium, built into `properdesktop-pdf`, is
under its own BSD-style licence, in `third_party/pdfium/LICENSE` once
fetched; its bundled AGG 2.3 under the notices in its sources.
