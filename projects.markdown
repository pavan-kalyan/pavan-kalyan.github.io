---
layout: page
title: Projects
permalink: /projects/
---

This page contains the list of all notable projects that I am working or have worked on. Some of these projects usually solve a personal pain point or they are just interesting to me. All projects noted here are Open Source.

### [Kontests Unofficial](https://github.com/pavan-kalyan/Kontests-Unofficial){:target="_blank"}


This flutter app lists all of the competitive coding competitions (across sites like codeforces.com, atcoder.jp, leetcode.com) in a neat UI. It contains contest timing and other info sorted on start time. The data is retrieved from [Kontests.net](https://kontests.net/){:target="_blank"}.

Main motivation to build this app was that I wanted to keep track of the different upcoming contests so I could avoid missing them and I wanted to view this info on my phone.
Future improvements will be to allow syncing with Google Calendar and set up notifications for certain contests.

Latest build (APK) is available [here](https://github.com/pavan-kalyan/Kontests-Unofficial/releases){:target="_blank"}. Currently, it's only available for Android, but since it's built on Flutter, same code can be used to build an IOS app.


### i3 Intelligent Display Management (Upcoming)

This software is designed to detect hotplugging of monitors and automatically restore previous layout. It's written in Go and utilizes inter process communication to efficiently manage the layouts.
It's intelligent enough to differentiate between monitors and outputs, which is particularly useful when using docks.


### Budgeter (Upcoming)

This app is designed to automatically track spending by reading SMS messages and email. Current plan is to use Google Sheets as backend/database (for easy data portability and zero-cost infra).
It would also have data analysis and reporting capability.



### [Digital Stethoscope](https://github.com/pavan-kalyan/DigitalStethoscope){:target="_blank"}

This project is designed to diagnose heartbeat audio from phone microphones using deep learning. It mainly attempted to detect [Heart Arrhythmia](https://www.mayoclinic.org/diseases-conditions/heart-arrhythmia/symptoms-causes/syc-20350668){:target="_blank"}. The main motivation was to allow preliminary diagnosis in remote regions of India where access to healthcare is severely limited.
The app also allowed sending the audio remotely to medical professionals for further diagnosis.

I lead this project along with 2 others as part of the [Robotics and Circuits Club](https://roboticsandcircuits.com/index.html#home){:target="_blank"} at MIT, Manipal.

The app code is available [here](https://github.com/pavan-kalyan/DigitalStethoscope){:target="_blank"} and the machine learning code and model is available [here](https://github.com/pavan-kalyan/heartbeat-audio-CNN){:target="_blank"}.