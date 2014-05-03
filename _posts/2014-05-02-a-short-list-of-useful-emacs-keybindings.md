---
layout: post
title: "A Short List of Useful Emacs Keybindings"
description: "Useful Emacs Keybindings list"
category: guides
tags: [emacs, keybindings, list]
---
{% include JB/setup %}

## Emacs Keybindings

### Copy/Paste (yank/kill)

Kill region (cut)

        C-w

Copy region to kill ring (copy)

        M-w

Kill line (and copy it; must be at start of line)

        C-k

Yank, then cycle through items in kill ring

        C-y M-y

Go the opposite direction in the kill ring (that’s Meta-Shift-y)

        M-Y

Browse kill ring (that’s an L)

        C-c l

### Editing

New line and indent (equivalent to “enter” followed by “tab”)

        C-j

Hippie expand

        M-/

First non-whitespace character in line

        M-m

### Comments

Comment line (or region if one is selected)

        M-;

### Describe key

Describe key

        C-h k

###Navigation

end of document

        C-M->

Beginning of document

        C-M-<

Move forward and back in buffer selection window

        C-s & C-r

Move to the first non-whitespace character on the line

        M-m

Move to the beginning of the current buffer

        M-<

Go to line

        M-g g

Push current point location to the mark ring

        C-{spc} C-{spc}

Set point to the previous location in the mark ring

        C-u C-{spc}

### Jumping

Jump to head char (uses ace-jump-mode)

        C-0

Jump to char within word

        C-u C-0

Jump to line

        C-u C-u C-0

### Window Controls


Close window

        C-x 0

Scroll the other window without leaving the current one

        C-M v

### M-x programs

Lists occurrences of a string

        M-x occur

## Copy/Paste compatible format

Here are the keybindings in a format that is easier to copy/paste into
your own document.


NOTE: Use C-c {letter} for custom commands


Copy / Paste

C-w :: kill region (cut)

M-w :: copy region to kill ring (copy)

C-k :: kill line (and copy it; must be at start of line)

C-y M-y :: yank, then cycle through items in kill ring

M-Y :: Go the opposite direction in the kill ring (that’s Meta-Shift-y)

C-c l :: browse kill ring (that’s an L)


Editing

C-j :: new line and indent (equivalent to “enter” followed by “tab”)

M-/ :: hippie expand

M-m :: first non-whitespace character in line


Comments

M-; :: comment line (or region if one is selected)


Describe key

C-h k :: describe key


Navigation

C-M-> :: end of document

C-M-< :: beginning of document

C-s & C-r :: move forward and back in buffer selection window

M-m :: move to the first non-whitespace character on the line

M-< :: move to the beginning of the current buffer

M-g g :: go to line 

C-{spc} C-{spc} :: push current point location to the mark ring

C-u C-{spc} :: set point to the previous location in the mark ring


Jumping

C-0 :: Jump to head char (uses ace-jump-mode)

C-u C-0 :: Jump to char within word

C-u C-u C-0 :: Jump to line


Window Controls

C-x 0 :: close window

C-M v :: Scroll the other window without leaving the current one


M-x programs

M-x occur :: lists all occurrences of a string
