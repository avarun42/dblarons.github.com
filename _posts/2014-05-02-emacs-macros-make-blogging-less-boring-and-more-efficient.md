---
layout: post
title: "Emacs Macros Make Blogging Less Boring (and More Efficient)"
description: "A summary of how I made blogging a little easier"
category: 
tags: [emacs, blogging]
---
{% include JB/setup %}

### Copy/Paste Copy/Paste

On my flight back to San Diego this morning, I decided to turn my
Evernote collection into [a single blog post](). To do this, I had to
copy/paste the title and link from each note in each folder into my
Markdown-formatted post in Emacs.

To make this process a little easier (and less monotonous), I created
a few Emacs keybindings to expedite the process.

### Post Structure

When I outlined my post in Markdown, I made the titles of Evernote
collections have larger type than the titles of the individual
folders. As you will see later, this ended up being a valuable
decision. The structure of each section looked like this:

    ## Programming Languages

        ### Bash

        ### Clojure

        ### C

        ...

The first keyboard macro I created allowed me to create subheadings
easily.

    C-x S-(  ;; start keyboard macro
    <ret><ret>###<spc>
    C-x S-)  ;; close keyboard macro

The macro is called from the end of the last heading line. Looking back, this macro could have been improved--I only figured out
a better method in my second macro.

### Moving Article Titles and URLs from Evernote to Emacs

The next problem I encountered was how to move 122 articles from my
Evernote folder into my Markdown file without taking all 5 hours of my
flight. In lieu of an API, I determined that at least some copy/paste
action would be required. The solution I settled on was:

1. Copy the article title from Evernote

2. Use the following macro that creates the Markdown-structured
article including the title that was copied from Evernote.

Structure required:

    ### heading

        [Article Title]\(http://myarticleurl)  ;; (ignore the forward-slash)

Macro:

    C-x S-(  ;; start keyboard macro
    <ret><ret>[  ;; format space and add braces
    C-y  ;; yank the article title from the kill ring
    C-e  ;; go to the end of the line
    (    ;; add parens for the link
    C-x S-)  ;; end the keyboard macro

Like the previous macro, this one was called from the end of the last line.

Unfortunately, I still had to go back and copy/paste the link into the parens.

### Adding Tags

After all the articles were added, I wanted to add the folder names (those with three #'s) into my list of tags for the article. Recognizing that this would have taken forever to do manually, I decided to write another handy macro (one that I will be sure to use again somewhere).

    C-x S-(  ;; start keyboard macro
    C-s ###  ;; start a search for the subheadings
    C-f C-<spc> C-e M-w  ;; select and copy the folder title
    C-s  ;; move to the next subheading
    C-x S-)  ;; end keyboard macro

This worked perfectly! I had all of my subheadings in the kill ring, ready to add to the tags section in my markdown file.

### Moving the Tags into the Tags Array

My last macro required me to take all of the tags in my kill ring and paste them, separated by a comma and space, into my tags array. The array is formatted like this:

    tags: [articles, guide, vim, emacs, scala, ...]

My macro took each tag from the kill ring (in reverse order, to my
dismay) and added them to the array.

    C-x C-(  ;; start keyboard macro
    ,<spc    ;; format
    C-y M-y  ;; get next from kill ring
    C-x C-)  ;; end keyboard macro

Note that this is a fencepost problem. I had to C-y the first title
out of the kill ring before I began that macro.

### Conclusion

Anyways, I hope you enjoy the meta-analysis of the last blog post and
perhaps understand a little better how to use keyboard macros. I was
just happy that instead of spending an extra hour copy/pasting from
Evernote into Emacs, I could write this post instead!



