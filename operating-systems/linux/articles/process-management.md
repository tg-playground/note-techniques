# Process management

### View Process

```shell
ps -ef
ps -aux
```

### View Process listen port by PID

lsof

```shell
sudo lsof -i | grep -i LISTEN | grep <pid>
```

### View Process by Port

lsof

```shell
lsof -i :<port>
lsof -i :80
```



### Kill Process

```shell
kill <PID>
kill -9 <PID>
```



### View System memory

```shell
free
```



### Process Management

When process crash, need to automatically restart it.

Insufficient system resources can cause processes to be killed. To prevent processes being permanently killed, we need to protect processes and make it automatically restart.

- service
  - **systemd**
- Process manager
  - nohup
  - forever
  - supervisor
  - pm2
- docker



#### nohup

nohup 是后台作业的意思， nohup运行的进程将会忽略终端信号运行。即后台运行一个命令。 
nohup COMMAND & 用nohup运行命令可以使命令永久的执行下去，和用户终端没有关系，例如我们断开SSH连接都不会影响它的运行。

#### Systemd service



#### Supervisor

supervisor是用Python开发的一套通用的进程管理程序，能将一个普通的命令行进程变为后台daemon，并监控进程状态，异常退出时能自动重启。之前一直使用nohup启动进程，之后接触了supervisor，感觉更为合适。

Install

```shell
apt-get install -y supervisor
yum install epel-release
# yum install -y supervisor
# systemctl enable supervisord # 开机自启动
# systemctl start supervisord # 启动supervisord服务

# systemctl status supervisord # 查看supervisord服务状态
# ps -ef|grep supervisord # 查看是否存在supervisord进程
```

Config

/etc/supervisor/conf.d

```shell
[program:mycommand]
command=cat
directory=/home/fenggese/
user=fenggese
```

Update Config

```shell
supervisorctl update
```

Start process

```shell
supervisorctl start mycommand
supervisorctl stop mycommand
```

Remove process

```shell
remove /etc/supervisor/conf.d/mycommand
```



#### pm2

#### docker





### References

Process Management

- [Running Nodejs applications in production forever vs supervisord vs pm2](https://mrvautin.com/running-nodejs-applications-in-production-forever-vs-supervisord-vs-pm2/)