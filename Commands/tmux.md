# Terminal multiplixer(s)
Some useful commands to use with `tmux` and `screen`

### Tmux
```bash
# Starts tmux from the Mac Terminal or Linux
tmux

# Starts a new named tmux session. Replace [name] with the name of your session
tmux new -s [name]	

# Attaches to a numbered session. Replace # with the number of your session
tmux a #

# Same as above, but used with named sessions
tmux a -t [name]

# Shows the complete list of all tmux sessions.
tmux ls	

# Closes the named session. Note that this is different from detaching from a session
tmux kill-session -t [name]

# detach from a session
crtl+a, d

# Exits tmux without detaching from any sessions
exit
```

### Screen
```bash
# create a screen with specific name
screen -S <SCREEN_NAME>

# available screens
screen -ls

# attach to a screen
screen -r SCREEN_NAME

# detach from the screen
<ctrl+a> 
d

# Kill the screen session
screen -S <SESSION_ID> -X quit
```
