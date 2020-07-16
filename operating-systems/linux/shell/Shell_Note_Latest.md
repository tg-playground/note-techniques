Linux note

Map 
- Linux fundational concept
- The basic command line
- Shell Script
- Others: Sed, AWK, Vim Editor

### __Chapter 0 Introduction__

### Gun Project

Its aim is to give computer users freedom and control in their computers and computing devices.Users are free to run the software.

Many orinary people own a computer when only big business and big government ran all the computers.

### Liunx

In 1991, the Kernel Linux appeared, developed outside the GUN project by Linus Torvalds.

Freedom for Linux is to know what your computer is doing, a computer is that without secrets.

### Why use Command line

The graphical user interfaces make easy tasks easy. The command line interfaces make difficult tasks possible.

### __Part 1 - Learning The Shell__

### __Chapter 1 - What Is The Shell?__

### what is the Shell?

A program, takes keyboard command to operating system. a shell program from GUN Project called bash(Bourne Again SHell).bash is an enhanced replacement for sh.

### Terminal Emulators

On GUI, a program called a terminal emulator to interact with the shell. KDE uses konsole and GNOME uses gnome-terminal.KDE and GNOME are a desktop environment.

### virtual terminal

terminal behind the graphical desktop.

	Ctrl+Alt+F1~F6 //access virtual terminal
	Alt+F7 //return to graphical desktop

### Some simple Command


	$ date
	$ cal
	$ df
	$ free
	$ exit

### __Chapter 2 - Navigation__


	$ pwd
	$ cd 
	$ ls
	$ cd   # to your home directory.
	$ cd - # to previous directory.
	$ cd ~user_name # to home directory of user_name.
	$ ls -la # long format and list all files contain hidden files.
	$ ls -lt # long format and sort by modification time 
	$ ls /usr /bin

### __Chapter 3 - Exploring the System__

	$ file # file type
	$ less # view file content.

### Long format fields
	
	-rw-rw-r-- 1 taogen taogen  1228 Dec 27 12:13 note.txt

	-rw-r--r-- // d:directory, -:file, l:symbolicLink, r:read, w:write, x:execute, user-group-everyone
	1   // File's number of hard links.
	taogen // username of file's owner.
	taogen // group name owns the file.
	1228 // Size of the file in bytes.
	Dec 27   // Date and time of the file's last modification.
	note.txt // file name.

### Directories of Linux System

/  /bin /etc /usr

### __Chapter 4 - Manipulating File and Directories__

	
$ cp file1 file2   # If file2 exist, overwritten with content of file1.
$ cp file1 file2.. directory
$ cp dir1/* dir2 
$ cp -r dir1 dir2
$ mv file1 file2 
$ mv file1 file2.. directory
$ mv dir1 dir2
$ mkdir dir1 dir2
$ mkdir -p playground/dir-{001..100}
$ rm file1 file2..
$ rm -i file1
$ rm -r dir
$ rm -rf dir #
$ ln file link # to create a hard link
$ ln -s item link # to create a symbolic link, a file or a directory

### Wildcard

	*, ?, [characters], [!characters], [[:class:]]
	eg:	
	g*, b*.txt, Data???, [abc]*, BACKUP.[0-9][0-9], 
	[[:upper:]]*,   //beginning with an uppercase letter.
	[![:digit:]]*   //not beginning with a numberal. 
	*[[:lower:]123] //ending with a lowercase letter or numberals 1,2 or 3.

### __Chapter 5 - Working with Commands__

	$ type # Display a command's type.Such as a shell builtin,a alias etc.
	$ which # Display an executable location.
	$ help # Get help for shell builtin. Such as cd.
	$ [command] --help # Display Usage information.
	$ man # Display a program's Manual page.
	$ apropos # Display appropriate commands.
	$ info # 
	$ whatis # Display a Decription of a command.
	$ type foo; alias foo='cd /usr; ls; cd -'
	$ unalias foo

### what are command?

- An executable program (binary file).
- A command built into shell itself.
- A shell function.
- An alias.

### __Chapter 6 - Redirection__

	$ cat
	$ sort
	$ uniq
	$ grep
	$ wc
	$ head
	$ tail
	$ tee

### Standard Input, Output, And Error

Standard Output and Error: Display status and error message of program on screen.

Standard Input : Input from keyboard.

I/O redirection: allows us to change where output goes where input comes from.

Redirectiong Standard Output

	$ ls -l /usr/bin > ls-output.txt 

Redirecting Standard Error

	$ ls -l /bin/usr 2> ls-error.txt

Redirecting Standard Output and Error To one File

	$ ls -l /bin/usr > ls-output.txt 2>&1
	$ ls -l /bin/usr /usr &> ls-output.txt

Disposing of Unwanted Output 
(at some time, redirecting standard output and error)

	$ ls -l /bin/usr &> /dev/null

Output a Error

	$ echo "There is a error!">&2

Redirecting Standard Input 
	
	$ cat ls-output.txt 
	$ cat > lazy_dog.txt
	ctrl+D
	$ cat < ls-output.txt

### Pipelines

Pipeline connects output of one command with input of second command.Using the pipe operator "|", the standard output of one command can be piped into standard input of another.

Redirection connect a command with a file.

	$ ls -l /usr/bin | less

### Filters

Pipelines are used to perform complex operations on data. To put several commands together into a pipeline. Its function as filter.

	$ ls /bin /usr/bin | sort | uniq -d | less
	$ ls /bin /usr/bin | sort | uniq | wc -l
	$ ls /bin /usr/bin | sort | uniq | grep zip
	$ head ls-output.txt
	$ head -n 5 ls-output.txt
	$ tail ls-output.txt
	$ ls /usr/bin | sort | tee ls.txt 
	$ ls /usr/bin | sort | tee ls.txt | grep zip

### __Chapter 7 - Seeing The World As The Shell Sees It__

### Expansion

Pathname Expansion

	$ echo *   # list all items of current directory. 
	$ echo D*
	$ echo *s
	$ echo [[:upper:]]*
	$ echo /usr/*/share
	$ echo .[!.]*

Arithmetic Expansion

	$ echo $((expression))
	$ echo $((2+2)) 
	$ echo $(((5+2) * 3 ))
	$ echo ~
	$ echo ~taogen	

Brace Expansion
	
	$ echo Front-{A,B,C}-Back
	$ echo Number-{1..5}
	$ echo {01..15}
	$ echo {Z..A}
	$ echo a{A{1,2},B{3,4}}b
	
Parameter Expansion

	$ echo $USER
	$ printenv | sort | less # display all environment variable

Command Substitution
	
	$ echo $(ls)
	$ ls -l $(which cp)
	$ file $(ls -d /usr/bin/* | grep zip)
	$ ls -l `which cp`

### Quoting

Double Quotes

	Special characters lose their special meaning,such as wildcard, and some expansion. Exception are "$", "\"(backslash), "`"(back-quote)

	$ ls -l "two words.txt" #Using double quotes, Stop the word-splitting
	$ echo this is a     test
	$ echo "this is a    test"
	$ echo "$cal"

Single Quotes

	$ echo The total is $100.00
	$ echo "The total is $100.00"
	$ echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
	
### __Chapter 8 - Advanced Keyboard Tricks__

	$ clear 
	$ histroy

### Command Line Editing

Cursor Movement Commands

	Ctrl+a //cursor to begin of the line
	Ctrl+e //cursor to end of the line
	Alt+f //forward one word.
	Alt+b //backward one word.
	Ctrl+l //Clear the screen and move cursor to top

Cutting and Pasting

	Ctrl+k //kill text end of line
	Ctrl+u //kill text beginning of line
	Alt+d //kill end of current word.
	Alt+Backspace //kill begin of current word.
	ctrl+y //insert kill text at current location.

completion   
	tab

### Using History

	$ history | less
	$ history | grep /usr/bin	
	Ctrl+r //search history
	Ctrl+j //copy from history
	Ctrl+p //previous history entry
	Ctrl+n //next history entry.

### __Chapter 9 - Permissions__

	id, chmod, umask, su, sudo, chown

### Owners, Group Membets, And Everybody Else

	$ id # Show your user account information

some files:	

	User acconts in /etc/passwd
	Groups in /etc/group
	User password in /etc/shadow


### Reading, Writing, And Executing

File Attributes

File Attribute,First character is file type.

	- //A regular file
	d //A directory
	l //A symbolic link.
	c //A character special file
	b //A block special file.

The remaining nine characters is file mode.

	(Owner)rwx (Group)rwx (World)rwx

chmod - Change File Mode

File Modes in Binary and Octal

	000~111, 0~7, 7 //rwx 5 //r-x

	$ chmod 600 foo.txt

Symbolic notation for file modes.

	permissions:r,w,x
	user:u,g,o,a 

	$ chmod u+x [file_name]
	$ chmod +x [file_name]
	$ chmod o-rw [file_name]
	$ chmod go=rw [file_name]
	$ chmod u+x,go=rx [file_name]

umask - Set Default Permission

using octal notation to removed a file's mode attributes.

	Original file mode --- rw- rw- rw-
	Default Mask(0002) 000 000 000 010
	Result             --- rw- rw- r--

Everywhere a 1 appears mask the binary value, an attribute is unset.

Changing mask value

	$ umask 0022
	$ umask # display crrent mask value


### Changing Idetities

	$ su [user_name]

sudo- Execute a Command As Another User

chown - Change File Owner And Group

	$ chown [owner] [:[group]] file...
	$ chown bob file_name
	$ chown bob:group1 file_name # change file owner to bob, group to group1.
	$ chown bob: file_name # Change file owner to bob, group to bob's group. 

chgrp - Change Group Onwership
	
	$ chgrp [group_name] [file_name] # equals chown [:[group_name]] [flie_name]


### Changing Your Password

	$ passwd [user]
	$ sudo passwd root # change root user password
	$ passwd # change crrent user password 


### __Chapter 10 - Process__

	ps, top, jobs, bg, fg, kill, killall, shutdown


### Viewing Process

	$ ps # crrent terminal seesion
	$ ps x # current user process
	$ pa aux | less # all users process

Viewing Processes Dynamically
	
	$ top

### Controlling Process

Putting A Process In the Background

	$ xlogo &
	$ jobs 
	$ bg %1
	$ fg %1

	Ctrl+c //interrupt
	Ctrl+z //Terminal Stop

### Signals

The kill command doesn't kill process, rather it send them signals.Signal ate one of several ways the Opeareting System commuicates with programs.

Process, like files, have owners, you can send signals to owner of process. You must have super privileges to send signals to process that don't belong to you.

Sending Signals To Process With kill

	kill [-signal] PID


Common Signals:  
1 HUP, 2 INT, 9 KILL, 15 TERM, 18 CONT, 19 STOP.

	$ kill [PID]
	$ kill -9 [PID]

Sending Signals To Multiple Process with killall

	$ killall [-u user] [-signal] [program_name]...
	$ killall xlogo
	$ killall -9 xlogo firefox


## Part 2 - Configuration And The Environment

### __Chapter 11- The Environment__

printenv, set, export, alias

	$ printenv | less # Display all environment variables
	$ set | less # Display shell and environment variables
	$ alias # Display all alias.
	$ export PATH # tell the shell to make the contents of PATH available to child processes of this shell.
 
Some Interesting Environment Variables

	SHELL, HOME, LANG, PATH, PS, PWD, USER.

### How Is The Environment Established?

a series of configuration script called startup files, which define default environment shared by all users. More startup files in our home directory that define personal environment.

	/etc/profile # for the Bourne shell
	/etc/bash.bashrc # for interactive bash shells
	~/.bash_profile
	~/.profile # executed by the command interpreter for login shells.
	~/.bashrc # executed by bash for non-login shells.

### Modifying The Environment

A general rule, To add direcotry to PATH, or define additonal environment variables, place those changes in `~/.bash_profile`(~/.profile). 

For everything else, place the changes in `~/.bashrc`.

Add Configuration In `~/.bashrc`

	$ vim ~/.bashrc

add the following lines to the .bashrc file:

	# Change umask to make directory sharing easier
	umask 0002
	# Ignore duplicates in command history and increase
	# history size to 1000 lines
	export HISTCONTROL=ignoredups
	export HISTSIZE=1000
	# Add some helpful aliases
	alias l.='ls -d .* --color=auto'
	alias ll='ls -l --color=auto'
	# some more ls aliases
	# alias ll='ls -l'
	# alias la='ls -A'
	# alias l='ls -CF'

Activating Our Changes

.bashrc not take affect util we close our terminal session and start a new one.
However, we can force bash to re-read the modified .bashrc file with the following command:

	$ source .bashrc

### __Chapter 12 - A Gentle Introduction To vi__

Why We Should Learn vi

- vi is always available. such as a remote server or a local system.
- vi is lightweight, fast, and powerful.

Basic Usage

	- Command mode, Insert Mode
	- Save and exit
	- Moving the Cursor Around
	- Cut, Copy, Paste
	- Search, Replace,:%s/[search]/[replace]/gc
	- Editing Multiple Files, Switching Between Files


## Part 3 - Common Task And Essential Tools

### __Chapter 14 - Pakcage Mangement__

The most important determinant of Linux distribution quality is packaging system and vitality support community.

Packaging Systems

Most distributions divide two camps of packaging technologies: The Debian ".deb" camp and the Red Hat ".rpm " camp.
 
- Debian, Ubuntu,Xandros
- Fedora, CentOs, Red Hat, OpenSUSE


### How A Package System Works

Package Files

Repositories

Many packages are available in central repositories of a distribution.

Dependencies

A package requires a shared resource such as a shared library. It's said to havea dependency. 

High And Low-level Package Tools

low-level tools handle task such as installing and removing package file, and high-level tools perform metadata searching and dependency resolution.
Debian: low-dpkg, high-apt-get,aptitude.
Red Hat: low-rpm, high-yum.

### Common Package Management Tasks

Finding a Package in a Repository

	$ apt-get update
	$ apt-cache search [package_name]
	$ yum search [package_name]

Install a Package From A Repository

	$ apt-get update
	$ apt-get install package_name
	$ yum install package_name

Install A Package From A Package File

	$ dpkg --install package_file
	$ rpm -i package_file

Removing A Package

	$ apt-get remove package_name
	$ yun erase package_name

Updating Packages From A Repository

	$ apt-get update; apt-get upgrade
	$ yum update

Upgrading A Package From A Package File

	$ dpkg --install package_file
	$ rpm -U package_file

Listing Installed Packages

	$ dpkg --list # Display installed package all infor
	$ dpkg --get-selections # Display installed package name information
	$ rpm -qa

Displaying Info About An Installed Package

	$ apt-cache show package_name
	$ yun info package_name

Finding Which Package Installed A File

	$ dpkg --search file_name
	$ which package_name
	$ rpm -qf file_name


### __Chapeter 15 - Storage Media__

	mount,umount, fsck, fdisk, mkfs, fdformat, dd, genisoimage, wodim, md5sum

### __Chapter 16 - Networking__

	ping,traceroute, netstat, ftp, wget, ssh

### Examining And Monitoring A Network

ping
send ICMP ECHO_REQUEST to network hosts.

	$ ping linuxcommand.org

A properly performing network will exhibit zero percent packet loss. A successful "ping" will indicate that the element of network are good working order.


### Transporting FIle Over A NetWork
	
	ftp, wget

### Secure Comunication With Remote Host

SSh(Secure Shell)

secure communication with a remote host. 1.authenticate, 2.encrypt all of communications between local and remote host.

The first time connect a remote host need authenticity. To accept the credentials of the remote host, enter yes when prompted.

SSH package on Linux: OpenSSH.
SSH Cilent For Windows: PuTTY.

	$ ssh -p [server_port] [user]@[host_ip]
	
	$ sftp -P [server_port] [user]@[host_ip]
	$ get [remote_file_name] [local_path]
	$ get index.html /home/taogen/Downloads/index.html
	$ get -r directory
	$ sftp -P [server_port] [user]@[host_ip]:filepath .
	$ put [localfile]  .

### __Chapter 17 - Searching For Files__

	locate, find, xargs, touch, stat

### locate - Find Files The Easy Way
Just searching continuous path.The locate database is created by another program named updatedb. You best to run `$ sudo updatedb` before using locate searching recent files. Example searching ..bin/zip..
	
	$ locate bin/zip
	/usr/bin/zip
	/usr/bin/zipcloak
	...

### find - Find Files The Hard Way
using "find" to searching files. Must using * fill filename, begin with, end with, between section. However locate auto add two * in you want find item,such as `$ locate hello` actually represent `$ locate *hello*` 
Such as 

	-name '*.JPG', -name 'test.*', -name '*note.*'
	-path '/home/taogen/*hello.*'

Example

	$ find ~
	$ find ~ | wc -l
	$ find ~ -type d | wc -l
	$ find ~ -type f | wc -l
	$ find ~ -type f -name "*.JPG" -size +1M | wc -l

Operators

	$ find ~ \( -type f -not -perm 0600 \) -or \( -type d -not -perm 0700 \)

Predefined Action

	$ find ~ -type f -name '*.BAK' -delete
	$ find /home/taogen/Downloads -ls

User-Defined Actions

	- exec command {} ;
	$ find ~/Downloads -exec ls -l '{}' ';'
	$ find ~ -ok ls -l '{}' ';'

xargs
	
	$ find ~ -print | xargs ls -l
	$ echo "~/Downloads" | xargs ls -l

	$ find ~ -iname '*.jpg' -print0 | xargs --null ls -l
	//-print0 #produce null separated output.
	//-null #accept null separated input.

playground
	$ mkdir -p playgound/dir-{001..100}
	$ touch playground/dir-{001.100}
	$ find palygound -type f -name 'file-B' -exec touch
	$ find playground \( -type f -not -perm 0600 \) -or \( -type d -not -perm 0700 \)
	$ find playground \( -type f -not -perm 0600 -exec chmod 0600 '{}' ';' \) -or \( -type d -not -perm 0700 -exec chmod 0700 '{}' ';' \)

### __Chapter 18 - Archiving And Backup__

	gzip,bzip2,tar,zip,rsync

### Compressing Files

gzip

	$ gzip foo.txt
	$ gunzip foo.txt
	$ gzip -d foo.txt //d-decompress
	$ gzip -tv foo.txt //t-test,v-verbose
	$ ls -l /etc | gzip > foo.txt.gz
	$ gunzip -c foo.txt.gz | less //c-cat
	$ zcat foo.txt.gz
	
bizp2	
	$ bzip2 foo.txt
	$ bunzip2 foo.txt.bz2

### Archiving Files
tar
	$ tar cf playgound.tar playgroundDir #c(create)
	$ tar tf playground.tar  # t(list)
	$ tar rf playground.tar test.txt #r(append)
	$ tar xf palyground.tar # x(extract)
	$ tar xzvf playgound.tar.gz
	$ find playground -name 'file-A' -exec tar rf playground.tar '{}' '+'
	$ find playground -name 'file-A' | tar cf - --files-from=- | gzip > playground.tgz #- (represent standard input/output)
	$ find playground -name 'file-A' | tar czf playground.tgz -T - #-T (equals --files-from)
	$ ssh remote-sys 'tar cf - Documents' | tar xf -

zip 

	$ zip -r playground.zip playgroundDir
	$ unzip playground.zip
	$ unzip -l playground.zip //l-list
	$ find ~ -name 'file-A' | zip -@ file-A.zip # -@ (pipe a list of filename to zip )
	$ ls -l /etc/ | zip ls-etc.zip -
	$ unzip -p ls-etc.zip | less #-p(pipe, sent to standard output)
 
### Synchronizing Files
	rsync options source destination
	$ mkdir -p playground/dir-{1..10}  foo
	$ touch playground/dir-{1..10}/file-{A..Z}
	$ rsync -av playground/* foo

	

### __Chapter 19 - Regular Expressions__


grep
global regular expression print. grep seaches string with regular expression from file or standard input.

	grep [options] regex [file..]

### Traditional Wildcards (not RegularExpression)

	Traditional Wildcards: *, ?, [characters], [!characters], [[:class:]]
	$ ls my*.txt
	$ ls test?.txt
	$ ls [abcd]*.txt
	$ ls [Abcd]*.txt
	$ ls [!abcde]*.txt
	$ ls [[:upper:]]*.txt
	$ ls [![:digit:]]* .txt

	$ ls /bin > dirlist-bin.txt; ls /usr/bin > dirlist-usr-bin.txt; ls /sbin > dirlist-sbin.txt; ls /usr/sbin > dirlist-usr-sbin.txt
	$ grep -l bzip dirlist*.txt	#just list of matches filename rather text line.
	$ grep -L bzip dirlist*.txt #list of not matches files.
	

### RegularExpression Metacharacter And Literals
grep using RegularExpression need '', Traditional wildcards not need.  
Regular don't need complete decribe string, but traditional wildcards need using * to completely decribe string.
grep by RegularExpression using pipeline and using quanrifiers need add -E option.Just used the egrep program instead.

	^ $ . [ ] - ? * + { } ( ) | \

The Any Character 

	$ grep -h '.zip' dirlist*.txt # -h:just display matches text line not display filename.

And Character Class

	$ grep  '[bg]zip' dirlist*.txt

Negation

	$ grep '[^bg]zip' dirlist*.txt

Character Ranges

	$ grep '[ABCDEFG]' dirlist*.txt
	$ grep '[A-G]' dirlist*.txt	

Anchors
	
	$ grep -h '^zip' dirlist*.txt
	$ grep -h '^zip$' dirlist*
	$ grep -h '^[A-Z]' dirlist*

Literals
	
	$ grep -h '\_test\$.txt' dirlist*

Alternation
	
	$ echo "AAA" | grep -E 'AAA|BBB' # used egrep
	$ grep -Eh '^(bz|gz|zip)' dirlist*.txt
	$ grep -Eh '^bz|gz|zip' dirlist*.txt

Quantifiers

quanrifiers need add -E option.

	?, *, +, {}

	$ grep -Eh 'zip[0-9]?$' dirlist*.txt
	$ grep -Eh '^[a-z]{0,2}zip' dirlit.*.txt

### Examples

Validating A Phone List

	$ for i in {1..10}; do echo "(${RANDOM:0:3}) ${RANDOM:0:3}-${RANDOM:0:4}" >> phonelist.txt; done
	$ cat phonelist.txt
 	$ grep -E '^\([0-9]{3}\) [0-9]{3}-[0-9]{4}$' phonelist.txt
	
Finding Filenames

general files paht contains [-_.0-9a-zA-z/]

	$ find . -regex '.*[^-_.0-9a-zA-Z/].*'
	$ locate --regex 'bin/(bz|gz|zip)'

Searching For Text On less And Vim

	$ less phonelist.txt
	/^\([0-9]{3}\) [0-9]{3}-[0-9]{4}$
	
	$ vim 
	/ ([0-9]\{3\}) [0-9]\{3\}-[0-9]\{4\}
	

### __Chapter 20 - Text Processing__
	Manipulating text
	Basic: cat, sort, uniq
	Slice: cut, paste, join, 
	Comparing Text:comm, diff, patch, tr, sed, aspell


### Basic Edit Text

	cat [option] [file]
	[standard_output] | cat
	$ cat > foo.txt
	$ cat -A foo.txt
	$ cat -ns foo.txt
	$ ls | cat
	
	sort [option] [file]
	[standard_output] | sort
	$ sort > foo.txt
	$ sort file1.txt file2.txt > final_sroted_list.txt
	$ sort --key=1,1 --key=2n distros.txt
	$ sort -k 3.7nbr -k 3.1nbr -k 3.4nbr distros.txt
	$ sort -t ':' -k 7 /etc/passwd | head
	
	$ uniq foo.txt
	$ sort foo.txt | uniq
	$ sort foo.txt | uniq -c

### Slicing And Dicing

cut

	cut [option] [file]
	$ cut -f 3 distros.txt
	$ cut -f 3 distros.txt | cut -c 7-10
	$ cut -d ':' -f 1 /etc/passwd | head

paste
	
	$ paste [file1] [file2]
	$ paste distros-dates.txt distros-versions.txt
	
join

	join distros-key-names.txt distros-key-vernums.txt | head

### Comparing Text

comm

	$ comm [file1] [file2]
	$ comm file1.txt file2.txt
	$ comm -12 file1.txt file2.txt

diff

	$ diff [file1] [file2]
	$ diff file1.txt file2.txt
	$ diff -c file1.txt file2.txt
	$ diff -u file1.txt file2.txt

patch 
	
	$ patch < patchfile.txt

### Editing On The Fly

Substitution, Translation, Removing repeated, Substitution by RegularExpression, spelling checker.

tr

	[standard_output] | tr [option] [argument]
	$ echo "lowercase letters" | tr a-z A-Z #Translation letter case
	$ echo "lowercase letters" | tr [:lower:] A
	$ echo "aaabbbccc" | tr -s ab #repeated contineu characters insteads of a character.
	$ echo "abcabcabc" | tr -s ab

sed
	sed [option] [argument] filename
	[standard_output] | sed [option] 
	sed 's/regexp/replacement/' [file]
	$ echo "front" | sed 's/front/back/'
	$ echo "front" | sed 's_front_back_'
	$ echo "front" | sed '1s/front/back/'
	$ echo "front" | sed '2s/front/back/'
	$ sed -n '1,5p' distros.txt
	$ sed 's/([0-9]{2})/([0-9]{2})/([0-9]{4})$/\3-\1-\2/' distros.txt

aspell

	aspell check [textfile]
	$ aspell check foo.txt
	$ aspell -H check foo.txt #HTML checking-mode


### __Chapter 21 - Formating Output__

	nl, fold, fmt, pr, printf, 
	File format: groff, enscript, ps2pdf
	
Controling Each Line Characters Length. Display Line Number.

nl
-Number Lines. It resemble cat -n.
	
	$ nl dirstros.txt 

fold
-Wrap Each Line To A Specified Length

	fold [option] [file]
	[std_output] | fold [option]
	$ echo "The quick brown fox jumped over the lazy dog." | fold -w 12
	$ fold -w 12 test.txt

fmt
- A Simple Text Formatter

	$ fmt -cw 50 fmt-info.txt
	$ fmt -w 50 -p '# ' fmt-code.txt

pr
¨C Format Text For Printing

	$ pr -l 15 -w 65 distros.txt

printf 
¨C Format And Print Data

	$ printf "I formatted the string: %s\n" foo
	$ printf "I formatted '%s' as a string.\n" foo
	$ printf "%d, %f, %o, %s, %x, %X\n" 380 380 380 380 380 380
	$ printf "%s\t%s\t%s\n" str1 str2 str3
	$ printf "Line: %05d %15.3f Result: %+15d\n" 1071 3.14156295 32589
	$ printf "<html>\n\t<head>\n\t\t<title>%s</title>\n\t</head>\n\t<body>\n\t\t<p>%s</p>\n\t</body>\n</html>\n" "Page Title" "Page Content"

groff
	
	$ zcat /usr/share/man/man1/ls.1.gz | groff -mandoc -T ascii | head
	$ zcat /usr/share/man/man1/ls.1.gz | groff -mandoc > ~/Desktop/foo.ps
	$ ps2pdf ~/Desktop/foo.ps ~/Desktop/ls.pdf

Text To PDF
	$ dpkg --get-selections | grep enscript
	$ sudo apt-get install enscript ghostscript
	$ enscript -p output.ps input.txt #txt to ps
	$ ls -l | enscript -p output.ps #stdandard output to ps
	$ ps2pdf output.ps output.pdf #ps to pdf

### __Chapter 23 - Compiling Programs__

### Compiling A C Program
./configure, make, make install

obtaining The Source Code

	$ ftp ftp.gnu.org
	anonymous
	ftp> cd gnu/diction
	ftp> get diction-1.11.tar.gz
	ftp> bye
	$ tar xzf diction-1.11.tar.gz

Building The Program

	$ ./configure
	$ less Makefile
	$ make

Installing The Program

	$ sudo make install
	$ which diction
	$ man diction


----------------------------------------------------------------------

## Part 4 - Writing Shell Scripts

	- Variable
	- Function
	- Expression
	- Flow Control
	- Reading Keyboard
	- Exception, Trouble Shooting
	- Positional Parameter
	- String And Number
	- Arrays

### How To Write A Shell Script
1.Write a script.  
2.Make the script executable.  
3.Put the script somewhere the shell can find it.  

Script File Format

	#!/bin/bash
	# This is our first script.
	echo "Hello world!"

	$ echo $PATH
	$ export PATH=~/bin:"$PATH"
	$ . ~/.bashrc # or source ~/.bashrc

Configuring vim For Script Writing

	:syntax on #turns on syntax highlighting.
	:set hlsearch #turns on the option to highlight search results.
	:set tabstop=4 #sets the number of columns occupied by a tab character. The default is 8 columns.
	:set autoindent #turns on the ¡°auto indent¡± feature.

### Variable 


Variable And Constant

	variable=value
	$ title="System Information Report"
	$ echo $title
	$ declare -r TITLE="Page Title" #-r (read-only)
	$ CURRENT_TIME=$(date +"%x %r %Z")

NOTICE: Assignment don't have blank beside "=".

global variable 

local variable of function can't change global variable.
	#! /bin/bash
	foo=1
	fun_1 () {
		foo=2
		echo "fun_1 foo=$foo"
	}
	echo "global foo=$foo"
	fun_1
	echo "global foo=$foo"


### Function

	function fun_name {
		commands
		return
	}
	fun_name () {
		commands
		return
	}

	fun_name or $(fun_name)

Shell Functions In Your .bashrc File

Shell functions make excellent repalcement for alias.Aliases are very limited rather shell function allow anything that can be scripted.

	# report disk space
	ds () {
		echo ¡°Disk Space Utilization For $HOSTNAME¡±
		df -h
	}

### Flow Control
Branch: if, switch case
Loop: while, until, for

NOTICE: Flow control need ";"


#### if

if commands; then
	commands
elif commands; then
	commands...
fi

#! /bin/bash
x=5
if [ $x -eq 5 ]; then
	echo "x equals 5."
else
	echo "x does not equal 5."
fi
#### case
	
case word in 
	[pattern []] commands ;;	
esac
eg1:
read
case $REPLY in 
	0) echo "The number equals 0"
		;;
	1) echo "The number equals 1"
		exit
		;;
esac
eg2:
read -n 1 -p "Type a character > "
case $REPLY in
	[[:alpha:]]) echo "'$REPLY' is a alphabetic." ;;
	[[:digit:]])  ...
	[ABC][0-9])   ...
	???)          ...
	*.txt)        ...
	*)            ...
	a|A)          ...
esac


#### while

while commnads; do
	commmands;
done
eg:
# while-read: read lines from a file
while read distro version release; do
	printf "Distro: %s\tVersion: %s\tReleased: %s\n" \
		$distro \
		$version \
		$release
done < distros.txt

# while-read2: read lines from a file
sort -k 1,1 -k 2n distros.txt | while read distro version release; do
	printf "Distro: %s\tVersion: %s\tReleased: %s\n" \
		$distro \
		$version \
		$release
done

#### until
	
until commands; do
	commands;
done
eg:
count=1
until [[ $cont -gt 5 ]]; do
	echo $count;
done

Breaking Out Of A Loop

break, continue		

#### for

for variable [in words]; do
	commands
done

for: C Language Form
for (( expression1; expression2; expression3 )); do
	commands
done

$ for i in A B C D; do echo $i; done
$ for i in {A..D}; do echo $i; done
$ for i in distros*.txt; do echo $i; done
for i in $(strings $FILENAME); do
for (( i=0; i<5; i=i+1 )); do
	echo $i
done


#### Exit Status
Commands issue a value to the system when they terminate, call an exit status. The value is an integer in the range of 0 to 255. zero indicates success and any other value indicates failure.

	$ ls -d /usr/bin
	$ echo $? #0 indicates success
	$ ls -d /bin/usr
	$ echo $? #2 
	$ true
	$ echo $? #0 


### test expression

[ expression ]

File Expression

	-e file #file exist.	
	-d file #file exist and is a directory.	
	-f file #file exists and is a regular file.
	-L file #file exists and is a symbolic link.
	-s file #file exists and has a length greater than zero.
	-r file #file exists and readable
	-w file #file exists and writable
	-x file #file exists and executable
	eg:
	if [ -f ~/test.txt ]; then 
		echo "File exists!"
	fi

String Expression

	string #string is not null
	-n string #length of string greater than zero.
	-z string #length of string is zero.
	string1 = string2 or string1 == string2 #string1 and string2 are equal.	
	string1 != string2
	string1 > string2
	string1 < string2

Integer Expression

integer1 -eq integer2 #equal 
integer1 -ne integer2 #not equal.
-le #less or equal.
-lt #less than.
-ge #greater or equal.
-gt #greater than
eg:
if[ $INT -eq 0]; then 
	echo "INT is zero"
fi

#### A More Modern Version of test


`[[ expression ]]`
It is similar to test. It supports all of test expression, but it adds an important new string expression.  
`string1 =~ regex #if string1 is matched by regexp, it returns true.`
	
	#!bin/bash
	INT=-5
	if [[ "$INT" =~ ^-?[0-9]+$ ]]; then
		echo "INT is a integer."
	else
		echo "INT is not an integer." >&2
		exit
	fi

[[ == ]] operator supports pathname expansion.

	if [[ $FILE == foo.* ]]; then 
		echo "$FILE matches pattern 'foo.*' "
	fi

#### (( )) - Designed For Integers

	$ if ((1)); then echo "It is true."; fi
	$ if ((0)); then echo "It is true."; fi
	if ((INT < 0)); then
		echo "INT is negative."
	fi
	if (( ((INT % 2)) == 0)); then
		echo "INT is even."
	else
		echo "INT is odd."
	fi

#### Combining Expression
AND,OR,NOT  
test: -a, -o, !  
[[]]and(()): &&, ||, !  

	if [[ INT -ge MIN_VAL && INT -le MAX_VAL ]]; then
	if [ $INT -ge $MIN_VAL -a $INT -le $MAX_VAL ]; then
	if [[ ! (INT -ge MIN_VAL && INT -le MAX_VAL) ]]; then
	if [ ! \( $INT -ge $MIN_VAL -a $INT -le $MAX_VAL \) ]; then

#### Control Operators: Another Way To Branch

	command1 && command2, command1 || command2

	$ mkdir temp && cd temp
	$ [ -d temp ] || mkdir temp
	$ [ -d temp ] && rm -rv temp

### Reading Keyboard

	read [option] [variable..]

If no variable after the read command, a shell variable REPLY will be assigned all the input.
	
	$ read a
	$ echo $a
	$ read var1 var2 var3
	$ echo $var1 $var2 $var3
	$ read
	$ echo $REPLY

prompt

	$ read -p "Enter one or more values > "
	$ read -p "Enter a username > " user_name

Default response

	$ read -e -p "What is your user name? " -i $USER

IFS

	$ IFS=":" read user pw uid gid name home shell <<< $(grep "taogen" /etc/passwd)
	$ echo "$user $uid $gid"

### Exception

#### Syntactic Errors

Missing quotes
Missing or unexpected tokens,such as ";"

	if [ 2 > 1 ] then 

Unanticipated Expansions, such as, 

	number= 
	if [ $number == 1 ]; then #the expression[ $number==1 ] equals [ == 1 ] 

#### Testing&Debugging

comment some line scripts
add echo some statements.

### Posotional Parameters

when running a shell script, we can add some arguments. we can call the argument in shell script using $1,...$9...Besides, $0 represent program path, $(basename $0) represent program name. $# represent number of arguments. 
$? Expands to the "exit status" of the most recently executed foreground pipeline.
$! represent latest running process PID.

Editting my_shell.sh
	
	#!/bin/bash
	echo "
	program_name: $0
	Number of arguments: $#
	argument1 = $1
	argument2 = $2
	"
	exit

Running my_shell.sh

	$ ./my_shell.sh a b c d
 
#### shift - Getting Access To Many Argument

The shift command makes all parameters to move down one each it is executed.

Editting posit-param2.sh

	#!/bin/bash
	# posit-param2: display all argument
	while [[ $# -gt 0 ]]; do
		echo "Argument $count = $1"
		count=$((count+1))
		shift
	done
	
	$ ./posit-param2.sh a b c d e f g h i j k

#### Using Postional parameters With Shell Function

Editting posit-param3.sh

	#!/bin/bash
	print_params () {
		echo "\$1 = $1"
		echo "\$2 = $2"
	}
	print_params a b c

Running print_params3.sh

	$ ./print_params3
 
Complete List of positional parameters
$*, $@

Editting posit-param4.sh
	#!/bin/bash
	print_params () {
		echo "\$1 = $1"
		echo "\$2 = $2"
		echo "\$3 = $3"
		echo "\$4 = $4"
	}
	pass_params () {
		echo -e "\n" '$* :'; 
		print_params $*
		echo -e "\n" '"$*" :';
		print_params "$*"
		echo -e "\n" '$@ :';
		print_params "$*"
		echo -e "\n" '"$@" :';
		print_params "$*"
	} 
	pass_params "word" "words with spaces"

Running posit-parm4.sh

	$ ./posit-param4.sh

#### A More Complete Application


	usage () {
		echo "$PROGNAME: usage: $PROGNAME [-f file | -i]"
		return
	}

	# process command line options
	interactive=
	filename=
	while [[ -n $1 ]]; do
		case $1 in
			-f | --file) shift
						  filename=$1
						  ;;
			-i | --interactive) interactive=1
						  ;;
			-h | --help) usage
						  exit
				  		  ;;

			*)          usage >&2
				          exit 1
				          ;;
		esac
		shift
	done


### Strings And Number

#### Parameter Expansion
 
Basic Parameters

$a, ${a}

	$ a="foo"
	$ echo "$a_file"
	$ echo "${a}_file"

Expansion To Manage Empty Variable

${parameter:-word}
If parameter is unset or empty, this expansion result in the value of word. it Resemble ${parameter:=word}

	$ foo=
	$ echo ${foo:-"substitute value if unset."}


${parameter:?word}
if parameter is unset or empty, exit with an error, the content of word are sent to standard error.

${parameter:+word}
If parameter is unset or empty, the expansion result in nothing. If parameter is not empty, the value of word is subtituted for parameter.


String Operations

${#parameter}
The length of parameter.

	$ foo="This string is long."
	$ echo "'$foo' is ${#foo} characters long."

${parameter:offset}

	$ echo ${foo:5}
	$ echo ${foo: -5}

${parameter:offset:length}

	$ echo ${foo:5:6}
	$ echo ${foo: -5:2}

${parameter#pattern}  
${parameter##pattern}  
pattern is a wildcard pattern like those used in pathname expansion.#  removes the shortest match. ## removes the longest match.

	$ foo=file.txt.zip
	$ echo ${foo#*.} #txt.zip
	$ echo ${foo##*.} #zip

${parameter%pattern}
${parameter%%pattern}
It resemble # and ##, but remove text from the end of the parameter.
	
	$ foo=file.txt.zip
	$ echo ${foo%.*} #file.txt
	$ echo ${foo%%.*} #file

${parameter/pattern/string}
${parameter//pattern/string}
${parameter/#pattern/string}
${parameter/%pattern/string}
search-and-replace the parameter

	$ foo=JPG.JPG
	$ echo ${foo/JPG/jpg} #jpg.JPG
	$ echo ${foo//JPG/jpg} #jpg.jpg
	$ echo ${foo/#JPG/jpg} #jpg.JPG
	$ echo ${foo/%JPG/jpg} #JPG.jpg

Case Conversion

${parameter,,}
All letters to lowercase
${parameter,}
First letter to lowercase
${parameter^^}
All letters to uppercase.
${parameter^}
First letter to uppercase.

	$ foo=aBc
	$ echo ${foo^^} ${foo,,}
	
#### Arithmetic Evaluation And Expansion

Number Bases

decimal number
octal 0number
Hexadecimal 0xnumber
any base#number

	$ echo $((0xff))
	$ echo $((2#11111111))

Simple Arithmetic

	$ echo $(( 5 / 2 ))
	$ echo $(( 5 % 2 ))

Assignment

	$ foo=1
	$ echo $(( foo = 5 ))
	$ echo $foo
	$ echo $((foo++))	
	$ echo $((++foo)
	
Bit Operations

	$ echo $((2<<1))

Logic

	$ if ((2>1)); then echo "true"; fi
	$ if (( 2>1 && 2>3 )); then echo "both is true"; fi
	$ a=0
	$ echo $((a<1?++a:--a))


### Arrays

Creating An Array

	$ a[1]=foo
	$ echo ${a[1]}
	$ days=(Sun Mon Tue Wed Thu Fri Sat)
	$ days=([0]=Sun [1]=Mon [2]=Tue [3]=Wed [4]=Thu[5]=Fri [6]=Sat)
	$ echo ${days[0]}

#### Array Operations

Outputting Entire Content Of An Array

The subscripts * and @ can access every element in an array.

	$ animals=("a dog" "a cat" "a fish")
	$ for i in ${animals[*]}; do echo $i; done
	$ for i in ${animals[@]}; do echo $i; done
	$ for i in "${animals[*]}"; do echo $i; done
	$ for i in "${animals[@]}"; do echo $i; done #best
	a dog
	a cat
	a fish

Determining The Number Of Array Elements

	$ a[100]=foo$ echo 
	${#a[@]} # number of array elements
	$ echo ${#a[100]} # length of element 100

Finding The Subscripts Used By An Array

	$ foo=([2]=a [4]=b [6]=c)
	$ for i in "${foo[@]}"; do echo $i; done
	$ for i in "${!foo[@]}"; do echo $i; done

Adding Elements To The End Of An Array

	$ foo=(a b c)
	$ echo ${foo[@]}
	$ foo+=(d e f)
	$ echo ${foo[@]}

Sorting An Array

	$ a=(f e d c b a)
	$ a_sorted=($(for i in "${a[@]}"; do echo $i; done | sort))
	$ echo "Sorted array: ${a_sorted[@]}"

Deleting An Array

To delete an array, use the unset command. unset may also be used to delete single array elements.  
assignment of an empty value to an array does not empty

	$ foo=(a b c d e f)
	$ unset foo
	$ echo ${foo[@]}	
	$ echo ${foo[@]}

Associative Arrays

Associative arrays use strings rather than integers as array indexes.

	$ declare -A colors
	$ colors["red"]="#ff0000"
	$ colors["green"]="#00ff00"
	$ colors["blue"]="#0000ff"
	$ echo ${colors["blue"]}

### Exotica

#### Group Command

Group command:  
{ command1; command2; command3;..}
Subshell:  
(command1; command2; command3;...)

	$ ls -l > output.txt
	$ echo "Listing of foo.txt" >> output.txt
	$ cat foo.txt >> output.txt
	
	$ { ls -l; echo "Listing of foo.txt"; cat foo.txt; } > output.txt
	$ (ls -l; echo "Listing of foo.txt"; cat foo.txt) > output.txt
	$ { ls -l; echo "Listing of foo.txt"; cat foo.txt; } | lpr


Process Substitution

group commands are preferable to subshells. Group commands are both faster and require less memory.

an example of the subshell environment problem
	
	echo "foo" | read
	echo $REPLY

The content of the REPLY variable is always empty because the read command is executed in a subshell, and its copy of REPLY is destroyed when the subshell terminates.  

Because commands in pipelines are always executed in subshells, any command that assigns variables will encounter this issue. Fortunately, the shell provides an exotic form of expansion called process substitution that can be used to work around this problem.

	For processes that produce standard output:
	<(list)
	or, for processes that intake standard input:
	>(list)

	$ read <  <(echo "foo")
	$ echo $REPLY
	$ echo <(echo "foo")
	
#### Traps

	trap argument signal..

some signal:  
SIGINT:interrupt by ctrl+C  
SIGTERM: terminate by kill  

	#!/bin/bash
	# trap-demo : simple signal handing demo
	trap "echo 'I am ignoring you.'" SIGINT SIGTERM
	for i in {1..5}; do
		echo "Iteration $i of 5"
		sleep 5
	done

	#!/bin/bash
	# trap-demo : simple singal handing demo
	exit_on_signal_SIGINT () {
		echo "Script interrupt." 2>&1
		exit 0
	}	
	exit_on_signal_SIGTERM () {
		echo "Script terminated." 2>&1
		exit 0
	}
	trap exit_on_signal_SIGINT SIGINT
	trap exit_on_signal_SIGTERM SIGTERM
	for i in {1..5}; do
		echo "Iteration $i of 5."
		sleep 5
	done


#### Asynchronous Execution

	A parent script must wait child script to finish, it can continue running.

	#!/bin/bash
	#async-parent : Asynchronous execute demo (parent)
	echo "Parent: starting..."
	echo "Parent: launching child script..."
	async-child &
	pid=$!
	echo "Parent: child (PID=$pid) launched."
	echo "Parent: continuing..."
	sleep 2
	echo "Parent: pausing to wait for child to finish..."
	wait $pid
	echo "Parent: child is finished. Contining..."
	echo "Patent: parent is done."

	#!/bin/bash
	# async-child : Asynchronous execution demo (child)
	echo "Child: child is running..."
	sleep 5
	echo "Child: child is done. Exiting."
	

#### Named Pipes
Named Pipes like web browser communicating with a web server.
process1 > named_pipe  
process2 < named_pipe  
as if  
process1 | process2  

create a named pipe

	$ mkfifo pipe1

Using Named Pipes

	$ ls -l > pipe1 #In the first terminal.just press Enter key to wait, then using another terminal to read the result of ls -l.
	$ cat < pipe1 #open second terminal, to read input frome pipe1.

	$ ls -l | cat
	
--END--






