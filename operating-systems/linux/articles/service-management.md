# Service Management



### Creating a Linux service with systemd

- Create a **Unit file** to define a systemd service on /etc/systemd/system/<service_name>.service

```shell
[Unit]
Description=ROT13 demo service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=centos
ExecStart=/usr/bin/env php /path/to/server.php

[Install]
WantedBy=multi-user.target
```

- Configuration options

`After`:  It simply means that your service must be started *after* the network is ready. If your program expects the MySQL server to be up and running, you should add `After=mysqld.service`

`Restart`: By default, systemd does not restart your service if the program exits for whatever reason. This is usually not what you want for a service that must be always available, so we’re instructing it to always restart on exit. `Restart=always`

`RestartSec`: By default, systemd attempts a restart after 100ms. You can specify the number of seconds to wait before attempting a restart, using `RestartSec=1`. It’s a good idea to set `RestartSec` to at least 1 second though, to avoid putting too much stress on your server when things start going wrong.

`StartLimitBurst`: when you configure `Restart=always` as we did, **systemd gives up restarting your service if it fails to start more than 5 times within a 10 seconds interval**. `StartLimitBurst=5
StartLimitIntervalSec=10`



- Start Service

```shell
$ systemctl start <service_name>
```



- Service automatically start on boot

```shell
$ systemctl enable <service_name>
```



### Create a Custom systemd ServicePermalink

1. Create a script or executable that the service will manage. This guide uses a simple Bash script as an example

    ```shell
    DATE=`date '+%Y-%m-%d %H:%M:%S'`
    echo "Example service started at ${DATE}" | systemd-cat -p info

    while :
    do
    echo "Looping...";
    sleep 30;
    done
    ```

2. Copy the script to `/usr/bin` and make it executable:

   ```shell
   sudo cp test_service.sh /usr/bin/test_service.sh
   sudo chmod +x /usr/bin/test_service.sh
   ```

3. Create a **Unit file** to define a systemd service: myservice.service

   ```shell
   [Unit]
   Description=Example systemd service.
   
   [Service]
   Type=simple
   ExecStart=/bin/bash /usr/bin/test_service.sh
   
   [Install]
   WantedBy=multi-user.target
   ```

   This defines a simple service. The critical part is the `ExecStart` directive, which specifies the command that will be run to start the service.

4. Copy the unit file to `/etc/systemd/system` and give it permissions:

   ```shell
   sudo cp myservice.service /etc/systemd/system/myservice.service
   sudo chmod 644 /etc/systemd/system/myservice.service
   ```

5. Start and Enable the Service

   ```shell
   sudo systemctl start myservice
   sudo systemctl status myservice
   # use the enable command to ensure that the service starts whenever the system boots
   # close by systemctl disable myservice
   sudo systemctl enable myservice 
   ```

6. Reboot and check the status of the service

   ```shell
   sudo systemctl status myservice
   ```




### Spring Boot Project as Systemd Service

```shell
[Unit]
Description=hotcrawler
After=syslog.target

[Service]
User=root
ExecStart=/usr/bin/java -jar /var/myapp/hotcrawler.jar
SuccessExitStatus=143
Restart=always
RestartSec=1
StartLimitBurst=5
StartLimitIntervalSec=10

[Install]
WantedBy=multi-user.target
```

2

```shell
[Unit]
Description=hotcrawler
After=syslog.target

[Service]
User=root
ExecStart=/usr/bin/java -jar /var/myapp/hotcrawler.jar
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target
```



References

- [Creating a Linux service with systemd - medium](https://medium.com/@benmorel/creating-a-linux-service-with-systemd-611b5c8b91d6)

- [Use systemd to Start a Linux Service at Boot](https://www.linode.com/docs/quick-answers/linux/start-service-at-boot/)

- [Installing Spring Boot Applications - Spring Boot Doc](https://docs.spring.io/spring-boot/docs/current/reference/html/deployment-install.html)