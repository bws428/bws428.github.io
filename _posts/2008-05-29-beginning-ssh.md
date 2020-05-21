---
title: Beginning SSH on Ubuntu
date: 2008-05-29
image: assets/images/trinity-ssh.jpg
categories: linux
tags: ubuntu
---

Let’s say you have a private LAN running in your secret underground lab. Maybe you’ve got a Linux box hardwired to a WiFi router, and maybe a Mac or Linux laptop floating around somewhere, and you need a quick way to transfer files or execute shell commands remotely.

What you need is [SSH](http://en.wikipedia.org/wiki/Secure_Shell), the Secure Shell. SSH is a powerful tool which allows secure remote login over insecure networks. It provides an encrypted terminal session with strong authentication of both the server and client using public-key cryptography. This tutorial will cover the basics of SSH’s most useful features:

- Logging into a remote computer over a secure connection.
- Transferring files and directories between computers over a secure connection.
- Enabling public-key authentication.
- Passwordless authentication for use with scripts and cron jobs.

The following assumptions are made about the reader:

- You know what a terminal/command line/shell is and how to start a session.
- You have at least a basic familiarity with Linux/Mac command-line syntax.
- You’re on a private LAN with access to at least two Linux/Mac computers (or, you have a user account on a remote server that accepts SSH connections).

As always, comments, corrections, and suggestions for improvement are appreciated.

## Installing OpenSSH

The Ubuntu (and MacOS X) flavor of SSH is called [OpenSSH](http://www.openssh.org/), a free, open-source implementation of the `ssh` protocol. It consists of two basic components, an `openssh-client` and an `openssh-server`. SSH clients communicate with SSH servers over encrypted network connections.

The `openssh-client` software should already be installed by default on Ubuntu. If you want to be able to accept SSH connections as well as request them, you’ll need the server software as well. To install it, simply run:

```bash
sudo apt update
sudo apt install openssh-server
```

Once `openssh-server` is installed, the SSH service will start automatically. You can verify this by typing:

```bash
sudo systemctl status ssh
```

You should see `Active: active (running)` in the output.

Ubuntu uses firewall configuration tool called UFW. If the firewall is enabled on your system, you'll need to open the SSH port:

```bash
sudo ufw allow ssh
```

Now your SSH server should be up and running and ready to accept connections.

## Using SSH to Login to a Remote Computer

Once OpenSSH is installed, you can login to a remote SSH server by using the `ssh` command:

```bash
ssh remoteuser@remote.host
```

where `remoteuser` is the username of the remote account you’re trying to access, and `remote.host` is the remote server’s hostname or IP address.

For example, if you know that your Ubuntu desktop machine (now running `openssh-server`) has a user account named `joebanks` and that the IP address of that computer on your private LAN is `192.168.0.12`, you could login remotely to that account from your Linux or Mac laptop by typing:

```bash
ssh joebanks@192.168.0.12
```

If you’re unsure of a computer’s local IP address, try running `ifconfig` on that machine. This will display the status of the active network interfaces, and the local IP address of that device will be listed after `inet addr`. It will most likely be in the form of `192.168.0.xx`.

Or, if you’ve got a web hosting account that allows shell access with a domain name like `cooldomain.com`, your syntax might look like:

```bash
ssh joebanks@cooldomain.com
```

Now, the first time an SSH client encounters a new remote server, it will report that it’s never seen the machine before, by printing the following message:

```bash
The authenticity of host 'remote.host (192.168.0.12)' can't be established.
RSA key fingerprint is 53:b4:ad:c8:51:17:99:4b:c9:08:ac:c1:b6:05:71:9b.
Are you sure you want to continue connecting (yes/no)?
```

This is just an extra security measure to ensure that you’re actually connecting to the machine you think you are. If you type `yes` (the most common response), you’ll see the following:

```bash
Warning: Permanently added 'remote.host' (RSA) to the list of known hosts.
```

Subsequent login attempts to this machine will omit the warning message. You’ll then be asked for the `remoteuser`‘s password:

```bash
remoteuser@remote.host's password:
```

And after correctly entering it, like magic, you’ll be logged into the remote machine, and instead of your local machine’s command prompt, you’ll see the following:

```bash
remoteuser@remote.host:~$
```

And, voila! You can execute commands on the remote server just as you would on your local machine. To close the `ssh` connection, type `exit`, or use `Ctrl-D`.

## Copying Files

To transfer files and directories from your local machine to the remote server and vice-versa, you’ll use SSH’s “secure copy” command, or `scp`. To copy a single file from your local machine to the server, use the following syntax:

```bash
scp file.txt remoteuser@remote.host:/directory
```

where `file.txt` is the name of a file in the current directory of your local machine, `remoteuser@remote.host` is the username and hostname or IP address of the server (just like in the above `ssh` examples), and `/directory` is the directory path on the server where you want your file copied.

For example, if you want to copy the local `file.txt` to the `/home/joebanks/docs` directory on the server you logged into above, you’ll run the following command from a _local terminal session:_

```bash
scp file.txt joebanks@192.168.0.12:/home/joebanks/docs
```

By “local terminal session,” I mean that you **do not** first `ssh` in to the remote computer and _then_ run the `scp` command. Secure copy opens the `ssh` tunnel on its own. Since secure copy uses the `ssh` protocol for a secure connection, whenever you run the `scp` command, you’ll be prompted for a password (or passphrase) if the `ssh` connection requires it.

You can be as verbose as you want with the local and remote filenames and directories, even changing the filename in the process, like so:

```bash
scp ~/docs/oldfile.txt joebanks@192.168.0.12:/home/joebanks/docs/newfile.txt
```

To copy a file from the server to your local machine, use the following syntax:

```bash
scp remoteuser@remote.host:file.txt /local/directory
```

where `remoteuser@remote.host` is the username and hostname or IP address of the server, `file.txt` is a file in the `/home/remoteuser` directory, and `/local/directory` is the local directory path into which the file will be copied.

Again, you can be as verbose as necessary, for example:

```bash
scp joebanks@192.168.0.12:newfile.txt /home/joe/downloads
```

where `newfile.txt` is a file in the `/home/remoteuser` directory.

## Copying Directories

To copy an entire directory (and all of its contents) from the local machine to the remote server, use the recursive `-r` switch, like so:

```bash
scp -r /local/directory remoteuser@remote.host:/remote/directory
```

where `/local/directory` is the path to the local directory you want copied, and `/remote/directory` is the remote directory _into which_ you want the directory to be copied.

To copy an entire directory (and all of its contents) from the remote server to the local machine, use the following:

```bash
scp -r remoteuser@remote.host:/remote/directory /local/directory
```

where `/remote/directory` is the path to the remote directory you want copied, and `/local/directory` is the local directory _into which_ you want the directory to be copied.

## Get Lazy

By now you’re no doubt getting sick of typing `joebanks@192.168.0.12` or whatever. It sure would be nice to type something shorter. Let’s fix that.

OpenSSH uses a client configuration file, located at `~/.ssh/config`, to simplify some of the tedious typing associated with logging into remote machines. To edit (or create) that file, we’ll use the `nano` text editor:

```bash
nano ~/.ssh/config
```

The simplest configuration file might look something like this:

```bash
Host volcano
    HostName 192.168.0.12
```

This associates the Host `volcano` with the HostName (or IP address) `192.168.0.12`, so now to login you’ll only need to type:

```bash
ssh joebanks@volcano
```

That’s cool, but let’s get even more lazy, shall we? How about this:

```bash
Host bigwoo
    HostName 192.168.0.12
    User joebanks
```

Now all we need to type is:

```bash
ssh bigwoo
```

Far out! You can add as much configuration data as you need to this Host, and as many different Hosts as you like. And this shorthand will also work with all the `scp` examples:

```bash
scp file.txt bigwoo:
```

To learn more visit the [OpenSSH docs](http://www.openbsd.org/cgi-bin/man.cgi?query=ssh_config), or run `man ssh_config`.

## Public-Key Authentication

To increase the security of your SSH session, or to set up passwordless authentication, you can enable SSH’s built-in [public-key cryptography](http://en.wikipedia.org/wiki/Public-key_cryptography). Then, instead of entering the `remoteuser`'s password on each login attempt, SSH will initiate a challenge-and-response protocol which attempts to match an encrypted public key (stored on the server) with a protected private key (stored on the local machine). This completely eliminates the need to send sensitive information (like a password) over the network, encrypted or not.

To generate a public/private key pair on your local machine, open a terminal and type:

```bash
ssh-keygen -t dsa
```

You’ll see the following message:

```bash
Generating public/private dsa key pair.
Enter file in which to save the key (~/.ssh/id_dsa):
```

Hit `Enter`. This selects the default location. Next you’ll see:

```bash
Enter passphrase (empty for no passphrase):
```

Enter a secure passphrase, ideally something unique and unguessable. Unlike a password, a passphrase is usually a phrase or complete sentence, with spaces and punctuation if you like. The longer and more obscure the phrase, the stronger it is.

**Note:** It is also acceptable to just hit `Enter` at this point, thereby selecting _no passphrase._ This can be appropriate in some instances (see below), but you should be aware that it is _not_ as secure. While `ssh` will still use an encrypted public/private keypair (which is almost certainly more secure than typing the remote user’s password like you’ve been doing), anyone gaining access to your local account will thereby also gain access to any remote hosts which recognize your public key.

After typing a passphrase and hitting `Enter`, you’ll see:

```bash
Enter same passphrase again:
```

Enter the same passphrase again. And finally:

```bash
Your identification has been saved in ~/.ssh/id_dsa.
Your public key has been saved in ~/.ssh/id_dsa.pub.
The key fingerprint is:
42:ac:8a:81:31:81:e5:7b:d2:01:42:2d:64:32:0f:dd localuser@localhost
```

Now copy your public key, which is stored in the local file `~/.ssh/id_dsa.pub` to the remote machine, by running the following command from a _local terminal session:_

```bash
ssh-copy-id -i ~/.ssh/id_dsa.pub remoteuser@remote.host
```

You’ll be prompted for the `remoteuser`‘s password one last time, and then you’ll see the message:

```bash
Now try logging into the machine, with "ssh 'remoteuser@remote.host'", and check in:

    .ssh/authorized_keys

to make sure we haven't added extra keys that you weren't expecting.
```

**MacOSX Users:** If the `ssh-copy-id` method doesn’t work with your version of OpenSSH, try the following command:

```bash
cat ~/.ssh/id_dsa.pub | ssh remoteuser@remote.host 'cat >> .ssh/authorized_keys'
```

Or, if the `~/.ssh` directory doesn’t yet exist on the remote machine, you can create the directory _and_ copy the public key all in one swell foop, like so:

```bash
cat ~/.ssh/id_dsa.pub | ssh remoteuser@remote.host 'mkdir .ssh; cat >> .ssh/authorized_keys'
```

Once the `~/.ssh/authorized_keys` file on the remote machine contains your public key, you’ll be prompted for your passphrase at each login attempt, rather than the `remoteuser`‘s system password, like so:

```bash
Enter passphrase for key '~/.ssh/id_dsa':
```

Finally, it’s a good idea to change the permissions of the `~/.ssh` directory and the `~/.ssh/id_dsa` file on your local machine.

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_dsa
```

Changing permissions in this way disables any outside access to your private key.

## Passwordless Authentication for Automated Tasks

You can use `ssh` and `scp` in shell scripts and cron jobs to automate repetitive tasks, such as remote backups of critical files. To do so, however, you’ll need to implement some sort of passwordless authentication scheme, so that scripts can run without human intervention.

Two methods are presented below, the first is the easiest to implement (and possibly the most prevalent), while the second is the most secure.

### Method 1: Sans Passphrase

If you want your scripts to have access to the `ssh` command without requiring a password or passphrase, simply go through the steps outlined above for enabling public-key authentication, but instead of entering a passphrase when prompted, simply press `Enter`.

The `ssh-keygen` program will generate a public/private key pair for you that will allow password- and passphrase-less access to any remote server which has your public key stored in its `authorized_keys` file.

**Is this secure?** Well, no. And yes. It’s probably more secure than typing in the `remoteuser`‘s password and sending it over the network. Sure it’s encrypted, but it could still be intercepted.

Public-key authentication sends no password or passphrase data over the network. The “challenge” sent by the server is encrypted with your public key, and can only be decrypted with your private key, which only you have.

However, as mentioned above, if someone gains access to your local computer, they’ll also have access to every remote server with your public key.

So, you’ll need to decide the level of security required. In my opinion, if you’re only communicating with machines on a private LAN, under your control, behind a router’s firewall, then this method is adequate. Others may disagree.

### Method 2: Using ssh-agent and keychain

The “proper” and secure way to achieve passwordless authentication is by using the `ssh-agent` and `keychain` daemons to store your passphrase between terminal sessions, so you don’t need to type it every time.

Setup of this method is beyond the scope of this article, but the following tutorials will walk you through the entire process, as well as the theory behind it:

- [OpenSSH key management, Part 1](http://www.ibm.com/developerworks/library/l-keyc.html)
- [OpenSSH key management, Part 2](http://www.ibm.com/developerworks/library/l-keyc2/)

## Go Forth and SSHify

I hope this tutorial serves as a gentle introduction to the Secure Shell. For further reading and advanced topics, check out the references listed below, and browse the [OpenSSH Documentation](http://www.openssh.org/manual.html).

---

### References

- Ubuntu Documentation, [SSH](https://help.ubuntu.com/community/SSH)
- Linuxize, [How to Enable SSH on Ubuntu](https://linuxize.com/post/how-to-enable-ssh-on-ubuntu-18-04/)
- Linuxize, [Using the SSH Config File](https://linuxize.com/post/using-the-ssh-config-file/)
- Linuxize, [How to Setup Passwordless SSH Login](https://linuxize.com/post/how-to-setup-passwordless-ssh-login/)
- Stack Exchange, [RSA vs. DSA for SSH authentication keys](https://security.stackexchange.com/questions/5096/rsa-vs-dsa-for-ssh-authentication-keys)
- GNU.org, [Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)

### Author's Note

This article was originally published on [Principia Labs](https://web.archive.org/web/20121230021549/http://principialabs.com/beginning-ssh-on-ubuntu/).
