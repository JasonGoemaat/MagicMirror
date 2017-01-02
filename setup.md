# Pi

## Install MagicMirror

Use the installation script, which basically installs the latest node version
and clones the git repository into `~/MagicMirror`.  Now go to
`~/MagicMirror/config` and copy `config.sample.js` to `config.js` and edit
it to change any settings (see Weather section below for setting up current
weather and forecase api keys and locations).

## Script

Create a script to run magic mirror.  I also put xset commands to try and
disable power saving and the screen saver built into X:

    xset s 0 0
    xset s noblank
    xset s off
    xset dpms 0 0 0
    xset dpms force on
    xset -dpms
    cd /home/pi/MagicMirror
    npm start &

And you'll need to do `chmod 755 magic.sh` so it can be executed.

## Desktop startup

* Edit the desktop startup and comment-out the screensaver and possibly file manager, and add a command to run the script we just created:
* `nano ~/.config/lxsession/LXDE-pi/autostart`

    @lxpanel --profile LXDE-pi
    #@pcmanfm --desktop --profile LXDE-pi
    #@xscreensaver -no-splash
    @/home/pi/magic.sh

Instead of that last line to run our script, you could also create a file
`~/.config/autostart/magic.desktop` that will run it when LXDE starts:

    [Desktop Entry]
    Type=Application
    Exec=/home/pi/magic.sh


### Weather

* Go to https://home.openweathermap.org/users/sign_up and create an account.
* Click on 'API' menu and pick the first option for current weather data, then select 'Free' which gives 60 calls per minute and 'Get API key and Start'
* Then on my account page (weird, doesn't remember my login on home page, clicking on 'sign in' shows me already signed in though), click 'API keys' and copy the api key.
* Need to get city location id, go to http://openweathermap.org/current and you can download the list from http://bulk.openweathermap.org/sample/ as city.list.us.json.gz
* Urbandale city code: 4879890
* Sample call: http://api.openweathermap.org/data/2.5/weather?id=4879890&APPID={APIKEY}

## Utilities

### Restart desktop

For testing and to enable quick restarting of the desktop, I created
`~/restart_desktop.sh` (and did `chown 755 ~/restart_desktop.sh`):

    /etc/init.d/lightdm restart

Now to restart the desktop I can do `sudo ~/restart_desktop.sh`


