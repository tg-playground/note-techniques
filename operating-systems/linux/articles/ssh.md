# SSH



### Local Windows ssh remote server using public key with Xshell

Steps

1. Use Xshell generate public key

2. Copy public key to server

   ```shell
   # find default authorizedKeysFile location
   grep "AuthorizedKeysFile" /etc/ssh/sshd_config
   # Edit the authorizedKeysFile 
   $ vim ~/.ssh/authorized_keys
   <your public key>
   ```

3. Use xshell conncet server by Public Key

   session's properties --> Connection --> Authentication, Method selected Public Key

4. Disable Password Authentication on your Server

   ```shell
   $ sudo vi /etc/ssh/sshd_config
   ...
   PasswordAuthentication no
   ...
   # restart the sshd service
   $ sudo systemctl restart sshd.service
   ```

### Local Linux ssh remote server using public key



References

[Accessing a Remote Server via Public-Key Authentication](https://blog.netsarang.com/1319/accessing-a-remote-server-via-public-key-authentication/)

[How To Set Up SSH Keys on CentOS 7](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-centos7)