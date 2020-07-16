# User Management



### Create a Sudo user

#### Debian

1. Log in your server as the `root` user

   ```shell
   local$ ssh root@server_ip_address
   ```

2. Use the `adduser` command to add a new user

   ```shell
   $ adduser yourname
   ```

   - Set and confirm the new user's password.

     ```
     Set password prompts:
     Enter new UNIX password:
     Retype new UNIX password:
     passwd: password updated successfully
     ```

   - Set user's information. (press ENTER for the default)

     ```
     Changing the user information for username
     Enter the new value, or press ENTER for the default
         Full Name []:
         Room Number []:
         Work Phone []:
         Home Phone []:
         Other []:
     Is the information correct? [Y/n]
     ```
     
   - Update user password

     ```shell
     passwd <username>
     ```

     

3. Use the `usermod` command to add the user to the `sudo` group

   ```shell
   $ usermod -aG sudo <username>
   ```

   By default, on Ubuntu, members of the `sudo` group have sudo privileges.

4. Test `sudo` access on new user account

   ```
   # switch to new user account
   $ su - yourname
   # verify `sudo` command
   $ sudo ls -la /root
   ```

   The first time you use `sudo` in a session, you will be prompted for the password of the user account. Enter the password to proceed.

   ```
   Output:
   [sudo] password for username:
   ```

#### Red Hat

1. Log in to the system as the `root` user.

2. Create a normal user account using the `useradd` command.

   ```shell
   $ useradd USERNAME
   ```

3. Set a password for the new user using the `passwd` command.

   ```shell
   $ passwd USERNAME
   Changing password for user USERNAME.
   New password: 
   Retype new password: 
   passwd: all authentication tokens updated successfully.
   ```

4. Run the `visudo` to edit the `/etc/sudoers` file. This file defines the policies applied by the `sudo` command.

   ```shell
   $ visudo
   # Find the lines in the file that grant sudo access to users in the group wheel when enabled.
       ## Allows people in group wheel to run all commands
       # %wheel        ALL=(ALL)       ALL
   # Remove the comment character (#) at the start of the second line. This enables the configuration option.
   # Save your changes and exit the editor.
   ```

5. Add the user you created to the `wheel` group using the `usermod` command.

   ```shell
   $ usermod -aG wheel USERNAME
   ```

6. Test that the updated configuration allows the user you created to run commands using `sudo`.

   ```shell
   # Use the su to switch to the new user account that you created.
   $ su USERNAME -
   # Use the groups to verify that the user is in the wheel group.
   $ groups
   USERNAME wheel
   # Use the sudo command to run the whoami command. 
   $ whoami
   USERNAME
   $ sudo whoami
   root
   ```

   

References

[How To Create a Sudo User on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart)

[Configuring `sudo` Access - Red Hat](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/2/html/Getting_Started_Guide/ch02s03.html)