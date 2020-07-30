### Commands to operate Client, Window and Pane

**bold command is with prefix C-b**

| | Client | Window | Pane |
| ---- | ---- | ---- | ---- |
| New  | $tmux| **c**    | **"**, **%** |
| Open | $tmux a [-t num]|- |- |
| Kill | $tmux kill-session -t [num] | **&** | **x** |
| Move | **(**, **)**, **L**|**p**, **n**, **[num]** | **o**, **Up**, **Down**|
| Rename | **$**  | **,**    |       | 

| Command | Usage |
| --- | --- |
| C-b, **q** | View pane num |
| C-b, **z** | Zoom pane and back|
| C-b, **w** |List windows |


To kill pane, $exit is also useful.

### Menu bar parameter

example: If `[3] 0:bash* 1:test-Z` is shown at the left bottom of menu bar. Meanings for each expression are below.

* [3] client number is 3.
* 0:bash 0th window with name bash
* \* currently shown
* 1:test 1st window with name test
* -Z one pane is maximized.

