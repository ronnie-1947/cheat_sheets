Linux 
===============

# Table Of Contents
- [Basic Commands](#Basic-commands-in-linux)
- [Compress and Archive](#Compress-and-Archive)
- [Input Output And Redirection](#Input-Output-and-Redirection)

## Basic commands in linux

| Command | Description |
| ------- | ----------- |
| `ls` |list files or folders in a directory|
| `ls -al` |list all files or folders in a directory including hidden|
| `tree [folder_path]` |list in tree structure|
| `cp [file_name] [file_name] [destination_path]` |Copy all the files to destination folder|
| `mv [target_name] [new_name]` |Rename target file or folder|
| `mv [file_name] [destination_path]` |Move file to given destination|

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


## Search In Files
