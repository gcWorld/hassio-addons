## Motion Addon
* Addon to use a usb camera with motion detection, using https://motion-project.github.io/
* This addon was forked from: https://github.com/lovejoy777/hassio-addons
* This fork will expose the MJPEG feed on port 8081, and *no* images are saved to disk using the default settings.


### Home Assistant integration
You can view the live MJPEG stream using:
```
camera:
  - platform: mjpeg
    mjpeg_url: http://hassio.local:8081
    name: motioncam
```

If you hane configured a periodic snapshot you can display the last snapped image with:
```
camera:
  - platform: local_file
    name: motion_lastsnap
    file_path: /share/motion/lastsnap.jpg
```

You can view the camera images on the HA front-end using a `Picture-entity` lovelace card with config:

```
camera_view: live
entity: camera.motioncam
type: picture-entity
```

Be sure to [add](https://www.home-assistant.io/components/stream/) the `stream` integration to your HA config.

**Note** that you may encounter an annoying issue whereby the camera stream doesn't display in the card and you see a grey box with `camera idle`. In this case you can use a [panel_iframe](https://www.home-assistant.io/components/panel_iframe/):

```
panel_iframe:
  motion:
    title: motion
    icon: mdi:camera
    url: http://hassio.local:8081
```

### Settings
##### config
*Optional*

Path to a predefined motion.conf settings file

##### videodevice
*/dev/video0*

##### width
*640*

Image width (pixels). Valid range: Camera dependent

##### height
*480*

Image height (pixels). Valid range: Camera dependent

##### framerate
*2*

Maximum number of frames to be captured per second.  
Valid range: 2-100. Default: 100 (almost no limit).

##### text_right
*%Y-%m-%d %T-%q*


Draws the timestamp using same options as C function strftime(3)  
Text is placed in lower right corner

```
%Y = year, %m = month, %d = date,
%H = hour, %M = minute, %S = second, %T = HH:MM:SS,
%v = event, %q = frame number, %t = camera id number,
%D = changed pixels, %N = noise level, \n = new line,
%i and %J = width and height of motion area,
%K and %L = X and Y coordinates of motion center
%C = value defined by text_event - do not use with text_event!
You can put quotation marks around the text to allow leading spaces
```

##### target_dir
*/share/motion*

Target base directory for pictures and films  
Use absolute path.

##### snapshot_interval
*0*

Make automated snapshot every N seconds (0 = disabled)

##### snapshot_name
*%v-%Y%m%d%H%M%S-snapshot*

File path for snapshots (jpeg or ppm) relative to target_dir  
Default value is equivalent to legacy oldlayout option  
For Motion 3.0 compatible mode choose: %Y/%m/%d/%H/%M/%S-snapshot  
File extension .jpg or .ppm is automatically added so do not include this.  
Note: A symbolic link called lastsnap.jpg created in the target_dir will always point to the latest snapshot, unless snapshot_filename is exactly 'lastsnap'

```
%Y = year, %m = month, %d = date,
%H = hour, %M = minute, %S = second, %T = HH:MM:SS,
%v = event, %q = frame number, %t = camera id number,
%D = changed pixels, %N = noise level, \n = new line,
%i and %J = width and height of motion area,
%K and %L = X and Y coordinates of motion center
%C = value defined by text_event - do not use with text_event!
You can put quotation marks around the text to allow leading spaces
```

##### picture_output
*off*

Output 'normal' pictures when motion is detected  
Valid values: on, off, first, best, center  
When set to 'first', only the first picture of an event is saved.  
Picture with most motion of an event is saved when set to 'best'.  
Picture with motion nearest center of picture is saved when set to 'center'.
`on` appears to continually capture.

##### picture_name
*%v-%Y%m%d%H%M%S-%q*

File path for motion triggered images (jpeg or ppm) relative to target_dir  
Default value is equivalent to legacy oldlayout option  
For Motion 3.0 compatible mode choose: %Y/%m/%d/%H/%M/%S-%q  
File extension .jpg or .ppm is automatically added so do not include this  
Set to 'preview' together with best-preview feature enables special naming convention for preview shots. See motion guide for details

```
%Y = year, %m = month, %d = date,
%H = hour, %M = minute, %S = second, %T = HH:MM:SS,
%v = event, %q = frame number, %t = camera id number,
%D = changed pixels, %N = noise level, \n = new line,
%i and %J = width and height of motion area,
%K and %L = X and Y coordinates of motion center
%C = value defined by text_event - do not use with text_event!
You can put quotation marks around the text to allow leading spaces
```

##### webcontrol_local
*off*

Restrict control connections to localhost only

##### webcontrol_html
*off*

Output for http server, select off to choose raw text plain