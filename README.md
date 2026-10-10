# Listener A<a href="https://www.ohioiot.com"><img src="https://www.ohioiot.com/logo_150.jpg" width="40" ></a>

##### [(back to the Voice Controls organization page)](https://github.com/OhioIoT-Voice-Controls)

This is the most basic implementation of our Vosk listener.  The `docker-compose.yml` will spin up a Vosk listener and an MQTT broker.  Anything you say that is picked up by Vosk will be sent as an MQTT message with topic `voice/command` where the payload is the collection of words spoken.  

You can see this repo in use in the first part of the OhioIoT YouTube video [3 Steps To Your Custom Voice Control](https://youtu.be/gnBhi573RWg).

## Installation
*** See the Security Recommendations below before you run these commands ***

Plug a USB microphone into a Raspberry Pi that has Docker and Docker Compose installed.  SSH into the Raspberry Pi and run the following commands:
```
git clone https://github.com/OhioIoT-Voice-Controls/Listener-A.git listener_a
cd listener_a
rm listener.py README.md
docker compose up
```
When you see `listening...` in the container logs, the system should be up.  At this point, anything Vosk hears you say will be sent out as the payload of an MQTT message with topic `voice/command`.  Once you confirm the IP address of your Raspberry Pi, you can connect any other device to the Mosquitto broker, exposed on port 1883.  Your connected devices can subscribe to `voice/command` and hear what you are saying in the incoming message payloads.

When you are comfortable that everything is in order, start running the container in the background:
```
docker compose up -d
```
To tear this down when you are down:
```
cd ~/listener_a
docker compose down
cd ..
rm -rf listener_a
```
## Security Recommendation
You shouldn't run a Docker container if you don't trust it.  Rather than trust the one shown in this video, you can verify what is in the container with the following steps.  If this doesn't resolve all questions, you can just skip straight to Listener C ([Listener C Build](https://github.com/OhioIoT-Voice-Controls/Listener-C-Build) and [Listener C](https://github.com/OhioIoT-Voice-Controls/Listener-C)), where you build the container image yourself, so any security concerns should be assuaged.

This runs the container with no privileges:
```
docker run -d --network=none --name=listener lvincek/listener_a:latest
```
This container will start and then immediately fail because it wasn't given access to the sound system.  You can run this command to confirm that the working directory is `/app` and the file being run is `listener.py` in the working directory:
```
docker image inspect lvincek/listener_a:latest --format \
      'WorkingDir={{.Config.WorkingDir}}  Entrypoint={{json .Config.Entrypoint}} Cmd={{json .Config.Cmd}}'
```
With that, you can run the following command to echo out the `/app/listener.py` that is being run:
```
docker run --rm --network=none --entrypoint cat lvincek/listener_a:latest /app/listener.py
```
You will see that this is the file shown in this git repo.  Reviewing the code you can see that it accesses the microphone and interprets speech, as is shown in the video.  When you are done, type `docker rm -f listener` to stop and remove the running container.


## Links
- [Listener B](https://github.com/OhioIoT-Voice-Controls/Listener-B)
- [Listener C Build](https://github.com/OhioIoT-Voice-Controls/Listener-C-Build)
- [Listener C](https://github.com/OhioIoT-Voice-Controls/Listener-C)
- [OhioIoT YouTube Channel](https://www.youtube.com/@ohioiot) - Agenda free tutorials showing you how to get started in IoT
- [OhioIoT GitHub Index](https://github.com/OhioIoT-Examples) - The central index of code examples available on GitHub

## About
<a href="https://www.ohioiot.com"><img src="https://www.ohioiot.com/logo_150.jpg" width="40" ></a>

*OhioIoT is an IoT platform designed for small-scale IoT projects.  For more, check out our website at [www.OhioIoT.com](https://www.ohioiot.com).*
