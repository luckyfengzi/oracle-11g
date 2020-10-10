Image for running Oracle Database 11g Standard/Enterprise. Due to oracle license restrictions image is not contain database itself and will install it on first run from external directory.

``This image for development use only``

# Usage
Download database installation files from [Oracle site](http://www.oracle.com/technetwork/database/in-memory/downloads/index.html) and unpack them to **install_folder**.
Run container and it will install oracle and create database:

```sh
docker run --privileged --name oracle11g -p 1521:1521 -v <install_folder>:/install jaspeen/oracle-11g
```
Then you can commit this container to have installed and configured oracle database:
```sh
docker commit oracle11g oracle11g-installed
```

Database located in **/opt/oracle** folder

OS users:
* root/install
* oracle/install

DB users:
* SYS/oracle

Optionally you can map dpdump folder to easy upload dumps:
```sh
docker run --privileged --name oracle11g -p 1521:1521 -v <install_folder>:/install -v <local_dpdump>:/opt/oracle/dpdump jaspeen/oracle-11g
```
To execute impdp/expdp just use docker exec command:
```sh
docker exec -it oracle11g impdp ..
```

# Other
If the dbca creating database task was halted at 76%, the reason perhaps is insufficient shm-object space. For more detail you can refer to [this site](https://www.codetd.com/article/7627695) .

Solution: Execute the following shell code before creating database task.

```sh
echo "tmpfs /dev/shm tmpfs defaults,size=1g 0 0" >> /etc/fstab
mount -o remount /dev/shm
```