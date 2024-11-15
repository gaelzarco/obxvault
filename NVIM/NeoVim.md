## The Basics

The basics for getting comfortable with NeoVim.

__NeoVim is a modal IDE.__ 

__File Navigation:__
you are able to navigate through a file using the following keys:
- `h` left
- `j` down
- `k` up
- `l` right

These keys can be paired with numbers to jump the respective amount of characters (left + right) or lines (up + down).

_example:_
- _`16j` moves you 16 lines down_
- _`10l` moves you 10 characters to the right_

You can navigate between words using:
- `w` to move one word to the right
- `b` to move one word to the left

You are also able to delete words/lines.

- `dd` deletes a line.
- `dw` deletes the word at the cursor.
- `db` deletes the word before the cursor.
- `dj` deletes a line down
- `dk` deletes a line up
- _etc..._

To `yank` (copy):
- `yy` yanks your current line.
- `yw` yanks the word at the cursor
- _etc..._

To paste:
- `p` pastes the last deleted or yanked content
	- __YANKING AND DELETING GO TO THE SAME BUFFER _(whichever came last)___

_If you haven't noticed by now, commands can be chained in NeoVim to execute specific operations._

_For example:
- Chaining `d16j` enables you to delete 16 lines down.
- Using `d2w` allows to you delete 2 words beginning from the cursor.
  
NeoVim has multiple modes that serve certain functionalities.
### `INSERT MODE`
`i` is to enter `INSERT MODE` at the beginning of the block cursor.
`a` is to enter `INSERT MODE` at the end of the block cursor.
- To leave `INSERT MODE` use `ctrl + c` or `ctrl + [` or `esc`

`INSERT MODE` allows you to edit your source code/files as you normally would.
### `VISUAL MODE`

`VISUAL MODE` gives you access to text-highlighting for copy/paste functionalities.
	_You are able to navigate using all the navigation keys that the `DEFAULT MODE` enables._

`v` is to enter `VISUAL MODE`
- `y` is to `yank` (copy)
- `p` is to paste
	- It does not grant you a new line and instead pastes the exact content you yanked.

`VISUAL LINE MODE` is basically a more convenient `VISUAL MODE`.

`shift + v` is to enter `VISUAL LINE MODE`
- You can use `shift + v` to highlight the entire line even in `VISUAL MODE`
- Using the same commands above, `p` now pastes with a new line at the end. 
	- if you attempt to use `p` after already having pressed `p`, you will now paste the last thing you deleted.

__YANKING AND DELETING GO TO THE SAME BUFFER.__
- This means that hitting `dd` followed by `p` in `DEFAULT MODE` will paste the last deleted text.
### `COMMAND MODE`
`:` is to enter `COMMAND MODE` 
- `w` to save your current file.
- `wq` to save your current file and quit.

`COMMAND MODE` enables you to execute certain commands in relation to NeoVim itself.
