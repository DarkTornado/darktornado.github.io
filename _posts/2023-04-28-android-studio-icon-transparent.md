---
layout: post
title: Android Studio Add Image Asset with Transparent Background
date: 2023-04-28 00:08:00 +0900
img: android-studio-icon-transparent.png
---

## Summary

1. Add icon as `Laucher Icons (Adaptive and Legacy)`.
1. Remove `{icon_name}` and `{icon_name}_background`.
1. Rename `{icon_name}_foreground` to `{icon_name}`.

***

In the past, we could use icon with transparent background.
So we could use image with transparent background as icon.

But, From several years ago, Android Studio started to require icon's background color when I add icon via "Image Asset" menu.

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/0.png)

<br>
We must select color without transparent even if original image has transparent background

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/1.png)

<br>
However, we can add icon with transparent yet. First, select `Laucher Icons (Adaptive and Legacy)`.
What you should see is `Full Bleed Layers`.

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/2.png)

<br>
I selected `Color` on `Background Layer` tab. We will remove it later, so just select whatever you want.

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/3.png)

<br>
Click `Finish` to add icons.

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/5.png)

<br>
In this post, icon's name that I added is `flash_on`.
Wow. `flash_on` icon is so awful.

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/6.png)

<br>
But, `flash_on_foreground` is very nice.
It is what I wanted. Icon with transparent background.

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/7.png)

<br>
So, I will remove `flash_on` and rename `flash_on_foregroound` to `flash_on`.

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/8.png)

<br>
I also removed `flash_on_background` too.

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/10.png)

![image]({{site.url}}{{site.baseurl}}/assets/images/android-studio-icon-transparent/11.png)

<br>
Good. This is what I wanted.

![image]({{site.url}}{{site.baseurl}}/assets/images/thumb/android-studio-icon-transparent.png)
