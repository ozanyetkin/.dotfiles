Custom dotfiles for customizing Arch Linux

# My Dotfiles Backup Script

This guide will help you create a backup of your dotfiles using a simple shell script.

## 1. Create a Folder for the Backup

```
mkdir my-dotfiles
```

## 2. Initialize Git

```
cd my-dotfiles
git init
```

## 3. Create File List

Create a file named `backup.conf` at the root of the folder. In this file, list out all the files & folders you need to backup. For example:

```
~/.gitconfig
~/.profile
~/.vimrc
~/.xinitrc
~/.zshrc
```

Our backup script will loop through the items and copy everything into respective folders.

## 4. Create Script

Create a file named `backup.sh` at the root of the folder. This is our script file. Copy the content below and paste it inside the file.

```
#!/bin/sh

# This script copies files mentioned inside `backup.conf` to the root of the project.

# File to look for the paths to backup.
backupPaths="./backup.conf"
# Home directory path.
homeDirectory=~
# Same line identifier to echo in the same line.
sameLine="\e[1A\e[K"

echo "🛑 Clearing configurations directory..."
# Removing the folder with existing contents. We have git version anyway!
rm -rf configurations
# Creating it again for backup.
mkdir configurations
sleep 1
echo -e "$sameLine✅ Configurations directory cleared."
sleep 1

echo -e "$sameLine🏁 Starting backup..."
sleep 1

# Looping through the list & avoiding the empty spaces
sed '/^[ \t]*$/d' $backupPaths | while read filePath; do
  echo -e "$sameLine⏳ Copying: $filePath"

  # Find & replace for ~ with home path
  findThis="~/"
  replaceWith="$homeDirectory/"
  originalFile="${filePath//${findThis}/${replaceWith}}"

  # Copying the files
  cp --parents --recursive $originalFile ./configurations
  sleep 0.05
done

git add .

echo -e "$sameLine🎉 Backup finished! You can review & commit your changes."
```

## 5. Make the Script Executable

```
chmod +x ./backup.sh
```

## 6. Commit All Changes

```
git add .
git commit -m "initial commit"
```

## 7. Run Script

```
./backup.sh
```

Now, your dotfiles backup script is ready to use. Run the script whenever you want to back up your configuration files.
