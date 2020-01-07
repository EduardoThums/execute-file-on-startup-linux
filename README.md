# Execute file on Linux startup

## First step
Create a *.service file on ```/etc/systemd/system/``` using the code below\
```
[Service]
ExecStart=/path/to/your/shell/script/file.sh

[Install]
WantedBy=default.target
``` 
## Second step
Create a shell script file on the exactly path used on the ```.service``` and write the below code into it.\

```
#!/bin/bash

VIVO_ADS_DIR_NAME=/tmp/vivoads
UPLOADED_FILE_DIR_NAME=/tmp/vivoads/uploaded-files
 
generic_clean(){
    sudo rm -rf $1
}

generic_create(){
    sudo mkdir $1
}

generic_permissions(){
    sudo chmod 777 $1
}

generic_clean $VIVO_ADS_DIR_NAME
generic_clean $UPLOADED_FILE_DIR_NAME

generic_create $VIVO_ADS_DIR_NAME
generic_create $UPLOADED_FILE_DIR_NAME

generic_permissions $UPLOADED_FILE_DIR_NAME
```

With the shell script file created we must give the rights permissions to the file using the chmod command. \
```chmod 744 /path/to/your/shell/script/file.sh```

## Third step
We must allow the system to run the shell script without ask for sudo permissions.

Opens the visudo utility:
```sudo visudo```\
Disable sudo ask: ```<your-user-here> ALL=(ALL) NOPASSWD: /path/to/your/shell/script/file.sh```


## Fourth step
Last but not least, we must enable the service unit execute every boot.\
```
chmod 664 /etc/systemd/system/disk-space-check.service
systemctl daemon-reload
systemctl enable /path/to/your/service/unit/file.service
```
