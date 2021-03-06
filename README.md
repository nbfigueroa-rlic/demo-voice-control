## Voice control for Easy Kinesthetic Teaching with pocketsphinx

Code adapted from https://github.com/gorinars/ros_voice_control.git

The script shows how to control a gripper and start/stop measurement recordings with English keywords using pocketsphinx

NOTE: The recognition engine is not perfect as it is a simple HMM, does not reliably reject noise, oftentime has false positives or no recognition after uttering the same word multiple times. The only thing that can be improved is the thresholds in the `.kwlist` or adapt the HMM, this can be done by following this tutorial https://cmusphinx.github.io/wiki/tutorialadapt/. A better option would be to use the Google Cloud Speech engine, for which a ROS-package already exists: http://wiki.ros.org/speech_recog_uc. This is if you need really reliable recognition.

## Installation

### Install pocketsphinx with dependencies
```
sudo apt-get install -y python python-dev python-pip build-essential swig libpulse-dev git
sudo apt-get install libasound-dev
sudo apt-get install libasound2-dev
sudo apt-get install python-pyaudio
sudo pip install pocketsphinx
```

### Install language models
```
sudo apt-get install pocketsphinx-hmm-en-*
```

### Run simple voice recognition example 
The simple voicecontrol_class.py implements the Voice Recognition system for the dictionary defined in 'demo-voice-control/commands/
voice_command_test.dic', once word is recognized it publishes them as a string message. To test run the following script:

```
rosrun demo_voice_control test_voice_control_node.py
```
Speak one of the default commands ( back / forward / left / move / right / stop )

You can see the messages of the words you spoke in:
```
rostopic echo /demo_voice_control/command
```
### Using your own keywords

You can run this with any set of words. To do that, you need lexicon and keyword list files
(check voice_cmd.dic and voice_cmd.kwlist for details). 

Word pronunciations for English can be found in 
[CMUdict](https://sourceforge.net/projects/cmusphinx/files/G2P%20Models/phonetisaurus-cmudict-split.tar.gz)

You can also download pocketsphinx acoustic models for several other languages [here](https://sourceforge.net/projects/cmusphinx/files/)

Read more about pocketsphinx on the official website: http://cmusphinx.sourceforge.net


## Application: Voice-control of Robotiq gripper + Data Recording

### Dependencies
First you must install the [Robotiq](http://wiki.ros.org/robotiq) gripper controller and dependencies, follow the instuctions here: https://github.com/epfl-lasa/lasa-wiki/wiki/Robotiq-gripper

### Examples
The teach_voicecontrol_class.py implements the Voice Recognition system for the dictionary defined in 'demo-voice-control/commands/
voice_command.dic' to open/close the gripper with voice activation and start/stop data recording.  

- To use only the ***gripper commands***, run the following launch file:
```
roslaunch demo_voice_control gripper_voice_control.launch
```
If you speak one of the default commands ( open / close ) the gripper should open / close.

- To run ***full teaching commands*** with voice activation, run the following launch file:

Besides controlling the gripper, this script will trigger record/stop for the data recorder from [record_ros](https://github.com/epfl-lasa/record_ros). Specifically, it will trigger the ros service calls that are generally typed in a terminal:
```
$ rosservice call /record/cmd "cmd: 'record/stop'"

```
with the (recording / stop) commands. To test, run the following launch file: 
```
roslaunch demo_voice_control teach_voice_control.launch
```
If you speak one of the default commands ( open / close / recording / stop ) the gripper should open / close and data recordings will start / stop.

**Contact**: [Nadia Figueroa](http://lasa.epfl.ch/people/member.php?SCIPER=238387) (nadia.figueroafernandez AT epfl dot ch)
