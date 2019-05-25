# Ubuntu specify user directory 
1. use adduser command
```sh
    adduser username
```
You have to use -m, otherwise no home directory will be created. If you want to specify the path of the home directory, use -d and specify the path:
```sh
    useradd -m -d /PATH/TO/FOLDER USERNAME
```
2. You can then set the password with:
```sh
    passwd USERNAME
```
All of the above need to be run as root, or with the sudo command beforehand. For more info, run
```sh
    man adduser.
```
