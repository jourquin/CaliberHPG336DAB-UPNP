# Stream Spotify (or any other sound) to a Caliber HPG336DAB/DIR radio

![Caliber HPG336DAB/DIR](/renderer/Caliber-HPG336DAB.png)

I use a [Caliber HPG336DAB/DIR](https://www.calibereurope.com/en/product/HPG336DAB-DIR-en_GB/) radio to listen to
Internet radio stations. This small device is also 
an [UPNP](https://en.wikipedia.org/wiki/Universal_Plug_and_Play) client. I was thus wondering if there is a possibility 
to listen to my Spotify playlists on it. 

After some Google searches, I came to the conclusion that the best (only?) solution is to stream the output of a computer 
sound card to the device. Using this approach, it is possible to stream virtually all computer sound sources to my Caliber 
radio.

I usually use MacOS computers, but I came across [StreamWhatYouHear (SWYH)](https://www.streamwhatyouhear.com), which runs on Windows 
and seems to do exactly what I'm looking for. Unfortunately, if the Caliber device identifies the SWYH server on my local 
network, it only finds an "empty list" and is unable to play any sound. Therefore, I decided to give a chance to 
[Universal Media Server (UMS)](https://www.universalmediaserver.com), a free DLNA, UPnP and HTTP/S Media Server, supporting all major
operating systems, with versions for Windows, Linux and macOS. 

I didn't test it (yet) with Linux, but here are working solutions for Windows and MacOS. Both are based on free, open-source software 
components.

## Windows

In order to capture the output of the sound card, I use the previous described SWYH software. In its "settings" window,
be sure to:
- Send stream in MP3 format.
- Use a specific HTTP port, otherwise the port changes each time SWYH is launched and the settings in UMS must be 
changed accordingly. I use port 8090.

UMS doesn't support the Caliber HPG336DAB/DIR out of the box. It is, however, rather simple to create a specific renderer for it. A
working renderer is provided in this project, along with a .png image of the device. Once UMS installed on your Windows box,
simply copy the two files located in the "renderer" directory of this project in 
the `C:\Program Files (x86)\Universal Media Server\renderers` directory (or another location if you installed UMS elsewhere).

Turn the Caliber device on, and it should appear in the "status" tab of UMS.

It is now time to "connect" UMS to SWYH. Therefore add a "Web content" entry in the "Shared Content" tab of UMS, using 
the following settings:
- Name: Soundcard (or any other name you can imagine).
- Type: Audio stream.
- Folders: Soundcard (or any other name).
- Source URL: http://localhost:8090/stream/swyh.mp3.

Obviously, this implies UMS and SWYH being installed on the same computer. 

Play some music (for instance, a Spotify playlist) on your computer. In the "Media" tab of the Caliber device, you should be able to
find the UMS server in the UPNP menu (along with the non-useable StreamWhatYouHear server). Choose the "Soundcard" folder and you 
should hear what is playing on your computer. Note that:
- On the Caliber side, it is possible that several attempts will be needed before the UMS Soundcard folder connects to the server.
- There is a relative long latency between the sound played on the computer and the one rendered by the Caliber device. My experience 
is a latency of about 30 seconds.
- You can mute the sound on the computer.

If you listen to Spotify music, you can use the Spotify smartphone app to change the music you listen to (with the latency described
earlier).

## MacOS

(Including M1 computers)

The general strategy I've adopted for Mac computers is similar to the one used for Windows. However, I didn't find something similar
to SWYH to capture the sound card. Therefore, I've implemented an alternative solution based 
on [BlackHole](https://existential.audio/blackhole/), an open source virtual audio driver that allows applications to pass audio 
to other applications and [VideoLAN (VLC)](https://www.videolan.org), a free and open source cross-platform multimedia player and 
framework that plays most multimedia files as well as DVDs, Audio CDs, VCDs, and various streaming protocols.

Once BlackHole and VLC installed, the sound card can be captured and broadcast to an HTTP stream, similarly to what SWYH
does. Therefore, go to the File | Open Capture Device menu of VLC. Choose to capture the "BlackHole 2ch" Audio device, tick the 
"Stream output" checkbox and open the "Settings" window. In this window:
- Choose to "Stream" with "HTTP" type.
- Mention the "localhost" address, and the 8090 port.
- Choose the "Raw" encapsulation method.
- Select "Audio" and "mp3" in the transcoding options.
- You can also specify a 128k bitrate and 2 channels (even if I'm not sure this is really needed).

Once "OK" pressed, an "audiocapture://BlackHole2ch_UID" entry appears in the VLC playlist. 

Similarly to what was done for the Windows solution, UMS must be installed. Copy the two files located in the "renderer" directory
of this project in `/Users/YOUR_HOME_DIR/Library/Application Support/UMS/renderers`.

Turn the Caliber device on, and it should appear in the "status" tab of UMS.

It is now time to "connect" UMS to VLC. Therefore add a "Web content" entry in the "Shared Content" tab of UMS, using 
the following settings:
- Name: Soundcard (or any other name you can imagine).
- Type: Audio stream.
- Folders: Soundcard (or any other name).
- Source URL: http://localhost:8090.

Obviously, this implies UMS and VLC being installed on the same computer. 

Play some music (for instance, a Spotify playlist) on your computer. In the "Media" tab of the Caliber device, you should be able to
find the UMS server in the UPNP menu. Choose the "Soundcard" folder and you should hear what is playing on your computer. Note that:
- On the Caliber side, it is possible that several attempts will be needed before the UMS Soundcard folder connects to the server.
- There is a relative long latency between the sound played on the computer and the one rendered by the Caliber device. My experience 
is a latency of about 30 seconds.
- You can mute the sound on the computer.

If you listen to Spotify music, you can use the Spotify smartphone app to change the music you listen to (with the latency described
earlier).

## iPhone

I didn't (yet) find a direct solution to stream iPhone music to the Caliber device. A simple workaround is to send music from the 
iPhone to a Mac computer via [Airplay](https://www.apple.com/airplay/). If the Mac is running the VLC / UMS combination explained in the 
[previous section](#macos), the sound of your iPhone will be streamed to the Caliber device.
  
