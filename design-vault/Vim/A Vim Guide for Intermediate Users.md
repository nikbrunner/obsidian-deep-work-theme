# A Vim Guide for Intermediate Users

#app/vim/tricks #app/vim/article

> You know already the basics of Vim and you want to get better? This article explains more advanced Vim concepts.

Source: [https://thevaluable.dev/vim-intermediate/](https://thevaluable.dev/vim-intermediate/)

Welcome to the second part of this series aimed to make you a better Vim user! If you have no idea about Vim, you should begin with [the first part](https://thevaluable.dev/vim-for-beginners/). In this article, I’ll explain many more concepts, some of them making Vim truly special compared to other editors. Who wasn’t blown away discovering Vim’s macros?

Specifically, we’ll see together:

- Ways you can organize open files in Vim using buffers, windows, tabs, and the argument list.
- Useful motions to jump quickly from one place to another in your entire codebase.
- Mapping new keystrokes to old keystrokes or commands.
- Powerful functionalities to repeat some of your keystrokes.
- Ways of manipulating the command line history.
- Plugins which offers different ways to manage some ideas we saw before.

The amount of information in this article can feel overwhelming. My advice: take your time and don’t try to swallow everything at once. Experiment with Vim as you read along, try to understand how it works, and you’ll have a powerful tool you can control entirely with your keyboard.

You’ll see at the end of each sections some related Vim’s help commands. You can read these help sections directly in Vim when you’re ready to dive deeper.

## Vim’s Spatial Organization

If you’re using an IDE, you’re certainly used to manage your files with tabs. Vim use other ways to represent and organize open files. Indeed, there are four [layers of abstraction](https://thevaluable.dev/abstraction-type-software-example/) you can use for that: the *buffers*, the *windows*, the *tabs*, and the *argument list*.

### Buffers

A *buffer* directly match an open file in memory. To make a comparison with a standard IDE, a buffer would be the *content* of a tab. The big difference: when you close a tab in an IDE, you close the file as well. Not in Vim; if you close a window containing a buffer, the buffer is still there, *hidden*.

In fact, a buffer can have three different states:

- *active* - The buffer is displayed in a window.
- *hidden* - The buffer is not displayed, but it exists and the file is still open.
- *inactive* - The buffer is not displayed and *empty*. It’s not linked to any file.

The content of a file in a hidden buffer is not directly visible in Vim. At that point, you might wonder: how do we know that this buffer is still open, if we can’t see it?

To see all opened buffered, we can look at the *buffer list*. You can use the command `:buffers` to display it. Each line contains:

1. The buffer unique ID.
2. Indicators displaying different informations (for example `a` for active, `h` for hidden, or (space) for inactive).
3. The name of the buffer, if any. It can be the filepath of the file linked to the buffer.
4. The line number where the cursor is.

For example: `27 %a "layouts/shortcodes/notice.html" line 18` means that the buffer ID 27 is in state `a` (active), its name is `layouts/shortcodes/notice.html` and the cursor in this specific buffer is on line 18. You can as well know what’s the current buffer displayed with the flag `%` just before its state.

To navigate through the buffer list, you can use these commands:

- `:buffer <ID_or_name>`- Move to the buffer using its ID or its name.
- `:bnext` or `:bn` - Move to the next buffer.
- `:bprevious` or `:bp` - Move to the previous buffer.
- `:bfirst` or `:bf` - move to the first buffer.
- `:blast` or `:bl` - move to the last buffer.
- `CTRL-^` - switch to the alternate buffer. It’s indicated in your buffer list with the symbol `#`.
- `<ID>CTRL-^` - Switch to a specific buffer with ID `<ID>`. For example, `75CTRL-^` switch to the buffer with ID 75.

You can as well apply a command to all buffers using `:bufdo <command>`.

Not all buffers are displayed in the buffer list. To display unlisted buffers, you can use the command `:buffers!` or `ls!`. You’ll see unlisted buffer with an indicator `u` just after its ID.

Now, let’s ask this existential question: how can we create buffers?

- If you create a window, a buffer will be created automatically (see below).
- `:badd <filename>` - Add `<filename>` to the buffer list.

If we can create buffers, we should be able to delete them:

- `:bdelete <ID_or_name>` - Delete a buffer by ID or name. You can specify more than one ID or name separated with spaces to delete multiple buffers.
- `:1,10bdelete` - Delete buffers from ID 1 to 10 included.
- `:%bdelete` - Delete all buffers.

If you modify a file, forget to save it, and close the window making the buffer hidden, you won’t be able to quit Vim. It will complain that you’re hidden buffer is not saved; to get around that, I would recommend to set the option hidden in your vimrc (by default `~/.vimrc`), as follow:

```other
set hidden
```

You can try it directly in your current session by running the command `:set hidden!` to toggle the option on and off. You can play around with it and see what suits best for you.

To see the value of any option, you can use a question mark. For example: `:set hidden?` or `:set filetype?`.

- `:help buffers`
- `:help :buffers`

### Windows

A window in Vim is nothing more than a space you can use to display the content of a buffer. Don’t forget: when you close the window, the buffer stays open.

When you open Vim, one window with one empty buffer are automatically created.

To create windows, you can use the `:new` command, or one of these keystrokes:

- `CTRL-W s` - Split the current window horizontally.
- `CTRL-W v` - Split the current window vertically.
- `CTRL-W n` - Split the current windows horizontally and edit a new file.
- `CTRL-W ^` - Split the current with the *alternate file* (buffer with the `#` indicator in your buffer list).
- `<buffer_ID>CTRL-W ^` - Split windows with the buffer of ID `<ID>`. For example, `75 CTRL-W ^` will open a window with the buffer of ID 75.

To move your cursor from one window to another, you can use:

- `CTRL-W <Down>` or `CTRL-W j`
- `CTRL-W <Up>` or `CTRL-W k`
- `CTRL-W <Left>` or `CTRL-W h`
- `CTRL-W <right>` or `CTRL-W l`

You always dreamt to move the windows? Me too. Here’s how to do it:

- `CTRL-W r` - Rotate the windows.
- `CTRL-W x` - Exchange with the next window

Who wants windows without being able to resize them? Here are the keystrokes you need:

- `CTRL-W =` - Resize windows for them to fit on the screen with the same size.
- `CTRL-W -` - Decrease window’s height.
- `CTRL-W +` - Increase window’s height.
- `CTRL-W <` - Decrease window’s width.
- `CTRL-W >` - Increase window’s width.

Using these keystrokes to move the cursor from window to window and to move the windows themselves is pretty tedious . We’ll see later a plugin which can help to make the whole operation smoother.

If you want to quit windows, you can use the commands:

- `:q` - To `q`uit the current window. People lied to you! `:q` doesn’t quit Vim, but a window. You quit Vim only if there is only one window open.
- `:q!` - To `q`uit the current window, even if there is only one window open with an unsaved buffer`!`.
- `:help windows`
- `:help opening-window`
- `:help window-move-cursor`
- `:help window-moving`
- `:help window-resize`

### Vim Tabs

We saw that a buffer is an open file, and a window is the container for an active buffer. We can see tabs as a container for a bunch of windows. In that way, it’s very different than the concept of tabs in a standard IDE!

Here are the commands to create and delete tabs:

- `:tabnew` or `:tabe` - Open a new tab.
- `:tabclose` or `:tabc` - Close the current tab.
- `:tabonly` or `:tabo` - Close every other tab except the current one.

To move from tab to tab, you can use these keystrokes:

- `gt` - `g`o to the next `t`ab.
- `gT` - `g`o to the previous tab.

You can also add a count before the last two keystrokes. For example, `1gT` go to the first tab. Yep, tabs are indexed from 1.

### The Argument List (arglist)

The argument list (also called arglist) is the fourth and last container allowing you to organize your open files. It’s useful to see it as a *stable subset* of the buffer list, as Drew Neil point it out in [one of his vimcast](http://vimcasts.org/episodes/meet-the-arglist/). As a result, it follows these two rules:

1. Every file in the arglist will be in the buffer list.
2. Some buffers in the buffer list won’t be in the arglist.

The files you want to open when you run Vim - such as executing `vim file1 file2 file3` - will be automatically added to the arglist and, as we just saw, to the buffer list.

The arglist can be useful to isolate some files from the buffer list to do some operations on them. Here are some commands you can use to manipulate the arglist:

- `:args` - Display the arglist.
- `:argadd` - Add file to the arglist.
- `:argdo` - Execute a command on every file in the arglist.

To edit the files in the arglist, you can use these commands:

- `:next` - Move to the next file in the arglist.
- `:prev` - Move to the previous file in the arglist.
- `:first` - Move to the first file in the arglist.

I don’t use very often the arglist personally, but many users do. The buffer list can be modified by other actions unrelated directly to buffers, like opening new windows. The arglist stays the same, except if you explicitly modify it. That’s why it’s stable.

## Mapping Keystrokes

We’ve seen a great deal of keystrokes and commands. It would be nice to be able to modify these keystrokes, or to assign new keystrokes to precise commands.

You can use mapping commands for every Vim mode:

- `:nmap` - Create new mapping for NORMAL mode.
- `:imap` - Create new mapping for INSERT mode.
- `:xmap` - Create new mapping for VISUAL mode.

It might sound confusing to have different mappings for different modes, but it’s actually very easy to remember, thanks to our muscle memory.

Let’s try an example together by mapping `w` to `dd`. By default, `dd` delete a line, and `w` is a motion to move your cursor from word to word.

1. Run the command `:nmap w dd`
2. Try to hit the keystroke `dd`. It will delete a line.
3. Try to hit `w`. It deletes a line to.

However, `w` can’t be used anymore to move from word to word. Let’s try to fix that by running: `:nmap v w`.

Try to hit `v` now. It deletes a line too! You just did a recursive mapping: `v` maps to `w` which maps to `dd`. It would be nice to:

1. Map `w` to `dd`
2. Map `v` to the motion made by `w` *before* its mapping with `dd`.

To do that, you can use the following mapping commands:

- `:nnoremap` - Create mapping for NORMAL mode (non recursive)
- `:inoremap` - Create mapping for INSERT mode (non recursive)
- `:vnoremap` - Create mapping for VISUAL mode (non recursive)

To do the silly mapping we wanted to do before, restart Vim to have the default mapping, then execute these commands:

```other
:nnoremap w dd
:nnoremap v w
```

This time, `w` delete a line and `v` moves from word to word.

You can as well use special characters in your mapping. For example:

- `<space>` for `SPACE`
- `<c-w>` for `CTRL-W`.
- `<cr>` for `c`arriage `r`eturn (`ENTER`).

To see the complete list, run the command `:help key-notation`

Now that you have the Power, I would advise you not to change the default Vim mapping as much as you can. The previous example of mapping I’ve given is a good one to show you what **not** to do. It’s very practical to use as much as you can the default mapping, because you can use them on any instance of Vim possible, even in a docker container or on a remote server.

If you want to create new mappings, you should use a special key called the *leader key*. It’s a way to create mapping namespaces: first, you use your leader key, then you use your keystroke. Thanks to the leader key, your new keystroke will never conflict with the default Vim keystrokes.

To set up your leader key, you need to set the variable mapleader. Here’s an example:

```other
:let mapleader = "<space>"
```

Every mapping command we saw are often written in your vimrc to set them when you open vim. For example, you can write the following to it:

```other
let mapleader = "<space>"
nnoremap <leader>bn :bn<cr> ;buffer next
nnoremap <leader>tn gt ;new tab
```

The keystrokes `<space> bn` will move to the next buffer, and `<space> tn` will move to the next tab. Notice that when you want to map a keystroke to a command, you need to add `<cr>` at the end exactly like you would type `ENTER` (or carriage return) to execute the command. Awesomeness!

- `:help mapping`
- `:help leader`
- `:help key-notation`

## Jump! Jump! Jump!

There are special motions in Vim called jump-motion. These motions jump several lines away, like the keystroke `G` we saw in the [last article](https://thevaluable.dev/vim-for-beginners/).

Moving using `k` or `j` is not considered as a jump-motion.

### Jump list

Each time we use a jump motion, the position of the cursor before the jump is saved in the *jump list*. You can move through this jump list with the following keystrokes:

- `CTRL-o` - Go to the previous (`o`lder) cursor position.
- `CTRL-i` - Go to the next cursor position (`i` is near `o`).

You can move from line to line and even from buffer to buffer with these holy commands. I use them all the time, and you will, too.

To display your jump list, use the command `:jumps`.

### Change list

Another useful list is the change list. Each time you insert something (using INSERT mode), the position of your cursor is saved in the change list.

You can navigate through the change list using these keystrokes.

- `g;` - Jump to the next change.
- `g,`- Jump to the previous change

Discovering them was like seeing the light for the first time.

### Methods Jumping

Since we’re developers, it’s nice to be able to jump from method to method. You can do that with the following keystrokes:

- `[m` - move to the start of a method.
- `]m` - move to the end of a method.

These methods should have similar syntax than Java’s methods for these keystrokes to work.

- `:help jump-motions`
- `:help jumplist`
- `:help changelist`

## Repeating Keystrokes

You know what’s great? Automation. You know what’s great with Vim? Keystrokes automation. That’s a very powerful feature, so get ready.

### Single Repeat

Automating tedious tasks is our job, as developers. That’s why some call us lazy; I call that focusing on the important.

When I heard about the sacred single repeat, my life in Vim changed. Here’s how you can fell like a powerful superhuman:

- `.` - Repeat the last change.
- `@:` - Repeat the last command executed.

The period is now my best friend. It’s simple and diabolically effective.

### Complex Repeat: The Macro

I deceived you with my period keystroke? Wait to see this one. Macros will bring even more power at your fingertips.

In Vim, you can record series of keystrokes and repeat them in order. The process includes multiple steps:

1. `q<lowecase_letter>` - Begin recording keystrokes in a register. You can think of a register as a place in memory, or as a clipboard.
2. Every keystrokes you’ll do onward will be saved.
3. `q` - Stop the recording.
4. `@<lowercase_letter>` - Execute the keystrokes you’ve recorded.

For example, let’s say that you need to repeat keystrokes you need to do on multiple lines:

1. Hit `qa`.
2. Do what you have to do. For example: `^cawhello<Esc>`.
3. Hit `q` again.
4. To execute your series of keystrokes, hit `@a`. For the example above, go on a new line.

Here, I use `q` (for the `<lowercase_letter>`) as an example; you can actually use whatever lowercase letter you want. If you need to repeat your keystrokes again after the first repeat, you can even use `@@` which repeat your previous `@` command.

## Command Line Window

You can access the history of your Ex commands directly in Vim:

- `q:` - Open command line history.
- `q/` and `q?` - Open search history.
- `CTRL+f` - Open command line history while in COMMAND LINE mode.

Using the keystrokes above, you can modify any command line you want and execute it with `ENTER`. Very handy when you need to repeat a command line with slight differences. It’s nice as well if you want to type a complex command using Vim’s editing power.

## Undo Tree

We saw in the previous article how to undo and redo a command. Vim allows you as well to save all these undos in a file, for each file you modify. It means that even if you close Vim and come back to your file, you’ll have access to your last change by “undoing” them.

You need to configure it in your vimrc file, by adding the following:

```other
" save undo trees in files
set undofile
set undodir=~/.vim/undo

" number of undo saved
set undolevels=10000
```

You don’t have to save your undos in `~/.vim/undo`. Change it without regret if you need to.

We set the option `undolevels` to 10000 to save 10000 undos maximum per file.

That’s not all: you could think that Vim will only save a *list* of undos, but it actually saves a whole undo tree.

Let’s take an example: when you do three changes, then undo two of them, and do one (or more) changes, a new branch will be created. You can picture it like this:

```other
@
|
| o -> third change
| |
| o -> second change 
|/
o -> first change
|
o
```

The third and second changes have been undo, `@` represent where you are now. You can revert to any of these changes, which means that you can revert to *any last changes* you’ve made.

It’s a bit difficult to navigate in this tree with vanilla Vim, but we’ll see in the next section a very useful plugin to come back to any changes you’ve made. You’ll even be able to search in the entire undo tree for a piece of content you want to find back!

- `:help undo-redo`
- `:help undo-persistence`
- `:help undo-tree`

## Plugins

Everything we saw until now can be managed differently using a couple of plugins. If I would learn Vim all over again, I would first get comfortable with everything we saw till now. That said, here are some plugins which can makes things even easier.

### Plugin Manager

It’s easier to manage Vim’s plugins with a good plugin manager. I would recommend `vim-plug`: you’ll find instructions to install it on its [Github page](https://github.com/junegunn/vim-plug).

Then, add the following at the beginning of your `.vimrc`:

```other
" Specify a directory for plugins
" - For Neovim: stdpath('data') . '/plugged'
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.vim/plugged')
call plug#end()
```

Again, you can replace `~/.vim/plugged` depending on where your dotfiles for Vim are.

For example, if you want to install the plugin `vim-bbye`, you need to:

1. Find the Github repository of the plugin, in that case `https://github.com/moll/vim-bbye`.
2. Take the username and the repository’s name from the url: `moll/vim-bbye`.
3. Add `Plug 'moll/vim-bbye'` between `call plug#begin('~/.vim/plugged')` and `call plug#end()`.
4. Source your `.vimrc` with the command `:source <path_to_vimrc>`. If your current buffer is already your vimrc , you can run `:source %`.
5. Run `:PlugInstall`.
6. Your plugin is installed!

To update every plugins you have, you can use the command `:PlugUpdate`. It will install the plugins not installed yet, too.

### Closing Buffers Without Closing Windows

By default, when you use `:bdelete` to close a buffer, the window will be closed too. If you want to keep your window layout, you can use the plugin [vim-bbye](https://github.com/moll/vim-bbye).

It’s a very small plugin which gives you a new command, `:Bdelete` (with an uppercase `B`), allowing you to close a buffer without closing a window.

### Easily Resizing Windows

I’m not a big fan with the vanilla way to resize windows. The plugin [winresizer](https://github.com/simeji/winresizer) gives you a new mode to resize them.

To enter this new mode, you need to use the keystroke `CTRL-e`. If you don’t like it, you can add this line to your vimrc for example:

```other
let g:winresizer_start_key = "<leader>w"
```

Now, you can use this new resize mode with the keystrole `LEADER w`. It’s just an example, use whatever keystroke works for you.

When you are in this new resize mode, you’ll see some help at the bottom of Vim. You can use `hjkl` to resize the window, `e` to change mode from `resize` mode, `focus` mode or `move` mode. The last one allows you to swap windows.

### Navigating through your buffer

I use the plugin [fzf.vim](https://github.com/junegunn/fzf.vim) intensively. It allows you fuzzy search buffers or even files in your current path (set up with the option `path`).

You’ll need to have [fzf](https://github.com/junegunn/fzf) installed to use it. Then, you can use the command line `:Buffers` (with a `B` uppercase) to fuzzy find and select the buffer you want.

That’s only a tiny subset of what fzf can do for you.

### Manipulating the Undo Tree

As promised, here’s a very useful plugin to come back to any change you want: [MundoTree](https://github.com/simnalamburt/vim-mundo). The command `:MundoToggle` will show you the undo tree of your current buffer in a new buffer.

From there, you can select whatever change you want to come back to.

## The Path to Mastery

There are many important Vim features I didn’t write about in this article. You’ll find at the end of the article the help commands for some of them. I might cover them in a next article, if some readers are interested.

I strongly believe that knowing, even on a very high level, the principles and fundamentals of a concept can help you understand anything built upon this concept. As such, knowing how vanilla Vim works and getting a good grasp on its functionalities is important to understand what plugin can be useful for your own needs.

What did we learn in this article:

When you’re comfortable with the concepts of this article, you’ll normally be able to understand [another article of mine](https://thevaluable.dev/vim-search-find-replace/) focused on searching and replacing in Vim.

What did we learn in this article?

- Buffers are open files, windows are containers for buffers, tabs are sets of windows, and the argument list is a subset of the buffer list.
- You can map keystrokes for any Vim mode recursively, but it’s good practice to avoid doing so.
- You can use powerful keystrokes to go through the jump list or the change list.
- Using the period `.` or Vim’s macros to automate repeating editing tasks is really powerful.
- You can use the command line windows to re-run or modify commands already executed, using the command line history.
- Vim’s undo tree allows you to come back to any modification made in any file, even after closing Vim.
- The plugins vim-bbye and winresize can help you to manage your buffers and windows in a easier way.

This article wasn’t enough? You’re never tired to read articles about Vim and you want always more? In that case, you can read the next part of this series: [Vim for advanced users](https://thevaluable.dev/vim-advanced/).

Are you faster with Vim than with any other editor or IDE? The question is not really important to me. I love using Vim because of its flexibility and overall approach to editing. That’s what gives me the real productivity boost I’m craving for.

