# 01 - Sleep / Wake

While your device is sleeping, you may not want a connection to the internet or bluetooth. One way to achieve this is to use [sleepwatcher](https://formulae.brew.sh/formula/sleepwatcher), which can be installed via Homebrew using `brew install sleepwatcher`.

Create `~/.sleep`

```sh
# ensure blueutil is installed
if ! command -v blueutil &> /dev/null
then
    echo "blueutil could not be found. Please install."
    exit
fi

# turn off bluetooth
blueutil -p 0

# turn off wifi, assume only one airport instance
networksetup -setairportpower Wi-Fi off
```

and grant it execute permissions using `chmod 700 ~/.sleep`.

This script can be tested by running `/opt/homebrew/sbin/sleepwatcher --verbose --sleep ~/.sleep` in the terminal and sleeping the device. On awaking from sleep, you'll notice that both bluetooth and wifi are switched off, while there is verbose printing in the console.

For running a script on awake, create `~/.wakeup` and again use `chmod 700 ~/.wakeup`. Like before, this script can be tested by running `/opt/homebrew/sbin/sleepwatcher --verbose --wakeup ~/.wakeup`.

Once you are happy with your scripts, *sleepwatcher* can be configured to run at launch time. Firstly add the default configuration

```
ln -sfv /opt/homebrew/Cellar/sleepwatcher/2.2.1/homebrew.mxcl.sleepwatcher.plist ~/Library/LaunchAgents/
```

and then load it

```
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.sleepwatcher.plist
```

From now on sleep/wake up your custom scripts will be ran. If you require system level scripts, create `/etc/rc.sleep` and `/etc/rc.wakeup` and use the `de.bernhard-baehr.sleepwatcher-20compatibility.plist` configuration.

## Resources

[Mac OS X: Automating Tasks on Sleep](https://www.kodiakskorner.com/log/258)
