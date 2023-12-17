# tmux 

`tmux` is a terminal multiplexer. It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal. And do a lot more.

## Installation

```bash
sudo apt install tmux
```

or on Mac

```bash
brew install tmux
```

## Usage

Let's assume you are via ssh on a remote server and you want to run a long running process. You can use `tmux` to run the process in a session and then detach from it. You can then log out and log back in later to check on the process. Your process will still be running, even if your ssh session is closed.

### On the remote server

```bash
tmux new -s myprocess
```

Then run your process. When you are done, detach from the session by pressing `Ctrl+b` and then `d`.

### On the remote server later

```bash
tmux ls
```

This will list all the sessions. You can then reattach to the session you want by typing:

```bash
tmux attach -t myprocess
```

### Panes

You can split your terminal into panes. This is useful if you want to run multiple processes in the same terminal. You can split the terminal vertically by pressing `Ctrl+b` and then `"` or horizontally by pressing `Ctrl+b` and then `%`.

To move panes around, you can use `Ctrl+b` and then `o` to cycle through the panes.