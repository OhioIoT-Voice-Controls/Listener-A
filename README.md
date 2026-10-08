# Listener A<a href="https://www.ohioiot.com"><img src="https://www.ohioiot.com/logo_150.jpg" width="40" ></a>

##### [(back to the Voice Controls organization page)](https://github.com/OhioIoT-Voice-Controls)

This is the most basic implementation of our Vosk listener.  The `docker-compose.yml` will spin up a Vosk listener and an MQTT broker.  Anything you say that is picked up by Vosk will be sent as an MQTT message with topic `voice/command` where the payload is the collection of words spoken.  

You can see this repo in use in the first part of the OhioIoT YouTube video [3 Steps To Your Custom Voice Control](https://youtu.be/_ERvoHMBDac).

## Installation
*** Before you run this code, check out the security recommendation below ***

Plug a USB microphone into a Raspberry Pi that has Docker and Docker Compose installed.  SSH into the Raspberry Pi and run the following commands:
```
git clone https://github.com/OhioIoT-Voice-Controls/Listener-A.git listener_a
cd listener_a
rm listener.py README.md
docker compose up
```
When you see `listening...` in the container logs, the system should be up.  At this point, anything Vosk hears you say will be sent out as the payload of an MQTT message with topic `voice/command`.  Once you confirm the IP address of your Raspberry Pi, you can connect any other device to the Mosquitto broker, exposed on port 1883.  Your connected devices can subscribe to `voice/command` and hear what you are saying in the incoming message payloads.

The `listener.py` in this repo is just an artifact, here for reference only.  You cannot run this file in this root directly with its current configuration.  To witness this file running inside the container on the Raspberry Pi, when the container is running, type:
```
docker exec -it listener sh
```
And then, when inside the Listener container (you'll see `# `):
```
cd /app
ls -la
cat listener.py
```
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
This YouTube video and Git repo were created in good faith.  However, you probably shouldn't run someone else's Docker container if you don't trust it.  You can verify that the container being pulled by this repo with the following:
```
docker run -d --network=none --name=listener --device /dev/snd --group-add audio lvincek/listener_a:latest
docker inspect listener
```
Look at the result from the `inspect` command.  You will notice that the working directly is /app, and the command that the command that is run is `python -u listener.py`.  With that, you can step into the running container with:
```
docker exec -it listener sh
```
And then, print the file on your screen, and you will see that it is in fact the listener.py that you see in this repo.
``
cat /app/listener.py
```
In all cases, are there any question, check out [Listener C Build](https://github.com/OhioIoT-Voice-Controls/Listener-C-Build) and [Listener C](https://github.com/OhioIoT-Voice-Controls/Listener-C).  For Listener C, you build the container image yourself, so any security concerns should be assuaged.

## Links
- [Listener B](https://github.com/OhioIoT-Voice-Controls/Listener-B)
- [Listener C Build](https://github.com/OhioIoT-Voice-Controls/Listener-C-Build)
- [Listener C](https://github.com/OhioIoT-Voice-Controls/Listener-C)
- [OhioIoT YouTube Channel](https://www.youtube.com/@ohioiot) - Agenda free tutorials showing you how to get started in IoT
- [OhioIoT GitHub Index](https://github.com/OhioIoT-Examples) - The central index of code examples available on GitHub

## About
<a href="https://www.ohioiot.com"><img src="https://www.ohioiot.com/logo_150.jpg" width="40" ></a>

*OhioIoT is an IoT platform designed for small-scale IoT projects.  For more, check out our website at [www.OhioIoT.com](https://www.ohioiot.com).*
