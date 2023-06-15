#!/bin/bash 

#Welcome to the usercreamod script, this script allows you to create users and/or modify them. This script will only function properly if you have superuser privilges. 
read -p Would you like to create or modify a user? [create/mod] '
case $REPLY in
        create)
                read -p 'Please enter the name of the user you would like to add to the server: ' user
                sudo useradd $user && read -p "You have successfully added $user to the server, would you like to modify this user? [yes/no] " REPLY1
              if [ "$REPLY1" = "yes" ]
              then
        source usercreamod.sh
              elif [ "$REPLY1" = "no" ]
              then
        echo Okay, have a good day!
              fi
              ;;
         mod)
                 read -p 'Please enter the username that you would like to modify: ' user
                 read -p 'How would you like to modify this user? You can assign them a UID #, group, password, or superuser privileges [uid/group/password/superuser] ' usermod
                        case "$usermod" in
                             uid)
                                 read -p "Please enter the UID # you would like to assign to $user: " UIDN
                                 sudo usermod -u $UIDN $user && read -p "You have successfully assigned the UID #$UIDN to $user. Is there anything else you would like to modify about $user? [yes/no] " modagain
                                 if [ "$modagain" = "yes" ]
                                 then
                                 source usercreamod.sh
                                 elif [ "$modagain" = "no" ]
                                 then
                                        echo Okay, have a good day!
                                 fi
                                 ;;
                              group)
                                read -p "Would you like to assign a primary group or a secondary group to $user? [primary/secondary] " group
                                if [ "$group" = "primary" ]
                                then
                                        read -p "What is the name of the group you want to assign to be $user's primary group? " pri
                                        sudo usermod -g $pri $user && read -p "You have successfully assigned $pri as $user's primary group. Is there anything else you would like to modify about $user? [yes/no] " group1
                                        if [ "$group1" = "yes" ]
                                        then
                                                source usercreamod.sh
                                        fi
                                        if [ "$group1" = "no" ]
                                        then
                                                echo Okay, have a good day!
                                        fi
                                 fi
                                 if [ "$group" = "secondary" ]
                                 then
                                         read -p "What is the name of the group you want to assign to be $user's secondary group? " sec
                                         sudo usermod -aG $sec $user && read -p "You have successfully assigned $sec as $user's secondary group. Is there anything else you would like to modify about $user? [yes/no] " group2
                                         if [ "$group2" = "yes" ]
                                         then
                                                 source usercreamod.sh
                                         fi
                                         if [ "$group2" = "no" ]
                                         then
                                                 echo Okay, have a good day!
                                         fi
                                 fi
                                 ;;
                              password)
                                      read -p "You are choosing to assign a password to $user, proceed? [yes/no] " passwd
                                      if [ "$passwd" = "yes" ]
                                        then
                                 sudo passwd $user && read -p "You have successfully assigned a password to $user. Is there anything else you would like to modify about $user? [yes/no] " modagain
                                 if [ "$modagain" = "yes" ]
                                 then
                                 source usercreamod.sh
                                 fi
                                 if [ "$modagain" = "no" ]
                                 then
                                 echo Okay, have a good day!
                                 fi
                                      fi
                                 if [ "$passwd" = "no" ]
                                 then
                                         read -p "Is there anything else you would like to modify about $user? [yes/no] " passwd1
                                         if [ "$passwd1" = "yes" ]
                                         then
                                         source usercreamod.sh
                                         fi
                                         if [ "$passwd1" = "no" ]
                                         then
                                         echo Okay, have a good day!
                                         fi
                                 fi
                                 ;;
                              superuser)
                                      read -p "You are choosing to assign superuser privileges to $user, proceed? [yes/no] " super
                                      if [ "$super" = "yes" ]
                                      then
                                              echo "$user ALL=(ALL) ALL" >> /etc/sudoers.d/$user && read -p "You have successfully assigned superuser privileges to $user. Is there anything else you would like to modify about $user? [yes/no] " modagain
                                              if [ "$modagain" = "yes" ]
                                              then
                                              source usercreamod.sh
                                              fi
                                              if [ "$modagain" = "no" ]
                                              then                                                                                                                                                                                                                                                echo Okay, have a good day!
                                              fi
                                      fi
                                      if [ "$super" = "no" ]
                                      then
                                              read -p "Is there anything else you would like to modify about $user? [yes/no] " super1
                                              if [ "$super1" = "yes" ]
                                              then
                                                      source usercreamod.sh
                                              fi
                                              if [ "$super1" = "no" ]
                                              then
                                                      echo Okay, have a good day!
                                              fi
                                      fi
            esac
esac