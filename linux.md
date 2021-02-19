Linux 
===============

# Table Of Contents
- [Basic Commands](#Basic-commands-in-linux)
- [Compress and Archive](#Compress-and-Archive)
- [Input Output And Redirection](#Input-Output-and-Redirection)
- [Action in Files](#actions-in-files-search-cut-display-etc)
- [Upload in Remote Server](#upload-in-remote-server)
- [Aliases & Environment Variables](#aliases--environmentvariables)
- [Processes](#processes)

## Basic commands in linux

| Command | Description |
| ------- | ----------- |
| `ls` |list files or folders in a directory|
| `ls -al` |list all files or folders in a directory including hidden|
| `tree [folder_path]` |list in tree structure|
| `cp [file_name] [file_name] [destination_path]` |Copy all the files to destination folder|
| `mv [target_name] [new_name]` |Rename target file or folder|
| `mv [file_name] [destination_path]` |Move file to given destination|
| `file [file_name]` |Show Meta Data about the file|
| `strings [file_name]` |Show the human readable Strings|

_____________________________________________________________________

## Compress and Archive

| Command | Description |
| ------- | ----------- |
| `gzip -v [file_name]` |Compress file with gzip, -v flag for more info|
| `gzip -d [file_name]` |decompress file with gzip|
| `tar -zcf [newName].tar.gz [target_path]` |compress and archive|
| `tar -cf [newName].tar.gz [target_path]` |archive with tar|
| `tar -zxvf [filename].tar.gz` |unArchive with tar|

_____________________________________________________________________

## Input Output and Redirection

Redirection reffers to saving command output to a file

| Command | Description |
| ------- | ----------- |
| `[any_command] > [file_name]` |Save the command output to file|
| `[any_command] >> [file_name]` |Appends the command output to the file|
| `[any_command] < [file_name]` |command operation to the contents of file_name|

_____________________________________________________________________

## Actions in files (search, cut, display etc)

### Grep

Grep is used to search keywords in files
```
    ps aux | grep nginx
    grep [keyword] [file_name]
```

### Cut
Cut is used to remove letters or words.. Used as 'split' in JS
```
    strings [file_name] | grep -i john | cut -d' ' -f2
```

### tr
tr is used for replaceing some characters
```
    grep bob /etc/passwd | tr ':' ' '
```

### column
column is used to display file content as table
```
    grep bob /etc/passwd | tr ':' ' ' | column -t
```

_____________________________________________________________________

## upload in remote server

### SFTP

1. Connect to Remote server
```
sftp root@<remoteIP>
```
2. Upload File into server
```
    cd [destination path in server]
    put [localFilePath]
```

### SCP
Send directly to server with scp
```
    scp [local_file_path] root@<ipAddress>:/[remote_path]
```
_____________________________________________________________________

## Aliases & EnvironmentVariables

| Command | Description |
| ------- | ----------- |
| `alias` |List all alias|
| `alias [new_command]=[existing_command]` |Create new alias|
| `unalias [command]` |Removes alias|
| `env` |Show all Environment Variables|
| `echo $[VARIABLE]` |Show Environment Variable|
| `export [VARIABLE]='...'` |Create Or Change Environment Variables|
| `unset [VARIABLE]='...'` |Remove Environment variable|
_____________________________________________________________________

## Processes
| Command | Description |
| ------- | ----------- |
| `ps -ef` |List all Processes|
| `pstree` |List all Processes in tree structure|
| `top` |Open task Manager|
| `htop` |Open task Manager|
| `kill [process_PID]` |Kill a background process|
_____________________________________________________________________

## Schedule Task With Cron 

Cron is used to Schedule Tasks.
```
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon, tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  *

* * * * * echo 'Hello' >> /tmp/text.txt
```

| Command | Description |
| ------- | ----------- |
| `crontab [fileName]` |Install Tasks from file|
| `crontab -l` |List all cron Tasks|
| `crontab -e` |Edit Cron Tasks|
| `crontab -r` |Remove all cron jobs|

_____________________________________________________________________

## Linux Boot Process


### Linux Boot Loaders

Main work of Boot loaders is to start the operating system

- Boot Loaders start the operating system
- Boot loaders can start the operating system with different options
- LILO (Linux Loader) Old boot loader used by systems
- GRUB (Grand Unified Bootloader) replaces LILO present day

### initrd Initial RAM Disk

Boot Loader uses Initial RAM Disk in its process
- Temporary filesystem that is loaded from disk and stored in memory
- Contains  helpers and modules required to load the permanent OS file system

### /boot Directory

This directory contains all files required to boot Linux

- initrd.img-x.xx.x-xx-generic (Initial RAM Disk)
- vmlinuz-x.xx.x-xx-generic (linux kernal)


### Runlevels/ Description

- Init Scripts starts other processes in each Run levels