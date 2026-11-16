# sigh, a code viewer

I use agentic AI for software development. But I like to know what's going on in
the codebase that I'm working on. I'm okay vibe coding stuff for small hobby
projects, but when I do it professionally I need to [maintain my
understanding](@/posts/coding-with-a-sigh) of what's going on in the code.

I figured that if I mostly create code by talking to the agent, then what's the
point of an editor? A more streamlined user interface dedicated to viewing code
may be more useful. You can see it as an editor or IDE but without the editor
itself.

The `sigh` UI is born out of this. I'm announcing it here in the hope that
others find it useful and help improve it. Here I talk about what it does, and
what it doesn't do well yet. Of course I used it to help me build and improve
itself.

The basic UI is oriented around a central code viewing pane and a number of
sidebars. There are the usual left and right sidebars, and also top and bottom
sidebars.

## Designing this

A lot of the work has been to think about the UI interaction patterns and how to
design them. I've arrived at a system with a central code viewer pane, and
sidebars. The sidebars can be operated from the keyboard and all work the same
way. The sidebar keys are organized in this pattern:

```
  i
j k l 
```

Where `j` and `l` are the left and right sidebars, and `i` and `k` are the top
and bottom sidebars. To open a sidebar, you press `ctrl-<key>`, so `ctrl-l` to
open a right sidebar. You can also open a sidebar through the sidebar widget in
the top bar.

Navigation through the sidebar is then done using the bare sidebar key. `j` goes
one item down in the sidebar. `J` (`shift-j`) goes one item up. All sidebars
work in this way, and I found this organization helps me remember what does what
quite well.

Your mileage may vary and one of the wishlist items is a configurable keyboard
mapping.

## Code viewer

The code viewer lets you navigate code in a file. You can scroll, there are line
numbers, there's syntax highlighting. The most interesting part is that it's
also a diff viewer: if you have local changes in your checkout in this file,
then you can see them. There are various diff modes, including difftastic
integration.

You can use the LSP to navigate items under the cursor to go to definition or go
to type definition.

If you want to edit a file, you can hit `e` to edit it in your `$EDITOR` and `E`
to edit it in your IDE.

[screenshot]

## Sidebars

All the sidebars follow some basic principles. They typically display a list of
items, which you can navigate easily using the mouse or keyboard. The file
explorer for instance displays a tree of files, and the search sidebar shows a
list of search hits. All sidebars use the same interaction paradigm, with both
keyboard and GUI support.

Let's do a quick tour of the sidebars:

### file explorer

A tree of the files in your project. It shows which files have been added,
modified and removed too.

### changes sidebar

The changes sidebar shows all the diff blocks in your project based on version
control. You can this way easily navigate from diff block to diff block. This is
handy if you want to review everything an agent touched.

### changed types

Often the data types live at the heart of a piece of code. This is a sidebar
that shows which types have changed.

### search

You can search through your entire codebase.

### find in file

You search in the currently displayed files.

### type search

You search through types in your codebase.

### history

You see your navigation history; which locations in which files you visited.

### docs

This sidebar doesn't display a list of items, but the documentation for a
particular item under the cursor, as obtained from the LSP.

### references

This shows all the references in the codebase to the item under the cursor, which also uses
the LSP.

### implementations

This shows all the implementations of a particular trait/interface, using the LSP.

### code tour

This is one of the more interesting feature. sigh ships with a little MCP that
lets the agent create a code tour. You can install a skill that explains how
this tour is made. You then just ask your agent to give you a code tour of the
changes in a repository, or any topic you want. Tour stops are shown inline in
the viewer, and you can navigate tour stop using the same sidebar mechanism as
anything else.

[screenshot]

## Wishlist and limitations

Sigh is pretty raw in some places. There are a lot of things that could do with
improvement, and I hope that you are interested in discussing these and
contributing them!

### only tested on Linux

I've only tested this on Linux. Most of the codebase should be cross platform,
but that needs to be tested.

### there's git support but less tested

While there's git support, I use [jujutsu]() for version control and the
features may be best tested using jj.

### Only Rust LSP support

While the syntax highlighting supports Python and TypeScript, the only LSP I've
currently integrated is rust-analyzer, so docs, go to definition, references,
are all limited to Rust. 

### No inline types yet

One major feature I'd like to add is use the LSP to show use inline types in one
way or another.

### LSP flakiness

When I use an IDE I often find myself restarting the language server as
something got stuck or wedged. It's my most popularly used command. Similarly,
the LSP currently gets wedged in various ways with `sigh`. Right now the fix in
sigh is to restart it. This still needs to be debugged and made more solid. My
dream is that I never have to restart the LSP again as `sigh` will detect it and
do it for me.

### words diff mode

The words diff mode kind of works, but sometimes displays rather unexpected
diffs. It's tricky to get this right, and I'm tempted to try to port what `jj`
does to improve this.

### better search

I intend to integrate ripgrep for full project search, but right now it's still
a simplistic search.

### configurability

I tried to make `sigh` not need configuration to work. But some kind of
configuration system is needed eventually.

Once consequence of this is that while `e` uses whatever your `$EDITOR` is, `E`
right now is hardcoded to start VS Code, as that can't be configured yet.

Another one is that you're stuck with the key bindings I've worked out, and
there's no way yet to reconfigure them.

### unfinished annotation sidebar

I have quite a bit work done that lets you create and edit new annotations in
the codebase and then send them off to your agent, so you can control the agent
from `sigh` in the context of the code. Right now the annotation editor isn't
supported (I had previous TUI support, but abandoded the TUI)

This all needs a rethink in various ways.

### many more

There are many more features I have thought of, and many more you will think of
that I haven't. Let me know!

