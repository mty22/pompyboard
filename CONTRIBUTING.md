# Contribution Guide

> [!CAUTION]
> This page is made for contributors building pompyboard.
> From this point onward we assume you know what you are doing.

## Project overview

The pompyboard project can be roughly broken down into 6 parts:

| Part           | Location                                       |
| -------------- | ---------------------------------------------- |
| PCB            | [`./pcb/`](./pcb/)                             |
| Firmware       | [`./firmware/`](./firmware/)                   |
| 3D model       | [`./3d/`](./3d/)                               |
| Website        | [`./web/`](./web/)                             |
| Infrastructure | [`./infra/`](./infra/)                         |
| Driver         | [pompyboard/OpenTabletDriver][otd-fork] (fork) |

## Setting up development environment

1. Clone this git repository
2. Open `README.md` file of the sub-project you want to work on
   (e.g. [`./firmware/README.md`](./firmware/README.md)) and go from there.

## Contribution standards

### Communication, communication, communication

Remember, people can't read your mind. If you did something amazing and you want
others to care about it, explain your motivation using fact and logic so anyone
with three digit IQ can understand. You don't necessarily have to dumb things
down. Just be clear about it.

### Formatting

It is expected that all text-based files are formatted using the correct
formatter before getting committed. See
[`./.vscode/settings.json`](./.vscode/settings.json) for more info.

### line length

Keep your text under 80 characters and rust code under 100 characters per line
unless absolutely necessary. If you are using vscode, a ruler should help you
know when you are over that limit.

### Commit

Be specific and concise. Prepend scope if necessary.

Examples:

- `remove unused files`
- `firmware: add more test cases for functionName`
- `web: bump packageName version from v6 to v9`

<!-- Links -->

[otd-fork]: https://github.com/pompyboard/OpenTabletDriver
