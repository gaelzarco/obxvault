
## INSERT MODE
`i` is to enter `INSERT MODE` at the beginning of the block cursor.
`a` is to enter `INSERT MODE` at the end of the block cursor.
- To leave `INSERT MODE` use `ctrl + c` or `ctrl + [` or `esc`

## VISUAL MODE
While in `VISUAL MODE` (the default mode in NeoVim), you are able to navigate through a file using the following keys:
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

While in `VISUAL MODE` you are also able to delete words/lines.

- `dd` deletes a line.
- `dw` deletes the word at the cursor.
- `db` deletes the word before the cursor.
- `dj` deletes a line down
- `dk` deletes a line up
- _etc..._

_If you haven't noticed by now, commands can be chained in NeoVim to execute specific operations._

_For example:
- Chaining `d16j` enables you to delete 16 lines down.
- Using `d2w` allows to you delete 2 words beginning from the cursor.
  
## COMMAND MODE
`:` is to enter `COMMAND MODE` 

Saving files:
- `w` to save your current file.
- `wq` to save your current file and quit.