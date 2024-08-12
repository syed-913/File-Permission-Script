# File-Permission-Script
#This is a File permission script written in Bash language. This script iterates over the files in the current directory and change the permission of files for User, Group and Others to common permissions -rwx-user, r-x group, r-x others.This script skips files with .conf extension and directories.


#!/bin/bash

# Run a for loop to iterate over all the files in the present directory and check if the file has neccassary permissions of 755, if not than "chmod them"

for file in *
do
        #Check if the file iterating over in current directory is a regular file
        if [[ -f $file ]]; then
        #Excluding certain files with extension of .conf
                if [[ $file == *.conf ]]; then
                        echo "Skipping $file because it is a config file"
                        continue
                fi
                        #Check if the file has necessary permission (755)
                        if [[ $(stat -c %a $file) -eq 755 ]]; then
                                echo -e "\"$file\" has Necessary  permissions for user"
                        #if file doesn't have regular permission than give it
                        else
                                #A prompt confirming the change of each file from the user
                                read -p "Do you really want to change the permission for <$file>? [Y/n]" ans
                                ans=${ans,,}
                                if [[ $ans == "y" || $ans == "yes" ]]; then
                                        chmod 755 $file
                                        echo -e "Permission Modified: \"$file\""
                                        #Redirecting the infromation about file (Like when the permissions of file were changed) to a log file for future auditing sessions
                                        echo -e "Permissions Modified: \"$file\",$(stat -c %a $file),$(date +"%A %B %Y %T")" >> /home/kali/log.txt
                                elif [[ $ans == "no" || $ans == "n" ]]; then
                                        continue
                                fi
                        fi
        #if current directory contains !(not) regular file then shoq error
        else
                echo "Error: $file is not a regualr file or maybe it's a directory"
        fi
done
