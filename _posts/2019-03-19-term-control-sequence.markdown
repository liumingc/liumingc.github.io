---
layout: post
title:  "Terminal control sequence"
date:   2019-03-19 21:31:50 +0800
categories: jekyll update
---

Output to terminal with color is attractive, so i read the wiki, and take some
notes here. Currently, i only want to record the most basic functions.
[wiki](https://en.wikipedia.org/wiki/ANSI_escape_code)

A sequence starts with `ESC`, (or ^[, 27/\x1b/\033), follow a second byte, and may
be more bytes.

| ESC c | RIS - Reset to initial state |
| ESC [ | CSI - Control Sequence Introducer |
| ESC P | DCS - Device Control String |

# CSI

| Code | Name | Effect |
| CSI n A | Cursor up |  |
| CSI n B | Cursor down |  |
| CSI n C | Cursor forward |  |
| CSI n D | Cursor back |  |
| CSI n E | Cursor next line |  |
| CSI n F | Cursor previous line |  |
| CSI n G | Cursor horizontal absolute(default n=1, to column n) |
| CSI n ; m H | Cursor position | Move the cursor to row n, column m |
| CSI n J | Erase in display | n=0, clear to end; n=1, clear to beginning; n=2, clear entire screen |
| CSI n K | Erase in line | |
| CSI n S | Scroll up | |
| CSI n T | Scroll down | |
| CSI n m | SGR - Select graphic rendition | See below |

## SGR parameters

| Code | Effect |
| 0 | Reset |
| 1 | Bold |
| 2 | Light |
| 3 | Italic |
| 4 | Underline |
| 5 | Slow blink |
| 6 | Rapid blink |
| 7 | Reverse video |
| 10-19 | Select font |
| 22 | Normal color or intensitiy |
| 24 | Underline off |
| 25 | Blink off |
| 27 | Inverse off |
| 30-37 | Set foreground color |
| 38 | Set foreground color |
| 40-47 | Set background color |
| 48 | Set background color |

Can specify multiple parameters, seperated by `;`, So you can use bold color,
set fg/bg color like `CSI 1;31;45m`.
Color table is

| Name | Num |
| Black | 0 |
| Red | 1 |
| Green | 2 |
| Yellow | 3 |
| Blue | 4 |
| Magenta | 5 |
| Cyan | 6 |
| White | 7 |


| 
