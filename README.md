# ged2dot, a GEDCOM to Graphviz converter

[![tests](https://github.com/vmiklos/ged2dot/workflows/tests/badge.svg)](https://github.com/vmiklos/ged2dot/actions")

The latest version is v7.4, released on 2022-08-01.  See the
[release notes](https://github.com/vmiklos/ged2dot/blob/master/NEWS.md).

## What's the problem the tool solves?

`ged2dot` is a script that takes a [GEDCOM](http://en.wikipedia.org/wiki/GEDCOM) file and tries to
visualize it using [Graphviz](http://www.graphviz.org/)'s `dot` tool. The basic idea is that you can
map individuals and families to graph nodes and connections between them to graph edges, then `dot`
takes care of the rest. What's unique about `ged2dot` is that it allows more than showing ancestors
and descendants of a single individual (what you can easily do with random family editor software).

You usually want to customize your input filename, output filenames, the root family and the depth
of the graph visitor. You can provide these either by using command-line arguments (see `ged2dot.py
--help`) or by using a configuration file (see `ged2dotrc.sample`). When using both, the
command-line arguments overwrite configuration values.

## Layout

The layout does a Breadth First Search (BFS) traversal on the graph, from the starting family:

![screenshot](https://vmiklos.hu/ged2dot/tests/screenshot.png)

This has several benefits over explicitly trying to guess which family belongs to which generation.
Some example more tricky cases, which are handled by `ged2dot`:

- root family -> husband -> sister -> showing her kids

- root family -> wife -> cousin -> showing her kid

- root family -> husband -> grand father -> showing both wives with the matching kids

- marrying cousins

(ged2dot <= 7.0 allowed multiple layouts, none of them supported the above more tricky cases.)

GEDCOM files don't contain images, but you can put images next to the GEDCOM file, and in that case
ged2dot will try to pick them up when generating `dot` output. The expected location is
`images/Given Family 1234.jpg`, relative to the GEDCOM file. For example, there is a person called
Ray Smith in the above screenshot. The birth year string is `Y`, so the image location has to be
`images/Ray Smith Y.jpg`.

## Qt-based GUI

The `qged2dot.py` script is a Qt-based GUI for `ged2dot`, which can turn the `dot` output into PNG
files.

For macOS, the DMG is not signed digitally, so you need to allow its usage
[explicitly](https://support.apple.com/guide/mac-help/open-a-mac-app-from-an-unidentified-developer-mh40616/mac).

The installer bundles Graphviz for macOS and Windows.

The app icon is by [Appzgear](https://icon-icons.com/icon/family-tree/120659).

## LibreOffice Draw GEDCOM import filter

The `libreoffice/` subdirectory contains a LibreOffice extension, that
implements a GEDCOM import filter for Draw. Needless to say, it uses `ged2dot`
internally -- think of it as a GUI for `ged2dot`, with the additional benefit
that you can hand-edit the resulting layout in Draw, if you want.

Its dependencies:

- It uses Graphviz to process the `dot` format. In case you don't have Graphviz
  installed:

  - For Windows, [get it here](https://graphviz.gitlab.io/_pages/Download/Download_windows.html) (2.38 is tested).

  - For Linux, use your package manager to install the `graphviz` package (2.28 is tested).

  - For macOS, install it from [brew](https://brew.sh/) (2.36 is tested).

- LibreOffice >= 7.2

Features:

- Filter detection: you can use File -> Open and select a GEDCOM file, and
  it'll be opened in Draw automatically.
- Import options: On import, a graphical dialog can be used to set a subset of
  the options available in a `ged2dotrc`.
- Internally reuses the excellent SVG import filter of LibreOffice, contributed
  by Fridrich Strba and Thorsten Behrens, so the result can be manually
  fine-tuned if necessary.
- Runs on Windows and Linux and macOS.

You can grap a release binary at [the releases page](https://github.com/vmiklos/ged2dot/releases) --
more on how to to install a LibreOffice extension
[here](https://wiki.documentfoundation.org/Documentation/HowTo/install_extension).

NOTE: Linux distributions install Python support separately, be sure to install the
`libreoffice-script-provider-python` (deb) or `libreoffice-pyuno` (rpm) packages before the OXT
file.

Once that's done, you'll see something like this if you open a GEDCOM file:

![screenshot](https://vmiklos.hu/ged2dot/libreoffice/screenshot.png)

## Bugs

For `ged2dot`, in case a given input results in a runtime crash, it's
considered a bug. If you have a fix for it,
[pull requests](https://github.com/vmiklos/ged2dot/pull/new/master) on GitHub are
welcome. Make sure to run `make check` before submitting your changes.

For the LibreOffice extension, in case you get an error during opening:

- For Windows, the log file location is something like:

```
C:/Users/John/Application Data/LibreOffice/4/user/Scripts/python/log.txt
```

- For Linux, start LibreOffice from a terminal, the log is printed to the
  standard error.

- For Mac, start LibreOffice from Terminal:

```
cd /Applications/LibreOffice.app/Contents/program
./soffice --nologo /path/to/test.ged
```

then the log is printed to the standard error as well.

## Icons

Icons are from
[WPZOOM](http://www.wpzoom.com/wpzoom/new-freebie-wpzoom-developer-icon-set-154-free-icons/),
in case placeholders have to be used for missing images.

## Resources

- [GEDCOM specification](https://www.familysearch.org/developers/docs/guides/gedcom)
- [Translation of ged2dot to JavaScript](https://gist.github.com/fetsorn/1d5f6cbc47989b32cb461528c1e253b4)

## Download

From [GitHub](https://github.com/vmiklos/ged2dot).