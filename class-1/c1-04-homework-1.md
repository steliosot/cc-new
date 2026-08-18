### Week 1 Homework: Linux Practice and Evidence

Complete this homework after finishing Week 1 Part 1 and Part 2.

#### Task 1: Create a New VM

1. Create a new Ubuntu VM in GCP.
2. Name it:

```text
week-1-homework
```

3. Enable HTTP and HTTPS traffic.
4. Connect to the VM using SSH.

#### Task 2: Practise Linux Files and Folders

Run the commands needed to do the following:

1. Create a new user with your first name.
2. Add that user to the `sudo` group.
3. Switch to that user.
4. Create a folder called `cloud_homework`.
5. Inside that folder, create two text files:
   - `about-cloud.txt`
   - `commands.txt`
6. In `about-cloud.txt`, write two sentences explaining what a virtual machine is.
7. In `commands.txt`, write five Linux commands you learned this week and what each command does.
8. Use `cat` to print both files in the terminal.

#### Task 3: Publish a Simple Web Page

1. Install Apache on the VM.
2. Create a file called `homework.html`.
3. The page must include:
   - your name
   - the module name
   - one paragraph explaining what you learned in Week 1
4. Copy `homework.html` to:

```text
/var/www/html/
```

5. Open the page in your browser using:

```text
http://YOUR_VM_EXTERNAL_IP/homework.html
```

#### Task 4: User Management

1. Create a temporary user called `testuser`.
2. Confirm that `/home/testuser` exists.
3. Delete `testuser`.
4. Delete `/home/testuser`.
5. Confirm that `testuser` is gone.

#### What To Submit

Submit a short document or Moodle text entry with:

- your VM external IP address
- the URL of your `homework.html` page
- the output of `whoami`
- the output of `pwd`
- the output of `ls -l cloud_homework`
- the output of `cat cloud_homework/about-cloud.txt`
- the output of `cat cloud_homework/commands.txt`
- a short note confirming that you deleted the VM when you finished

> [!IMPORTANT]
>
> Delete the VM when you finish the homework. Do not leave cloud resources running unnecessarily.

