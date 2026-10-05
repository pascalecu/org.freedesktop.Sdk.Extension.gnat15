<!--
  SPDX-FileCopyrightText: 2025 Junde Yhi <junde@yhi.moe>
  SPDX-License-Identifier: CC0-1.0
-->

# Freedesktop SDK Extension - GNAT 15

This repository contains the files needed to package the GNU Ada/SPARK 15.x toolchain as a Freedesktop SDK extension (`org.freedesktop.Sdk.Extension.gnat15`) for Freedesktop SDK 25.08.

Included components are:

- [GNAT]: the GNU [Ada] compiler
- [GNATprove]: the GNU [SPARK] analyzer
- [GPRbuild]: the GNAT Project Manager, with companion tools
- [Alire] (`alr`): the Ada Library Repository tool

The components are repackaged from pre-built binaries published by:

- [alire-project/GNAT-FSF-builds](https://github.com/alire-project/GNAT-FSF-builds/releases)
- [alire-project/alire](https://github.com/alire-project/alire/releases)

[GNAT]: https://gcc.gnu.org/onlinedocs/gnat_ugn/
[Ada]: https://www.adacore.com/about-ada
[GNATprove]: https://github.com/AdaCore/spark2014
[SPARK]: https://www.adacore.com/about-spark
[GPRbuild]: https://github.com/AdaCore/gprbuild
[Alire]: https://alire.ada.dev/

## Use

Flatpak mounts installed SDK extensions under `/usr/lib/sdk/${sdk_name}`. SDK extensions are not automatically added to the environment, so the provided enable script must be sourced when using the SDK directly:

```sh
. /usr/lib/sdk/gnat15/enable.sh
```

For applications using Freedesktop SDK as their runtime and following the [ide-flatpak-wrapper] convention, such as Flatpak builds of VSCodium or Visual Studio Code, set `FLATPAK_ENABLE_SDK_EXT` to enable the extension.

For a single launch:

```sh
FLATPAK_ENABLE_SDK_EXT=gnat15 flatpak run com.vscodium.codium
```

To enable it permanently for an application:

```sh
flatpak override --user \
  --env=FLATPAK_ENABLE_SDK_EXT=gnat15 \
  com.vscodium.codium
```

Multiple SDK extensions can be enabled using a comma-separated list. `*` can be used to enable all available SDK extensions.

To use this extension when building another Flatpak application, add it to `sdk-extensions` and source the enable script during the build:

```yaml
sdk: org.freedesktop.Sdk
sdk-extensions:
  - org.freedesktop.Sdk.Extension.gnat15

modules:
  - name: enable-gnat15
    buildsystem: simple
    build-commands:
      - . /usr/lib/sdk/gnat15/enable.sh
```

[Flatseal]: https://flathub.org/apps/com.github.tchx84.Flatseal
[ide-flatpak-wrapper]: https://github.com/flathub-infra/ide-flatpak-wrapper

## Build

Run `flatpak-builder`, or `org.flatpak.Builder` from Flatpak, with the manifest:

```sh
flatpak run org.flatpak.Builder \
  --force-clean \
  --sandbox \
  --user \
  --install \
  --install-deps-from=flathub \
  --ccache \
  --mirror-screenshots-url=https://dl.flathub.org/media/ \
  --repo=repo \
  build \
  org.freedesktop.Sdk.Extension.gnat15.yml
```

When building inside a container environment where FUSE mounts are unavailable, such as some Toolbx setups, use the host `flatpak-builder` or disable `rofiles-fuse` if necessary.

## Components

The current extension contains:

- GNAT 15.3.0-1
- GNATprove 15.1.0-1
- GPRbuild 26.0.0-1
- Alire 2.1.1

The extension itself remains a GNAT 15 extension; the companion tools use their own independent versioning schemes.

## License

The metadata files included in this repository are public domain work under the CC0 1.0 license. See [LICENSE](./LICENSE) for a copy of the license text.