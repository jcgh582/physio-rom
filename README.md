# Physio-ROM: Computer vision, AI Range of Motion Reports

## What is Physio ROM

Physio ROM stands for "Automated Range of Motion (ROM) calculation for
Physiotherapists" and is an application that helps physiotherapists and their
patient to understand the range of motion of their limbs around a joint.

This is important for people that have hurt their joints or for some other
reason and cannot use their limbs to the full extent, such as after a fall,
a stroke, or an accident.

The project includes a fully-featured cloud-based API which wraps state-of-the-art 
real-time keypoint dectection methods into an accessible and open-source platform.

<a href="http://www.youtube.com/watch?feature=player_embedded&v=6p6oaIISKdM
" target="_blank"><img src="http://img.youtube.com/vi/6p6oaIISKdM/0.jpg" 
alt="It Waves Back!" width="480" height="360" border="10" /></a>

## Installation

The system consists of:
1. a Web front-end that captures the video,
2. a python based backend system to where the video is uploaded and pre-processed,
3. a C++ based computer vision system that calculates the limbs and the angles
on the limbs from the pre-processed video,
4. a Web service that serves a JSON file with the results
5. a Web page that serves the video with the results rendered and overlayed on top of the video.

The Web front end through which the video is captured can be found in the templates directory.

The python based backend system is run via the ```__main__.py``` python program.
Auto-setup is set up via the python virtualenv. Install python virtualenv and run the
following

```
make env
. env/bin/activate
make getreqs
```

The C++ based computer vision system requires the installation and compilation
of the openpose software suite. This is detailed in the [openpose
documentation](https://github.com/CMU-Perceptual-Computing-Lab/openpose/tree/master/doc).
The dependencies for this package are listed in requirements.txt.

## Usage

### Server Usage
Start the flask server by running:

```
python __main__.py
```

The server side implementation has, thus far only been tested on unix systems. We
cannot guarantee. Certain linux distros may require running the code in sudo, because 
in-browser web camera streaming only functions in web ports (apparently security reasons)

### API Workflow

*Index Page:* Hosts a simple video capture framework, which includes a graphical interface to the API.

```
<host>
```

*Uploader:* Dedicated video update page (GUI), and interface to CLI post/get upload of video.

```
<host>/upload
<host>/uploader
```

The uploader returns a unique string called `runid` in JSON object. This is the your key to your data in the API. The uploaded videos are provcessed in ffmpeg and can be in any standard format.


*Process video:*  Passes uploaded video to openPose and returns the features of intrest in json format.

```
<host>/process?runid=<runid>
````


*View video:* Shows a player which loops the uploaded video along with
identified pose.

```
<host>/airom/playvideo?runid=<runid>&fps=<view-framerate>
```

*Annotate video:* Annotates the previous video with the angle of the specified
joint (where the number corresponds to a particular joint).

```
<host>/airom/getoverlay?runid=<runid>&joint=<joint-label>
```
Joints labelled by index in the following list:
['Right elbow','Left elbow','Right Shoulder','Left Shoulder','Right Knee','Left Knee','Right Hip','Left Hip']


*Watch Annotated video:* Shows a player which loopes the uploaded video with
both identified pose and annotated angle.

```
<host>/airom/playoverlay?runid=<runid>&fps=<view-framerate>
```

*Generate report:* Generate a report for a given joint

```
<host>/airom/getreport?runid=<runid>&report=<report-template>
```

### Futher API Tools

```
<host>/airom/getframe?runid=<runid>&framenum=<frame-index>
```
Gets frame at index.

```
<host>/airom/getname?runid=<runid>&framenum=<frame-index>
```
Gets frame by name.

Further detailed usage information is included in the doc/ folder.

## Third-party dependencies

Third-party dependencies that have been used or modified for this
project that have their own software licence include:

| Software | Copyright | Licence | Link | Use              |
 ---------:|:---------:|:-------:|:----:|:----------------:|
| openpose | -         | LGPL    | -    | Pose detection   |
| caffe    | -         | BSD     | -    | ML framework     |
| numpy    | -         | BSD     | -    | Faster searching |
| flask    | -         | BSD     | -    | web serving      |

## Copyright and Licence
Copyright (C) 2017 Aqeel Akber and collaborators - All Rights Reserved

You should have received a copy of the [licence][LICENCE] with this file. If not,
please write to: Aqeel Akber <aqeel.akber@gmail.com>. This licence
applies to all files held under the copyright of this project
only and not third-party dependencies.

If you believe that anything in this project infringes your
copyright, please contact us with your information and a detailed
description of your intellectual property, including the specific
place in this project where you believe your intellectual property is
being infringed.

## Contributing ♥

Those interested, see [CONTRIBUTING][CONTRIBUTING.org] for guidelines.
