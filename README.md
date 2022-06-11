# Obsidian Syncing Alternative

## How to keep your Obsidian notes synced between your desktop app and phone app?

It feels like I have found the holy grail of note taking. [Obsidian](https://obsidian.md) is an amazing app available for your desktop for note-taking. The only problem is that the syncing functionality between phone and the desktop app is a premium feature. So far, I was relying on [GitJournal](https://gitjournal.io/) for syncing my notes. But it breaks the consistency. Don't get me wrong, GitJournal is one clean android app, but it does break flow. It is also not as simple to setup and also requires providing your git key to the app. Also, I did not have access to all the cool plugins that [Obsidian App](https://obsidian.md/mobile) has created by its wonderful community. I am not sure if I found this solution or part of this solution somewhere or I thought of it on my own. I am trying to document it here for the benefit of anyone using Obsidian. It simply involves using a free app Termux. This is assuming you are using Android. I have not tested this solution on iOS.

## Setting up desktop and phone Obsidian sync

I assume you are already using Obsidian if you have reached this page. But if not, you can download and start using Obisdian on your desktop after downloading it from the website [here](https://obsidian.md). Also, I am assuming that you have setup syncing your notes to a git repo. Check this out [How to sync Obsidian vault for free using Git?](https://desktopofsamuel.com/how-to-sync-obsidian-vault-for-free-using-git/) if you haven't already done that. Also, I am assuming that you have the Android app installed, [linked here](https://play.google.com/store/apps/details?id=md.obsidian). As it was mentioned, the solution is based on Termux. So, the next step is to install the app [Termux](https://play.google.com/store/apps/details?id=com.termux) on your Android device.

> Install Termux on your android device.

Next, setup a folder on your android device where you want to store your synced notes from your desktop. 

> Run the following commands in Termux to setup git. (It is a little tricky to set this up. It hasn't been a very deterministic process for me. Hopefully you have better luck than me.) 

``` 
termux-setup-storage
```
Allow permission to storage.
```
apt update && apt upgrade -y
pkg install git -y
pkg install openssh -y
```

> Now you need to clone your repo. Follow the instructions in [this article](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) to setup your personal authentication token for GitHub if you haven't already done so and clone your repo.

```
git clone https://github.com/username/repo.git
Username: your_username
Password: your_token
```

> Setup access to your git repo using your method of liking (personal token, ssh key, etc.). Make sure you are able to push and pull your repo.

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

``` crontab -e ```

> Add a line in the cronjob that says
 
```
* * * * * bash /path/to/sync.sh
```

### Then all you need to do is to keep Termux running! (If your phone shuts off, because you never cared to plug it in in time, you will have to just open termux app and let it run in the background)

> Open the Obsidian app on your android device and select 'Open folder as vault' and open the folder that you just cloned.
 
## Footnote

Please let me know if you liked this solution or you have any questions by leaving a comment in the Issues section of this GitHub repo. 
