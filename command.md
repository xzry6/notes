# Note: Commands
This is a learning note of some useful commands
### Agenda
* [Terminal](#terminal)
* [Vim](#vim)

### Terminal
#### Shortcuts
| Key | Description |
| ----------- | ----------- |
| `Ctrl + A` | Go to the beginning |
| `Ctrl + E` | Go to the end |
| `Ctrl + L` | Clears the Screen |
| `Ctrl + U` | Cut everything backwards |
| `Ctrl + K` | Cut everything forward |
| `Ctrl + W` | Cut one word backwards |

#### Commands
| Command | Description |
| ----------- | ----------- |
| `find [dir] -name [search_pattern]` | Search for files |
| `grep [-r] [search_pattern] [file]` | Search for all lines containing the pattern |
| ``sed [-i] "" "s/[original]/[replace]/g" `grep [original] -rl [fileName]` `` | Search and replace the content | 
| `whatis [command]` | Gives a one-line description of [command] |

### Vim
| Key | Description |
| ----------- | ----------- |
| `h / j / k / l` | \< / v / ^ / \> |
| `b / e` | Jump to the next/previous word |
| `^ \ $` | Go to the beginning/end of a line |
| `fx` | Jump to next occurrence of character x |
| `gg / G / 5G`| Go to the first/last/5th line |
| `{ / }` | Jump to next/previous paragraph |
| `r` | Replace a single character |
| `i / I / a / A / o / O` | Insert at wordBeginning/linBegining/wordEnd/lineEnde/nextLine/lastLine |
| `v / ctrl+v / V` | Visual/VisualBlock/VisualLine |
| `u / ctrl+r` | Undo/redo |
| `zo / zc` | Unfold/fold |
