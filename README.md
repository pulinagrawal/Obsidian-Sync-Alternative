# Obsidian Syncing Alternative

## How to keep your Obsidian notes synced between your desktop app and phone app?

It feels like I have found the holy grail of note taking. [Obsidian](https://obsidian.md) is an amazing app available for your desktop for note-taking. The only problem is that the syncing functionality between phone and the desktop app is a premium feature. So far, I was relying on [GitJournal](https://gitjournal.io/) for syncing my notes. But it breaks the consistency. Don't get me wrong, GitJournal is one clean android app, but it does break flow. It is also not as simple to setup and also requires providing your git key to the app. Also, I did not have access to all the cool plugins that [Obsidian App](https://obsidian.md/mobile) has created by its wonderful community. I am not sure if I found this solution or part of this solution somewhere or I thought of it on my own. I am trying to document it here for the benefit of anyone using Obsidian. It simply involves using a free app Termux. This is assuming you are using Android. I have not tested this solution on iOS.

## Setting up desktop and phone Obsidian sync

I assume you are already using Obsidian if you have reached this page. But if not, you can download and start using Obisdian on your desktop after downloading it from the website [here](https://obsidian.md). Also, I am assuming that you have setup syncing your notes to a git repo. Check this out [How to sync Obsidian vault for free using Git?](https://desktopofsamuel.com/how-to-sync-obsidian-vault-for-free-using-git/) if you haven't already done that. Also, I am assuming that you have the Android app installed, [linked here](https://play.google.com/store/apps/details?id=md.obsidian). As it was mentioned, the solution is based on Termux. So, the next step is to install the app [Termux](https://f-droid.org/packages/com.termux/) on your Android device. I would STRONGLY recommend that you install Termux from F-Droid because it is no longer being maintained on Play Store. The mirrors/repositories are not setup correctly and outdated, so you wouldn't be able to perform all the steps mentioned below as it is.

> Install Termux on your android device.

> Run the following commands in Termux to setup git. (It is a little tricky to set this up. It hasn't been a very deterministic process for me. Hopefully you have better luck than me. The uncertainity might be a result of the fact that I was using Play Store Termux before, first when I wrote these instructions. With F-Droid Termux it worked in first try.  

``` 
termux-setup-storage
```
Allow permission to storage. Type 'ls' to make sure that you can see the 'storage' folder. 
```
ls
```
>Update linux distribution and install git and openssh. Remember to press 'Enter' on to any prompts that show up to keep the default versions of the packages.
You need git to clone and pull and push changes to your vault. You need openssh to clone your repo because GitHub removed https cloning. Upon installation of openssh, it will provide the path to your public key. Please make a note of that.
```
apt update && apt upgrade -y
pkg install git -y
pkg install openssh -y
```
>Print the public key using the following command.
```
cat /path/to/public/key/ends/like/ssh_host_ed25519.pub
```
>Copy the printed text and follow the instructions [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account#adding-a-new-ssh-key-to-your-account) to add it to your GitHub.

>Next, setup a folder on your android device where you want to store your synced notes from your desktop. 'cd' into this folder.
 
```
mkdir ~/storage/shared/path/to/folder/
cd ~/storage/shared/path/to/folder/
```

> Now you need to clone your repo. Follow the instructions in [this article](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) to setup your personal authentication token for GitHub if you haven't already done so and clone your repo.

```
git clone git@github.com:username/repo.git
```

I had a little issue setting this up because the ssh-agent was not started and I was getting "Error: Permission denied (publickey)". I followed the following links 
-[GitHub Permission denied](https://docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey)
-[Oracle start ssh-agent](https://docs.oracle.com/cd/E19118-01/n1.sprovsys51/819-1655/egcor/index.html)
The steps I followed to fix the issue. Mind the removal of '.pub' extension in the second command. We need to use the private key path associated with the public key we added to GitHub. 
```
eval `ssh-agent`
ssh-add /path/to/public/key/ends/like/ssh_host_ed25519
```

> Make sure you are able to push and pull your repo. 
```
cd ~/storage/shared/path/to/folder/repo
git pull
```
I had to deal with a fatal error the solution to which was provided after the error. The command I entered was
```
git config --global --add safe.directory ~/storage/shared/path/to/folder/repo
```
I encountered another issue where I was not able to pull when I restarted Termux. To solve that I added the following lines to my ~/.bashrc.
```
# ~/.bashrc
eval `ssh-agent`
ssh-add /path/to/public/key/ends/like/ssh_host_ed25519
```

> Create a sync.sh file with the following lines.


```
#sync.sh
TPWD=$PWD

cd ~/path/to/your/cloned/vault

git add -A
git commit -m "android backup at $(date)"
git pull
git push

cd $TPWD
```

> Make this file executable by running

``` chmod 755 /path/to/sync.sh ```

> Next, you need to setup a cron job in Termux. For more details on setting up a cron job you can refer to the following article. https://phoenixnap.com/kb/set-up-cron-job-linux.

```
pkg install cronie termux-services
```

> Restart Termux then run the following.

```
sv-enable crond
crontab -e 
```

> Add a line in the cronjob that says
 
```
* * * * * bash /path/to/sync.sh
```

### Then all you need to do is to keep Termux running! (If your phone shuts off, because you never cared to plug it in in time, you will have to just open termux app and let it run in the background)

> Open the Obsidian app on your android device and select 'Open folder as vault' and open the folder that you just cloned.
 
## Footnote

Please let me know if you liked this solution or you have any questions by leaving a comment in the Issues section of this GitHub repo. 
