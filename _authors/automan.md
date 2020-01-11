---
date: 2019-05-16 23:48:05
layout: post
title: AutoMan
subtitle: Automation Tool - AutoMan
description: >-
  AutoMan is an application base on OpenCV/Qt5.12.4. It can use to do automation testing, or do some repeated work.
image: >-
  https://res.cloudinary.com/dm7h7e8xj/image/upload/v1559821647/theme6_qeeojf.jpg
optimized_image: >-
  https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1559821647/theme6_qeeojf.jpg
category: Software
tags:
  - welcome
  - blog
author: sharklib
paginate: true
---

# AutoMan
### <a href="files/automan.zip">Mac Version </a> <a href="files/AutoMan.msi">Windows Version </a><a href="">Linux Version</a>
AutoMan is an application base on OpenCV/Qt5.12.4. It can use to do automation testing, or do some repeated work.

#code
Here is a exsample. I want to download all music from sq688.com, but it is a boring work if you had tried to do so. Here is the way I used AutoMan to finish download music of Michael Jackson https://www.sq688.com/detail/463.html
   Step 1: Find the link of album of  Michael Jackson. https://www.sq688.com/detail/463.html
   Step 2: Downlaod one song from this site and record the steps and those key screenshot.
   Step 3: Make a script to repeat those steps:
 ```
    open(https://www.sq688.com/detail/463.html)
    for($links)
        click($links)
        wait($open)
            click($open)
                wait($get)
                    cmd(ctrl+v)
                    click($get)
                        wait($save)
                            click($save)
                            wait($ok)
                                click($ok)
                            
                        cmd(ctrl+w)
            cmd(ctrl+w)
```
In this sample, AutoMan will use OpenCV to find out all elments like links, and click them, in the subsquent web, find the object like open, get, save, ok and click, in the middle of them, call a shotcut "Ctrl+V" to paste the content in clipboard to a text area, when all steps are done, call another two shotcut "Ctrl+w" to close a web tab.
<img src="/assets/img/downloadsong.png">
<iframe width="560" height="315" src="https://www.youtube.com/embed/I3OswJPI1I0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

   

