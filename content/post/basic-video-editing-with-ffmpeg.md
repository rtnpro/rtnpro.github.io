+++
author = "Ratnadeep Debnath"
categories = ["planet", "rtnpro", "ffmpeg", "video", "audio", "editor"]
date = 2016-08-17T18:04:40Z
description = ""
draft = false
slug = "basic-video-editing-with-ffmpeg"
tags = ["planet", "rtnpro", "ffmpeg", "video", "audio", "editor"]
title = "Basic video editing with ffmpeg"

+++


The following is my cheat sheet for basic video editing with **ffmpeg**

## split

When I want to split a video ``input.mkv`` from 10s to 30s and save it as ``output.mkv``:

```
ffmpeg -i input.mkv -ss 10 -t 30 -vcodec copy -acodec copy output.mkv
```

## concatenate

Let's say, I want to join (concatenate) 2 video (or audio) files: ``part1.mkv``, ``part2.mkv`` into a single file: ``joined.mkv``. I will first create a file: ``input.txt`` with the following content:

```
file part1.mkv
file part2.mkv
```
and then execute the following
```
ffmpeg -f concat -i input.txt -vcodec copy -acodec copy joined.mkv
```

## add audio to video

When I want to superimpose an audio track on top of a video track:
```
ffmpeg -i video.mkv -i audio.ogg -c:v copy -c:a copy output.mkv
```

## speed up

If I want to speed up a video ``input.mkv`` 4 times, I will do the following:

```
ffmpeg -i input.mkv -filter:v "setpts=0.25*PTS" output_4x.mkv
```

## strip audio from a video

```
ffmpeg -i [input_file] -vcodec copy -an [output_file]
```

