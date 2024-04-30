We will be creating a git repository with a script that backup your required dotfiles.
1. Create a folder for the backup

mkdir my-dotfiles

2. Initialize git

git init

3. Create file list

Create a file named backup.conf at the root of the folder.
In this file we need to list out all the files & folders we need to backup. eg:

~/.gitconfig
~/.profile
~/.vimrc
~/.xinitrc
~/.zshrc

Our backup script will loop through the items and copy everything into respective folders.
4. Create script

Create a file named backup.sh at the root of the folder.
This is our script file. Copy the content below and paste it inside the file.

#!/bin/sh

# This script copy files mentioned inside `backup.conf` to the root of the project.

# file to look for the paths to backup.
backupPaths="./backup.conf"
# home directory path.
homeDirectory=~
# same line identifier to echo in the same line.
sameLine="\e[1A\e[K"

echo "🛑 Clearing configurations directory..."
# removing the folder with exsiting contents. we have git version anyway!
rm -rf configurations
# creating it again for backup.
mkdir configurations
sleep 1
echo -e "$sameLine✅ Configurations directory cleared."
sleep 1

echo -e "$sameLine🏁 Starting backup..."
sleep 1

# looping through the list & avoiding the empty spaces
sed '/^[ \t]*$/d' $backupPaths | while read filePath; do
  echo -e "$sameLine⏳ Copying: $filePath"

  # find & replace for ~ with home path
  findThis="~/"
  replaceWith="$homeDirectory/"
  originalFile="${filePath//${findThis}/${replaceWith}}"

  # copying the files
  cp --parents --recursive $originalFile ./configurations
  sleep 0.05
done

git add .

echo -e "$sameLine🎉 Backup finished! You can review & commit your changes."

5. Make the script executable

chmod +x ./backup.sh

6. Commit all changes

git add .
git commit -m "initial commit"

7. Run script

./backup.sh
