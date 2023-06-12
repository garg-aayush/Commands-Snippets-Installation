# Bash
Some useful and frequently used `Bash` commands

## Compress/Decompress files
```bash
# compress the files to tar
tar -czvf file.tar.gz dataset/
# decompress the tar file
tar -xzvf file.tar.gz -C dataset/
# list tar files
tar -t file.tar.gz
```

### Processes info
```bash
# get process command
ps <pid>

# kill the process gracefully
kill <pid>

# kill the process forcefully
kill -9 <pid>

# kill multiple processes
nvidia-smi | grep 'python' | awk '{ print $5 }' | xargs -n1 kill -9
```


### Find, list, count files
```bash
# find files (recursive search by default)
find <foldername> -name *json -type f

# if too many files in a folder (count)
find <foldername> -name *json -type f | wc -l

# find and copy
find <foldername> -name *json -type f -exec cp {} ../otherfolder/ \c

# Files that are in both Dir1 and Dir2
find "$Dir1/" "$Dir2/" -printf '%P\n' | sort | uniq -d

# Files that are in Dir1 but not in Dir2
find "$Dir1/" "$Dir2/" "$Dir2/" -printf '%P\n' | sort | uniq -u

# Files that are in Dir2 but not in Dir1
find "$Dir1/" "$Dir1/" "$Dir2/" -printf '%P\n' | sort | uniq -u
```

### Network control from terminal
```bash
# check the internet availability
ping -c <n_packets> <website>

# check active connection on the device
nmcli connection

# device status
nmcli dev status

# available wifi list
nmcli device wifi list

# connect to a wifi
nmcli --ask dev wifi connect <SSID>
```
