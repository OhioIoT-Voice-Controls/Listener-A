# Listener A<a href="https://www.ohioiot.com"><img src="https://www.ohioiot.com/logo_150.jpg" width="40" ></a>

##### [(back to the Voice Controls organization page)](https://github.com/OhioIoT-Voice-Controls)

This is the most basic implementation of our Vosk listener.  The `docker-compose.yml` will spin up a Vosk listener and an MQTT broker.  Anything you say that is picked up by Vosk will be sent as an MQTT message with topic `voice/command` where the payload is the collection of words spoken.  

You can see this repo in use in the OhioIoT YouTube video [3 Steps To Your Custom Voice Control](https://youtu.be/_ERvoHMBDac).

## Installation
Plug a USB microphone into a Raspberry Pi that has Docker and Docker Compose installed.  SSH into the Raspberry Pi and run the following commands:
```
git clone https://github.com/OhioIoT-Voice-Controls/Listener-A.git listener_a
cd listener_a
rm listener.py README.md
docker compose up
```
At this point whatever speech that is interpreted by Vosk will be sent out to the Mosquitto broker in the payload of an MQTT message, with topic `voice/command`.  Once you confirm the IP address of your Raspberry Pi, you can connect any other device to the Mosquitto broker, exposed on port 1883.  Your connected devices can subscribe to `voice/command` and hear what you are saying in the incoming message payloads.

The `listener.py` in this repo is just an artifact, here for reference only.  You cannot run this file in this root directly with its current configuration.  To witness this file running inside the container on the Raspberry Pi, when the container is running, type:
```
docker exec -it listener sh
```
And then, when inside the Listener container (`# `):
```
cd /app
ls -la
cat listener.py
```

## Links
- [OhioIoT YouTube Channel](https://www.youtube.com/@ohioiot) - Agenda free tutorials showing you how to get started in IoT
- [OhioIoT GitHub Index](https://github.com/OhioIoT-Examples) - The central index of code examples available on GitHub

## About
<a href="https://www.ohioiot.com"><img src="https://www.ohioiot.com/logo_150.jpg" width="40" ></a>

*OhioIoT is an IoT platform designed for small-scale IoT projects.  For more, check out our website at [www.OhioIoT.com](https://www.ohioiot.com).*
