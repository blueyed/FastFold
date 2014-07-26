FastFold
========

Automatic folds (that is, folds generated by a fold method different
from `manual`), bog down VIM considerably in insert mode and are often
reevaluated prematurely (for example, when inserting an opening fold marker
whose closing counterpart has yet to be added to complete the fold.)

See http://vim.wikia.com/wiki/Keep_folds_closed_while_inserting_text
for a discussion.

With this plug-in, the folds in the currently edited buffer are updated by an
automatic fold method only

- when saving the buffer, or
- when closing or opening folds, or
- when typing `zuz` in normal mode

and are kept as is otherwise (by keeping the fold method set to `manual`). Each
of these triggers for updating folds can be disabled by adding

- `let g:fastfold_savehook = 0`, or
- `let g:fastfold_togglehook = 0` (which is the default), or
- `let g:fastfold_map = 0`

to the `.vimrc` file.

For example, by adding
```
set foldmethod=syntax

let g:tex_fold_enabled=1
let g:vimsyn_folding='af'
let g:xml_syntax_folding = 1
```
to the `.vimrc` file and installing this plug-in, the folds in a TeX, Vim or XML
file are updated by the `syntax` fold method when saving the buffer or typing
`zuz` in normal mode and are kept as is otherwise.

==

- If you prefer that folds are only updated manually but not when saving the buffer,
  then add `let g:fastfold_savehook = 0` to your `.vimrc`.

- If you prefer that folds are updated whenever you close or open folds by a
  standard keystroke such as `zx`,`zo` or `zc`, then add `let
  g:fastfold_togglehook = 1` to your `.vimrc`.

  The exact list of these standard keystrokes is
  `zx,zX,za,zA,zo,zO,zc,zC,zr,zR,zm,zM,zi` and it can be customized by changing
  the global variable `g:mapsuffixes` (that defaults to `let g:mapsuffixes =
  ['x','X','a','A','o','O','c','C','r','R','m','M','i']`).

  A suggested setting is to add
  ```
  let g:fastfold_togglehook = 1
  let g:mapsuffixes = ['x','X']
  ```
  to your `.vimrc` file.

  There is also a command `FastFoldUpdate` that updates folds and which can be
  used to update folds whenever you close or open folds by other mappings such
  as `<Ctrl-Z>` by adding `nnoremap <Ctrl-Z> :FastFoldUpdate<CR><Ctrl-Z>` to
  your `.vimrc`.
  The variant `FastFoldUpdate!` (with a bang) gives a echo line that shows by which
  fold method the folds were updated.

- If you prefer that this plug-in does not add a normal mode mapping that updates
  folds (that defaults to `zuz`), then add `let g:fastfold_map = 0` to your
  `.vimrc`.

  You can remap `zuz` to your favorite keystroke, say `<F5>`, by adding `nmap
  <F5> <Plug>(FastFoldUpdate)` to your `.Vimrc`.


==

Note that this plug-in will overwrite your manual folds when saving the currently edited
buffer, unless you either

- explicitly tell this plug-in to refrain from it via `g:fastfold_skipfiles`, a
  list of regular expressions against which the current file name is matched,
  or
-  when entering the buffer the local and global `foldmethod` were equal to
   `manual` and
  * since then the local and global `foldmethod` have remained equal to `manual`, or
  * since then the buffer has not been saved (if `let g:fastfold_savehook = 1`) and no fold has been toggled (if `let g:fastfold_savehook = 1`) and `zuz` (if `let g:fastfold_map = 1`) has not been typed in normal mode.

===

`FastFold.vim` integrates with version 1.2 and above of the `restore_view.vim`
plug-in that stores and restores the last folds by the `:mkview` and `:loadview`
if `restore_view.vim` is loaded *AFTER* `FastFold.vim`. (To ensure the correct
autocmd execution order.)

A recent fork of it is available at

http://www.github.com/Konfekt/restore_view

==

A `CustomFoldText()` function that displays the percentage of the number of buffer lines that the folded text takes up and indents folds according to their nesting level is available at

http://www.github.com/Konfekt/FoldText

