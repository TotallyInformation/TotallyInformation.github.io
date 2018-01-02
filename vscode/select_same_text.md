---
title: VSCode - select all of the same text
description: >
    If you want to quickly change all of the same text in a document, you can use the Ctrl+F2 shortcut.
comments: true
---

Select the text or code that you want to change. VSCode highlights all matching text. Press <kbd>Ctrl+F2</kbd> and type your new text.

VSCode supports multi-cursor editing which this tip makes use of.

### Multi-cursor mouse shortcuts

* <kbd>Alt</kbd> and click will add additional cursors. (Note that some Linux distributions may map a system function to <kbd>Alt+LeftMouse</kbd> which may interfere with this shortcut).
* <kbd>Shift+Alt</kbd> and drag does a box select with cursors on every line selected.

### Multi-cursor keyboard shortcuts

* <kbd>Ctrl+Alt+Up</kbd>, <kbd>Ctrl+Alt+Down</kbd> extends multiple cursors upwards and downwards from the previously selected cursors. Note that using both up and down after each other extends around the previous cursors.
* <kbd>Ctrl+D</kbd> selects next occurrence of word under cursor or of the current selection
* <kbd>Ctrl+K</kbd>, <kbd>Ctrl+D</kbd> moves last added cursor to next occurrence of word under cursor or of the current selection
  The commands use matchCase by default. If the find widget is open, then the find widget settings (matchCase / matchWholeWord) will be used for determining the next occurrence
* <kbd>Ctrl+U</kbd> undoes the last cursor action, so if you added a cursor too many or made a mistake, you can press <kbd>Ctrl+U</kbd> to go back to the previous cursor state. Adding cursor up or down (<kbd>Ctrl+Alt+Up</kbd>, <kbd>Ctrl+Alt+Down</kbd>) now reveals the last added cursor to make it easier to work with multiple cursors on more than 1 viewport height at a time (i.e. select 300 lines and only 80 fit in the viewport).

* <kbd>Ctrl+Shift+Alt+Up</kbd>, <kbd>Ctrl+Shift+Alt+Down</kbd>, <kbd>Ctrl+Shift+Alt+Left</kbd>, <kbd>Ctrl+Shift+Alt+Right</kbd>, <kbd>Ctrl+Shift+Alt+PageUp</kbd>, <kbd>Ctrl+Shift+Alt+PageDown</kbd> extend selections of box-selections. If standard multi-cursors selected, will box-select around the first cursor.

  These shortcuts are not available on Linux

Some options don't appear to work on my Windows 10 installation - possibly due to clashing shortcuts from extensions:

* <kbd>Alt+Shift+L</kbd> Selects all the same words that the first cursor is in. (Similar to selecting text and then <kbd>Ctrl+F2</kbd>). This works from the menu but not from the keyboard shortcut.
* <kbd>Alt+Shift+I</kbd> extend cursor selections to end of line.

### Changing the keyboard modifier

The `editor.multiCursorModifier` can be set to either:

* `ctrlCmd` - Maps to <kbd>Ctrl</kbd> on Windows and <kbd>Cmd</kbd> on macOS.
* `alt` - The default <kbd>Alt</kbd>.

This can also be toggled from the `Selection` menu.

### Multi-cursor menu items

The `Selection` menu has a number of entries related to multiple cursors.

