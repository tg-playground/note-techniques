#!/bin/bash

# Java auto-installation Script

command_name=$(basename $0)

usage () {
    echo "Usage:"
    echo "Please using \"$command_name [file_path]\""
    echo "like $command_name ~/Downloads/jdk-1.8.0.tar.gz";
    return
}

install_jdk () {
    echo "******************************"
    echo "Installing JAVA begin..."

    # Is JDK installed
    if [ -n $(java -version) ]; then
        read -p "Java is installed! Is reInstall java?(y/n):" isReinstall 
        if [ $isReinstall != y ] && [ $isReinstall != n ]; then
            while [ $isReinstall != y ] && [ $isReinstall != n ]; do
                read -p "Please input y or n:" isReinstall;
            done
        fi
    fi
    if [ $isReinstall == n ]; then
        echo "Java installation is canceled!";
        return
    fi
   
    # Is file existed
    echo "package directory is $pkg_source_directory"
    jdk_path="$pkg_source_directory/$(ls -p $pkg_source_directory | grep -v / | grep "^jdk" | head -n 1)"
    echo "package_path is $jdk_path"
    if [ ! $jdk_path ]; then
        echo "Not assigned package file path!"
        return 
    fi
    if [ ! -f $jdk_path ]; then
        echo "File $jdk_path is not exist!"
        return 1;
    fi

    # tar package
    file_type=$(file $jdk_path)

    if [[ $file_type =~ "tar archive" || $file_type =~ "gzip" ]]; then
        if [[ $jdk_path =~ .+\.gz.* ]]; then
            echo "Unzip $(basename $jdk_path)..."
            sudo tar xzf $jdk_path -C /usr/my_package;
        else
            sudo tar xf $jdk_path -C /usr/my_package;
        fi
    else
        echo "Not archive file!";
        return;
    fi

    # configuring environment
    jdk_name=$(tar tf $jdk_path | head -n 1)
    jdk_name=${jdk_name/\/*/}

    JAVA_HOME=$install_directory/$jdk_name

    if [ -d $JAVA_HOME/bin ]; then
echo "
export JAVA_HOME=$JAVA_HOME
export PATH=\$PATH:\$JAVA_HOME/bin
">>~/.profile ;
        echo "Success! You need logout or restart. Your JAVA_HOME is $JAVA_HOME"
        
    else
        echo "Fail! $JAVA_HOME/bin is invalid directory";
    fi
    echo ""
}

install_mariaDB () {
    echo "******************************"
    echo "installing mariaDB begin..."
    mariaDB_path=$pkg_source_directory/$(ls -p $pkg_source_directory | grep -v / | grep "mariadb" | head -n 1)
    echo "mariadb path is $mariaDB_path"
    if [ $mariaDB_path ] && [ -f $mariaDB_path ] ; then
        echo "Unzip $(basename $mariaDB_path)..."
        sudo tar xzf $mariaDB_path -C /usr/my_package
    else
        echo "file $mariaDB_path is not exist!"
        return
    fi

    # Configuration
    mariaDB_name=$(tar tf $mariaDB_path | head -n 1 )
    mariaDB_name=${mariaDB_name/\/*/}
    MARIADB_HOME=$install_directory/$mariaDB_name

    echo "export MARIADB_HOME=$MARIADB_HOME
export PATH=\$PATH:\$MARIADB_HOME/bin
" >> ~/.profile

    echo "installing mariaDB success!"
    echo ""
    return 
}

install_tomcat () {
    echo "******************************"
    echo "installing tomcat begin..."
    tomcat_path=$pkg_source_directory/$(ls -p $pkg_source_directory | grep -v / | grep "apache-tomcat" | head -n 1)
    echo "Tomcat path is $tomcat_path"
    echo "installing tomcat success!"
    echo ""
    return
}

install_eclipse () {
    echo "******************************"
    echo "installing eclipse begin..."
    eclipse_path=$pkg_source_directory/$(ls -p $pkg_source_directory | grep -v / | grep "eclipse" | head -n 1)
    echo "eclipse path is $eclipse_path"
    echo "installing eclipse success!"
    echo ""
    return
}

install_idea () {
    echo "******************************"
    echo "installing idea begin..."
    idea_path=$pkg_source_directory/$(ls -p $pkg_source_directory | grep -v / | grep "idea" | head -n 1)
    echo "idea path is $idea_path"
    echo "installing idea end!"
    echo ""
    return
}
########################################
# Main

install_directory="/usr/my_package"
pkg_source_directory=$1;

# Is command correct
if [ ! $pkg_source_directory ]; then 
    usage
    exit 1;
fi

# Is correct directory
if [ ! -d $pkg_source_directory ]; then
    echo "Packages directory is not exist!"
    exit 1;
fi

# Begin installation

echo "******************************"
read -p "
Selecting your want to install package:
1.JDK 2.MariaDB 3.Tomcat 4.Eclipse 5.Idea 0.ALL
" install_code

while [[ ! $install_code =~ [0-4] ]]; do
    read -p "Please input 0~4:" install_code
done

echo "install code is $install_code"

for (( i=0; i<${#install_code}; i++)); do
    case ${install_code:i:1} in
    0) install_jdk
        install_mariaDB 
        install_tomcat 
        install_eclipse 
        install_idea 
        ;;
    1) install_jdk 
        ;;
    2) install_mariaDB 
        ;;
    3) install_tomcat 
        ;;
    4) install_eclipse 
        ;;
    5) install_idea 
        ;;
    esac
done

echo "All installation process is over!"

exit
