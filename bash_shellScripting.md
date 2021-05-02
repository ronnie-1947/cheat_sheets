## Begin a Script
- Make a file with any extension .sh .txt
- change file permisson to executeable and writable

_____________

## Variables

```
# Set a variable
NAME="any_name"

# use the variable
echo "Hello my name is ${NAME}"

# Reassign the variable
NAME="other_name"

# put command output to a variable. Works in both ways
USER_NAME=$(id -un) || USER_NAME=`id -un`
```

_____________

## If Else Condition
[[ ]] available only on bash. Works only on bash shell

More resource on https://linuxize.com/post/bash-if-else-statement/

```
if [[ "${UID}" -eq 0]]
then
    echo "You are root"
else
    echo "You are not root"
fi
```
_____________

