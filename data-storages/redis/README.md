# Redis

## Starting Redis Server

```shell
$ redis-server /etc/redis.conf
```

Or

```shell
$ systemctl start redis
```



## Enable Connect by Remote IP

There are a few settings that would affect this: **bind** and **protected-mode**. They work together to provide a baseline of security with new installs.

**The server only accepts connections from clients connecting from the IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain sockets**.

**By default protected mode is enabled**.

Find the following in your `redis.conf` file and comment it out:

```
bind 127.0.0.1
```

By adding a `#` in front of it:

```
# bind 127.0.0.1
```

Or, if you would rather not comment it out, you can also add the IP of your `eth0`/`em1` interface to it, like this:

```
bind 127.0.0.1 192.168.1.57
```

Also, unless you're using password security, you'll also have to turn off protected mode by changing:

```
protected-mode yes
```

To:

```
protected-mode no
```

## Set Password

### set template password by Redis-cli

```shell
# set password
$ ./redis-cli
$ 127.0.0.1:6379> AUTH PASSWORD
(error) ERR Client sent AUTH, but no password is set
$ 127.0.0.1:6379> CONFIG SET requirepass "<your_password>"
OK
$ 127.0.0.1:6379> AUTH <your_password>
Ok
```

### Set Password by Update configuration file

#### 1. Update Configuration

```
sudo nano /etc/redis/redis.conf 
```

replace

```php
# requirepass foobared
```

with

```php
requirepass YOURPASSPHRASE
```

#### 2. restart redis-server

```shell
$ redis-server restart
```

Shutdown by redis-cli, and start it

```
# shutdown
$ ./redis-cli 
$ 127.0.0.1:6379> shutdown

# start
$ systemctl start redis
# or
$ redis-server /etc/redis.conf
```

#### 3. Verify

```shell
$ ./redis-cli
$ 127.0.0.1:6379> set key1 18
(error) NOAUTH Authentication required.
$ 127.0.0.1:6379> auth <your_passowrd>
OK
```



## Remove Password

### using redis-cli temporarily Remove 

```shell
$ ./redis-cli
$ 127.0.0.1:6379> CONFIG SET requirepass ""
```

###  Update Redis Configuration

Remove Password

```
# requirepass foobared
```

Verify

```
$ ./redis-cli
$ 127.0.0.1:6379> AUTH PASSWORD
(error) ERR Client sent AUTH, but no password is set
```

