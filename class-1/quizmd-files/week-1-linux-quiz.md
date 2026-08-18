# Week 1 Linux Quiz

## Question 1

What does the `pwd` command show?

- The full path of the current directory
- All installed packages
- The current user's password
- The public IP address of the VM

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: `pwd` means print working directory. It shows where you are in the filesystem.

## Question 2

Which command lists files and folders in the current directory?

- `ls`
- `cd`
- `pwd`
- `cat`

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: `ls` lists the contents of a directory.

## Question 3

What does `cd ..` do?

- Moves one directory up
- Deletes the current directory
- Creates a new directory
- Prints the current directory

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: `..` means the parent directory, so `cd ..` moves one level up.

## Question 4

Which command creates a folder called `jedi`?

- `mkdir jedi`
- `touch jedi`
- `cat jedi`
- `rm jedi`

Answer: 1
Imposters: 4
Type: single
Time: 30
Explanation: `mkdir` means make directory.

## Question 5

What does `cat jedi/luke_copy.txt` do?

- Prints the file contents in the terminal
- Copies the file
- Deletes the file
- Opens the file in a browser

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: `cat` can print the contents of a text file to the terminal.

## Question 6

Why do we use `sudo apt install apache2` instead of `apt install apache2`?

- Installing software requires administrator privileges
- Apache only works with the `sudo` username
- `apt` cannot install web servers
- `apache2` is a Python package

Answer: 1
Imposters: 3
Type: single
Time: 35
Explanation: Installing system software changes the VM, so it requires administrator privileges.

## Question 7

What is Apache doing in Week 1?

- Serving web pages over HTTP
- Creating Linux users
- Running Python packages
- Managing the GCP coupon

Answer: 1
Imposters: 3
Type: single
Time: 30
Explanation: Apache is the web server. It receives browser requests and returns web pages.

## Question 8

Which folder does Apache use by default for web pages in this lab?

- `/var/www/html/`
- `/home/yoda/jedi/`
- `/etc/users/`
- `/usr/bin/apache/`

Answer: 1
Imposters: 2
Type: single
Time: 30
Explanation: Apache serves files from `/var/www/html/` by default in this lab.

## Question 9

What does `sudo usermod -aG sudo bilbo` do?

- Adds `bilbo` to the `sudo` group
- Deletes the `bilbo` user
- Changes `bilbo`'s password
- Lists all users on the VM

Answer: 1
Imposters: 3
Type: single
Time: 35
Explanation: The command adds the user `bilbo` to the `sudo` group, giving administrator privileges.

## Question 10

Why should you be careful with `sudo rm -r /home/frodo`?

- It deletes a folder recursively with administrator privileges
- It installs software globally
- It changes the VM external IP address
- It starts the Apache web server

Answer: 1
Imposters: 2
Type: single
Time: 35
Explanation: `rm -r` deletes folders recursively. With `sudo`, it has administrator privileges, so mistakes can remove important files.

