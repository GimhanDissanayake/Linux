# Linux
### All the values in < > need to be replaced with yours
- Log in to bash shell
```
ssh <user_name>@<ip_address>
```

# Stop Prompting for PASSWD all the time
- edit the sudovers.d file
```
sudo visudo
```
- add the following line to the end of doc
```
<user_name> ALL=(ALL) NOPASSWD: ALL
```
- save and exit
- run following command to remove cashed passwd
```
sudo -k
```
- check weather it works!
```
sudo ls
```
