### Week 1 Part 1: Introduction to Linux and Apache

This lab introduces the Linux command line and shows you how to deploy a simple web page with Apache on a GCP virtual machine.

#### What You Will Learn

- How to connect to your VM.
- How to use basic Linux commands.
- How to create folders and files.
- How to copy, view, and remove files.
- How to install Apache.
- How to publish a simple HTML page.

#### Before You Start

Complete [Week 0](../class-0/README.md) first.

Create a new Ubuntu VM in GCP using the Week 0 instructions. For this lab, name the VM:

```text
week-1
```

Make sure HTTP and HTTPS traffic are enabled when you create the VM.

Connect to the VM using the **SSH** button in GCP.

> [!NOTE]
>
> Type the commands yourself where possible. Copying from documents into a terminal can introduce invisible formatting errors.

#### Part A: Basic Linux Commands

1. Print your current directory:

```bash
pwd
```

2. List the files and folders in your current directory:

```bash
ls
```

> [!TIP]
>
> You have seen `pwd` and `ls`. How could you print your current location and then list the files in that location?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> pwd
> ls
> ```
>
> </details>

3. Create a new user called `yoda`:

```bash
sudo adduser yoda
```

Enter a password when prompted. The password will not appear on the screen while you type. You can press `Enter` to skip the optional user details, then type `Y` to confirm.

4. Add `yoda` to the `sudo` group:

```bash
sudo usermod -aG sudo yoda
```

5. Switch to the new user:

```bash
sudo su - yoda
```

6. Confirm your current directory:

```bash
pwd
```

You should be in:

```text
/home/yoda
```

> [!TIP]
>
> You switched to `yoda`. How can you check both your username and your current folder?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> whoami
> pwd
> ```
>
> </details>

7. Clear the terminal:

```bash
clear
```

8. Create a folder called `jedi`:

```bash
mkdir jedi
```

9. Move into the `jedi` folder:

```bash
cd jedi
```

10. Confirm your current directory:

```bash
pwd
```

You should see:

```text
/home/yoda/jedi
```

11. Move back to `/home/yoda`:

```bash
cd ..
```

> [!TIP]
>
> You are in `/home/yoda` now. What command would move directly back into the `jedi` folder?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> cd jedi
> ```
>
> </details>

12. Update the package list:

```bash
sudo apt update
```

13. Install `nano`, which provides the `pico` editor used in this lab:

```bash
sudo apt install nano
```

Type `Y` if Ubuntu asks you to confirm the installation.

#### Part B: Create and Manage Files

14. Create a file called `luke.txt` inside the `jedi` folder:

```bash
pico jedi/luke.txt
```

Add this text:

```text
Luke Skywalker was a Tatooine farmboy who rose from humble beginnings to become one of the greatest Jedi the galaxy has ever known.
```

Save and exit:

- Press `Ctrl + S` to save.
- Press `Ctrl + X` to exit.

15. Check that the file exists:

```bash
ls jedi
```

> [!TIP]
>
> You created `luke.txt`. What command would show the contents of the `jedi` folder in long format?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> ls -l jedi
> ```
>
> </details>

16. Create a second file:

```bash
pico jedi/obi-wan.txt
```

Add this text:

```text
Obi-Wan Kenobi was a legendary Jedi Master.
```

Save and exit.

17. List the files with detailed information:

```bash
ls -l jedi
```

> [!TIP]
>
> In the `ls -l` output, why do you see `yoda yoda` next to the files?
>
> <details>
> <summary>Show answer</summary>
>
> The first `yoda` is the file owner. The second `yoda` is the group. The files belong to the `yoda` user and the `yoda` group.
>
> </details>

18. Copy `luke.txt`:

```bash
cp jedi/luke.txt jedi/luke_copy.txt
```

19. Copy `obi-wan.txt`:

```bash
cp jedi/obi-wan.txt jedi/obi-wan-copy.txt
```

20. Delete the copied Obi-Wan file:

```bash
rm jedi/obi-wan-copy.txt
```

21. Copy the full `jedi` folder:

```bash
cp -r jedi jedi_backup
```

22. Confirm both folders exist:

```bash
ls
```

23. Delete the backup folder:

```bash
rm -r jedi_backup
```

> [!WARNING]
>
> Be careful with `rm`. It deletes files and folders. Always check the path before running a delete command.

24. Edit the copied Luke file:

```bash
pico jedi/luke_copy.txt
```

Add this line at the top:

```text
THIS IS A COPY
```

Save and exit.

25. Print the file contents in the terminal:

```bash
cat jedi/luke_copy.txt
```

> [!TIP]
>
> Small research task: search the web for how to list only files ending in `.txt` in Linux. What command could you run for the `jedi` folder?
>
> <details>
> <summary>Show answer</summary>
>
> One simple answer is:
>
> ```bash
> ls jedi/*.txt
> ```
>
> The `*` symbol is a wildcard. It means "match any text before `.txt`".
>
> </details>

#### Part C: Install Apache

26. Install Apache:

```bash
sudo apt install apache2
```

Type `Y` if Ubuntu asks you to confirm the installation.

27. Check that Apache is running:

```bash
systemctl status apache2
```

Press `Ctrl + C` or `q` to return to the terminal if the status view stays open.

> [!TIP]
>
> What is Apache doing in this lab?
>
> <details>
> <summary>Show answer</summary>
>
> Apache is the web server. It receives HTTP requests from a browser and returns web pages, such as the default Apache page or your `yoda-site.html` page.
>
> </details>

28. Open your browser and visit the external IP address of your VM.

You should see the Apache default page.

#### Part D: Publish a Simple HTML Page

29. Create a new file:

```bash
pico yoda-site.html
```

30. Add this HTML:

```html
<!doctype html>
<html>
  <head>
    <title>Yoda Site</title>
  </head>
  <body>
    <h1>Hello from my cloud VM</h1>
    <p>I created this page from the Linux terminal.</p>
  </body>
</html>
```

Save and exit.

31. Copy the file to the Apache web folder:

```bash
sudo cp yoda-site.html /var/www/html/
```

> [!TIP]
>
> Why do we need `sudo` when copying a file into `/var/www/html/`?
>
> <details>
> <summary>Show answer</summary>
>
> `/var/www/html/` is a system folder used by Apache. Normal users usually cannot write there, so `sudo` gives the command administrator privileges.
>
> </details>

32. Visit this URL in your browser:

```text
http://YOUR_VM_EXTERNAL_IP/yoda-site.html
```

Replace `YOUR_VM_EXTERNAL_IP` with your VM external IP address.

#### Command Summary

- `pwd` shows the current directory.
- `ls` lists files and folders.
- `cd` changes directory.
- `cd ..` moves up one directory.
- `mkdir` creates a folder.
- `pico` opens a terminal text editor.
- `cp` copies a file.
- `cp -r` copies a folder.
- `rm` deletes a file.
- `rm -r` deletes a folder.
- `cat` prints a file in the terminal.
- `sudo apt install` installs software.
- `systemctl status` checks a service.

Part 1 is complete. Continue to [Part 2](c1-02-part-2.md).
