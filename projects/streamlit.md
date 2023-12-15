---
layout: project
type: project
image: img/streamlit.jpg
title: "Web Application to Automate Data Annotation Pipeline"
date: 2022
published: true
labels:
  - Python
  - Computer Vision
  - Automation
  - Data Analytics
summary: "Developed a Streamlit web application to automate parts of getting video data annotated before being fed into machine learning models."
---

Nowadays, one the most expensive aspects of getting a machine learning model trained is acquiring a large volume of quality training data.  State of the art neural networks especially require immense amounts of data and this does not come cheap.  In computer vision applications, raw video must be scanned and annotated by humans in order to prepare training data.  Annotation in this context means drawing boxes, n-sided polygons around the contours of an object, etc. in a manner that completely encapsulates a desired object while minimizing image pixels that do not belong to said object.  It is a tedious process that must be executed for every frame in a video which adds up quite quickly.  As such rather than using every frame, it is much more efficient to only utilize frames that contain an object of interest to annotate.

Streamlit which handles much of the backend networking and provides high-level objects for easily deploying a functional web application.  At my current job, I utilized Streamlit to develop a Python application to automate many parts of extracting appropriate training data clips from several terabytes of raw video data.  This included using ffmpeg to clip specific portions of video and down sampling framerates of video, using the API of our annotation platform to automatically generate tasks for our annotators after creating the clip, automatically moving these tasks to different workflows as annotators marked tasks as complete, etc.  In addition, I was able to perform a multitude of analytics functions such as monitoring how many tasks our annotators had in a menu as well as using latitude/longitude data to pinpoint the location of where video data was collected.
