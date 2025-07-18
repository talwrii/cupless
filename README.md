# cupless
**@readwithai** - [X](https://x.com/readwithai) - [blog](https://readwithai.substack.com/) - [machine-aided reading](https://www.reddit.com/r/machineAidedReading/) - [📖](https://readwithai.substack.com/p/what-is-reading-broadly-defined
)[⚡️](https://readwithai.substack.com/s/technical-miscellany)[🖋️](https://readwithai.substack.com/p/note-taking-with-obsidian-much-of)

Printing on Linux without CUPS.

![logo](logo-small.png)

## Motivation
I don't like [CUPS](https://openprinting.github.io/cups/). It's this hidden daemon, that uns in the background, gets in the way of printing and can go wrong and is then difficult to debug. We live in the future with standard file formats ([PWG](https://openprinting.github.io/driverless/01-standards-and-their-pdls/#pwg-raster-format)) and standardized printer protocols ([IPP](https://en.wikipedia.org/wiki/Internet_Printing_Protocol)): we should be able to print directly on the command-line without talking to a daemon  see the errors right there, rather than debugging through a layers of queues with obscure commands of web interfaces. And yet, I couldn't find the tools to do this, so I hacked up my own. 

Also, I would quite like to be able to "just print" images nad have them be scaled etc, without having to do the scaling myself.

## Installation
This tool depends on [mutool](https://www.mankier.com/1/mutool), [ipptool](https://www.cups.org/doc/man-ipptool.html), and [pdfjam](https://github.com/pdfjam/pdfjam). Importantly, all of these can be installed with apt on ubuntu with `sudo apt install mupdf-tools texlive cups-ipp-utils` (texlive supplies `pdfjam`)

Once you have installed these you can install this with  [pipx](https://github.com/pypa/pipx).

```
pipx install cupless
```

## Usage
`cupless --ipp $ULRL file` will print a file to a printer using IPP. To discover the IPP URI for your printer (which looks something like ipp://printer/ipp/print you can use a tool like `ippfind` which queries printers on the local network or review your printers documentation). Your printer will probably have an [mdns / bounjour / avahi](https://en.wikipedia.org/wiki/Multicast_DNS) domain name ending in `.local` which works on linux with no configuration.

Once you have found your printers URI you can put this in `~/.config/cupless.ini` like so:

```
[printer]
uri=ipp://printer/ipp/print
```

You can then print with `cupless file`.


Images will be scaled to fit the page. At the moment, only single page files are supported - I imagine i will fix this as soon as I have to print something with multiple pages.

## Some technical details
`ipp` is an HTTP-based standard that most printers support that allows files to be sent to the printer and printed. Unfortunately, likely to make printers simpler, standard formats such as PDF or PNG are not supported - rather than either niche (PWF, URF) or proprietary or lossy (JPEG) formats.

This tool is a wrapper around `mutool` and `ipp` which takes a PDF converts it into a niche format and sends it to a printe.

## Alternatives and prior work
Obviously there has been a bunch of work on standardizing printers. This tool makes use of [mutool](https://www.mankier.com/1/mutool) for converting PDF. Attah wrote a tool called [ippclient](https://github.com/attah/ppm2pwg) (and an alternative to mutool) which I think performs a similar role to this tool - but there is no automatic installation process for this tool.

## Caveats
I didn't want to depend upon `ipptools` as this is maintained by cups (though it is a separate package). However, I had difficulty getting the print command to work so gave up and used iptools. If I feel inspired (of if you do and feel like submitting patches) I might remove this. `pdfjam` depends on latex and texlive - which is quite big - however it is quite good at resizing images.

This only works with printers that have a default resolution of 300dpi.

This only supports internet printers connected to your network which support IPP - so we **do not support usb printers**.

This tool does not support printer discoverty - instead you can use `ippfind` directly.

At the moment this tool only suppors printing single page documents.

## About me
I am **@readwithai**. I create tools for reading, research and agency sometimes using the markdown editor [Obsidian](https://readwithai.substack.com/p/what-exactly-is-obsidian).

I also create a [stream of tools](https://readwithai.substack.com/p/my-productivity-tools) that are related to carrying out my work.

I write about lots of things - including tools like this - on [X](https://x.com/readwithai).
My [blog](https://readwithai.substack.com/) is more about reading and research and agency.
