### Week 1 Part 2: Linux User Management

This lab introduces Linux user management. You will create users, give a user administrator privileges, switch between users, change passwords, and remove a test user.

#### What You Will Learn

- What `sudo` means.
- How to create Linux users.
- How to add a user to the `sudo` group.
- How to switch between users.
- How to change a user's password.
- How to delete a user and their home directory.

#### Before You Start

Complete [Part 1](c1-01-part-1.md) first.

You should already be connected to your Week 1 VM using SSH.

> [!IMPORTANT]
>
> User-management commands can change access to your VM. Read each command before running it.

#### Part A: Understanding `sudo`

The `sudo` command means "superuser do". It runs a command with administrator privileges.

You need `sudo` for tasks such as:

- installing software
- creating or deleting users
- changing system files
- changing another user's password

> [!TIP]
>
> Why should you be careful when running a command with `sudo`?
>
> <details>
> <summary>Show answer</summary>
>
> `sudo` runs commands with administrator privileges. A mistake can change system files, delete important data, or change who can access the VM.
>
> </details>

#### Part B: Create a New User

1. Create a new user called `bilbo`:

```bash
sudo adduser bilbo
```

Enter a password when prompted. The password will not appear on screen while you type.

You can press `Enter` to skip optional details such as room number and phone number. Type `Y` to confirm at the end.

2. Confirm that `bilbo` has a home directory:

```bash
ls /home
```

You should see a folder called:

```text
bilbo
```

> [!TIP]
>
> You created `bilbo`. What is the usual path to `bilbo`'s home directory?
>
> <details>
> <summary>Show answer</summary>
>
> ```text
> /home/bilbo
> ```
>
> </details>

#### Part C: Give the User `sudo` Access

3. Add `bilbo` to the `sudo` group:

```bash
sudo usermod -aG sudo bilbo
```

4. Check the groups for `bilbo`:

```bash
groups bilbo
```

The output should include:

```text
sudo
```

> [!TIP]
>
> Small theory check: in `sudo usermod -aG sudo bilbo`, why is the `-a` option important?
>
> <details>
> <summary>Show answer</summary>
>
> `-a` means append. It adds `bilbo` to the `sudo` group without removing the user from other groups. Without `-a`, you can accidentally replace the user's group list.
>
> </details>

5. Switch to `bilbo`:

```bash
su - bilbo
```

The `-` moves you into `bilbo`'s home directory.

6. Confirm the current user:

```bash
whoami
```

You should see:

```text
bilbo
```

> [!TIP]
>
> You are logged in as `bilbo`. What command would return you to the previous user session?
>
> <details>
> <summary>Show answer</summary>
>
> ```bash
> exit
> ```
>
> </details>

7. Confirm the current directory:

```bash
pwd
```

You should see:

```text
/home/bilbo
```

#### Part D: Change a User Password

8. Change `bilbo`'s password:

```bash
sudo passwd bilbo
```

Enter the current password if prompted, then enter the new password twice.

> [!TIP]
>
> Why does Linux hide your password while you type it?
>
> <details>
> <summary>Show answer</summary>
>
> It hides the password so people looking at your screen cannot see or count the characters you typed.
>
> </details>

#### Part E: Create and Delete a Test User

9. Create another user called `frodo`:

```bash
sudo adduser frodo
```

10. Confirm the user home directories:

```bash
ls /home
```

You should see both:

```text
bilbo
frodo
```

11. Delete the `frodo` user:

```bash
sudo userdel frodo
```

12. Check `/home` again:

```bash
ls /home
```

You may still see the `/home/frodo` folder. Deleting a user does not always delete the user's home directory.

> [!TIP]
>
> Theory check: why might Linux keep `/home/frodo` after deleting the `frodo` user?
>
> <details>
> <summary>Show answer</summary>
>
> The home directory may contain files the administrator wants to keep, inspect, or back up before deleting.
>
> </details>

13. Delete the `frodo` home directory:

```bash
sudo rm -r /home/frodo
```

> [!WARNING]
>
> Be careful with `sudo rm -r`. It deletes folders recursively. Check the path before pressing `Enter`.

14. Confirm that `frodo` is gone:

```bash
ls /home
```

> [!TIP]
>
> Small research task: search the web for a Linux command that deletes a user and their home directory in one command. What command did you find?
>
> <details>
> <summary>Show answer</summary>
>
> One common answer is:
>
> ```bash
> sudo userdel -r USERNAME
> ```
>
> The `-r` option removes the user's home directory and mail spool. Always check the username carefully before running it.
>
> </details>

#### Part F: Optional Privilege Removal

You can remove a user from the `sudo` group with:

```bash
sudo gpasswd -d username sudo
```

Do not run this on your only administrator account.

> [!WARNING]
>
> Always keep at least one working `sudo` user. If you remove `sudo` access from every account, you can lock yourself out of administrator tasks on the VM.

#### Command Summary

- `sudo adduser USERNAME` creates a user.
- `sudo usermod -aG sudo USERNAME` adds a user to the `sudo` group.
- `groups USERNAME` shows a user's groups.
- `su - USERNAME` switches to another user.
- `whoami` shows the current user.
- `sudo passwd USERNAME` changes a user's password.
- `sudo userdel USERNAME` deletes a user account.
- `sudo rm -r /home/USERNAME` deletes a user's home directory.
- `sudo gpasswd -d USERNAME sudo` removes a user from the `sudo` group.

Part 2 is complete. Now complete [Quiz 1](c1-03-quiz-1.md), then [Homework 1](c1-04-homework-1.md).
