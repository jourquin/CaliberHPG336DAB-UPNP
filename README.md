# Stream Spotify (or any other sound) to Caliber HPG336DAB/DIR

I use a small Caliber [HPG336DAB/DIR](https://www.calibereurope.com/en/product/HPG336DAB-DIR-en_GB/) radio to listen to
Internet radio stations as my house is located in a white spot with no access to the FM or DAB bands.

This small device is also an UPNP client. I was thus wondering if there is a possibility to listen to my Spotify playlists on it. 

After some Google searches, I came to the conclusion that the best (only?) solution is to stream the output of a computer 
(its sound card) to the device. Using this approach, it is possible to stream virtually all computer sound sources to my Caliber 
device.

I usually use MacOS computers, but I came across [StreamWhatYouHear](https://www.streamwhatyouhear.com), which runs on Windows 
and seems to do exactly what I'm looking for. Unfortunately, if my Caliber device finds the StreamWhatYouHear server on my local 
network, it only finds an "empty list" and is unable to play any sound. Therefore, I decided to give a chance to 
[Universal Media Server (UMS)](https://www.universalmediaserver.com), a free DLNA, UPnP and HTTP/S Media Server, supporting all major
operating systems, with versions for Windows, Linux and macOS. 


I didn't test it (yet) with Linux, but here are working solutions for Windows and MacOS.

## Windows

In order to capture the output of the sound card, I use the previous described StreamWhatYouHear software. In the "settings" window,
be sure to:
- send stream in MP3 format
- use a specific HTTP port, otherwise the port changes each time the StreamWhatYouHear is launched and the settings in UMS must be 
changed accordingly. I decided to use port 8090.

UMS doesn't support the Caliber HPG336DAB/DIR out of the box. It is, however, rather simple to create a specific renderer for it. A
working renderer is provided with this project, along with a .png image of the device. Once UMS installed on your Windows box,
simply copy these two files in the `C:\Program Files (x86)\Universal Media Server\renderers` directory (or another location if you 
installed UMS elsewhere).

Turn the Caliber device on, and it should appear in the "status" tab of UMS.

It is now time to "connect" UMS to StreamWhatYouHear. Therfore add a "Web content" entry in the "Shared Content" tab of UMS, using 
the following settings:
- Name: Soundcard (or any other name you can imagine)
- Type: Audio stream
- Folders: Soundcard (or any other name)
- Source URL: http://localhost:8090/stream/swyh.mp3

Obviously, this implies that UMS and StreamWhatYouHear are installed on the same computer. 

 



  




 