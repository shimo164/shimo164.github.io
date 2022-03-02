- [Commands to operate Client, Window and Pane](#commands-to-operate-client-window-and-pane)
- [Menu bar parameter](#menu-bar-parameter)
- [Move a pane to another window](#move-a-pane-to-another-window)

# Commands to operate Client, Window and Pane

*Note: default prefix is Ctrl+b*

| | Client | Window | Pane |
| ---- | ---- | ---- | ---- |
| **New**  | $tmux| prefix c | prefix "<br> prefix % |
| **Open** | $tmux a [-t num]|- |- |
| **Kill** | $tmux kill-session [-t num] | prefix & | prefix x<br>$exit |
| **Move** | prefix (<br>  prefix )<br>  prefix L|prefix p<br> prefix n<br>  prefix num | prefix o<br>  prefix Up<br>  prefix Down|
| **Rename** | prefix $  | prefix ,    |       |

*Note: To kill pane, window $exit is also useful.*

| Command | Usage |
| --- | --- |
| prefix q | View pane num |
| prefix z | Zoom pane and back|
| prefix w |List windows |
| prefix { | Rotate current pane |


# Menu bar parameter

Example: When `[3] 0:bash* 1:test-Z` is shown at the left bottom of menu bar,

| Content | Meaning                              |
| :-----: | ------------------------------------ |
|   [3]   | Client number is 3.                  |
| 0:bash  | 0th window with name bash            |
|   \*    | Currently shown                      |
| 1:test  | 1st window with name test            |
|   -Z    | One pane is maximized in this window |


# Move a pane to another window

1. Change to command mode with `C-b, :`

2. Enter `:join-pane  [-s src-pane] [-t dst-pane]`

ex. Move current pane to window[1]

`join-pane -t :1`

Note that `:` is required before the window number.
