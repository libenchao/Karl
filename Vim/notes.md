# Vim Notes
This note is for memo for vim commands and concepts, reading this note frequently helps to master vim. Most points come from << Practical Vim Edit Text at the Speed of Thought >> which is a wonderful book.

## basic commands
1. `.` to repeat last change.
2. `>{motion}` to indent, for example `>G` can indent current line to the end. `>>` can change current line. Also for `<{motion}`.
3. `["x]c{motion}`delete {motion} text [into x register] and start insert mode.
4. `["x]C` delete current charactor to the end of line and start insert mode. Equal to `c$`.
5. `["x]s` delete [count] charactors [into x register] and start insert mode. Equal to `cl`.
6. `["x]S` delete [count] lines [into x register] and start insert mode. Equal to `^C`.
7. `~` can change 

## concepts
1. `motion`
2. `operator`

## mark
1. `m{a-zA-Z}` to make a mark.
2. ```\`{a-z}``` to goto a mark.

## movement
1. `f{char}` to go to next `{char}` in the current line. `;` to the next, `,` to the previous. `F{char}` to go to previous `{char}`.
2. 

## others
1. `vim -u NONE -N filename` can start vim without any vimrc and in a `compatible` mode. Of course `vim -u vimrc_file_name filename` can specify a vimrc file instead of default `~/.vimrc`.
2. 
