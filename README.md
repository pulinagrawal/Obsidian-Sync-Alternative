# Obsidian-Sync

## How to keep your Obsidian notes synced between your desktop app and phone app?

It feels like I have found the holy grail of note taking. [Obsidian](https://obsidian.md) is an amazing app available for your desktop for note-taking. The only problem is that the syncing functionality between phone and the desktop app is a premium feature. So far, I was relying on [GitJournal](https://gitjournal.io/). But it breaks the consistency. GitJournal is one clean android app but it does break cohesion. It is not as simple to setup and also requires providing your git key to an app. Also, I did not have access to all the cool plugins that [Obsidian App](https://obsidian.md/mobile) has created by its wonderful community. I am not sure if I found this solution or part of this solution somewhere or I thought of it on my own. But it simply involves using a free app Termux. This is assuming you are using Android. I have not tested this solution on iOS.

## Setting up desktop and phone Obsidian sync

I assume you already using Obsidian if you reached this page. But if not, you can download and start using Obisdian on your desktop after downloading it from the website. Also, I am assuming that you have the Android app installed. As it was mentioned, the solution is based on Termux. So, the next step is to install the app [Termux]() on your Android device.

> Install Termux on your Android device.

Next, setup a folder on your android device where you want to store your synced notes from your desktop.

> Setup notes syncing folder in your Android device.

Next, you need to setup a cron job in Termux. For more details on setting up a cron job you can refer to the following article. https://phoenixnap.com/kb/set-up-cron-job-linux. 
 
> Run the following command in Termux.

``` crontab -e ```

Now basically, add a line that will run the right sequence of git commands to pull content from a remote git repo and then push the current contents back to the remote. 

All you need then is to keep Termux. Running

## Footnote

Sorry for a badly written solution. More details coming soon...

Please let me know if you liked this solution by leaving a comment in the Issues of this GitHub repo. 
