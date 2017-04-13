# pi-irrc
#### Use your TV remote to control your VLC media player.

The idea for the project came when I bought a new PC, connected it to my tv and my stereo system. Then when I had to pause/play something from VLC, I had to go to the computer or bring my wireless mouse/keyboard to the TV or whatever. Or if I wanted to change the volume, I had to use the remote for the stereo system. I guess you've been there and the bottom line is that it's inconvenient.
To clarify from the beginning there is a way to [control VLC from your phone](https://wiki.videolan.org/Control_VLC_from_an_Android_Phone/), over SSH, telnet or a web interface provided by VLC. Apart from the phone option, the others are still quite inconvenient.

So I decided to intercept IR signals from my remote control, pass and process them through my PI and then use the [VLC's HTTP "API"](https://wiki.videolan.org/VLC_HTTP_requests/).

### What you need

This is more of an article with a little bit of code to help you achieve this goal. So naturally you'll need a few things before you get started:

```
1. Raspberry PI - I'm using PI 2 B with Raspbian installed
2. IR sensor - *TODO: add sensor model*
3. A remote control - I'm using my TV's remote (Samsung *TODO remote model*)
4. Connecting wires - I'm using jumper cables
5. All your devices need to be connected to the same network(e.g. your home network)
```

### Configure VLC
First thing you can do is to configure VLC to run the web interface every time it's open so you can use the [HTTP Requests](https://wiki.videolan.org/VLC_HTTP_requests/). That's pretty easy to do. Just make sure port 8080 is open. That should be the default port for the web interface.

My current VLC version is 2.2.4

```
1. Open Preferences
2. Interfaces tab -> Show all
3. Then from the left menu choose Interfaces -> Main interfaces
4. Then you check the web checbox
```

Now the web interface for VLC is enabled. You can test it at `http://localhost:8080`. It's going to ask you to add a password for security reasons and you won't be able to use the web interface without adding a password. I know - it's a bit stupid and there was kind of a [debate](https://trac.videolan.org/vlc/ticket/9671) going on about it. However you can still use the [HTTP Requests](https://wiki.videolan.org/VLC_HTTP_requests/), which control the player, which is what we need for our script.

### Set local IP to be static
So in order for our PI to be able to access the machine which runs VLC, you'll need to use it's IP address (localhost won't work). But sometimes that IP changes. Checkout how you can make your local IP static on your OS, because we'll need it for our script.
