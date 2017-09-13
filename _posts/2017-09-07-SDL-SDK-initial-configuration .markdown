---
layout: post
title:  "SDL SDK initial configuration"
date:   2017-09-07 21:50:00 +0300
categories: SDL SDK
---
I want to build a plug-in for SDL Trados Studio 2017. Since the initial configuration of the SDL SDK is a bit complicated, I am going to record some of the details here in case I need to backtrack.

* Step 1: Install [Visual Studio 2017 Community Edition](https://my.visualstudio.com/downloads/featured).

  This step was not too hard once I figured out that the [SDL advice](http://producthelp.sdl.com/SDK/StudioIntegrationApi/4.0/html/60cb0474-a3eb-44ec-b7b3-cef989fd8fe1.htm) to install Microsoft Visual Studio 2010 was severely outdated. Visual Studio 2017 takes up 12 GB of disk space, which is a little hard to fathom after working mostly with Linux tools in recent times. I also had to figure out that Avast was blocking the installation process.

  Just a couple of notes: 

  1. Download Visual Studio [here](https://www.visualstudio.com/vs/community/?wt.mc_id=o~co~MSDN~WEBSITE~FY16~11Nov~LearnVSCommunity).

  2. See what software Microsoft has available [here](https://my.visualstudio.com/downloads/featured) (log-in required).

* Step 2: Install the SDL SDK. 

  In principle, this is just a matter of downloading the file `SDLTradosStudio2017SDK.exe` and running the installer. Unfortunately, the SDL templates were not available in Visual Studio 2017 after I ran the SDL SDK installer and it finished running without any output (usually a bad sign). A search of the SDL SDK forum found [this link](https://community.sdl.com/developers/language-developers/f/61/t/12467) as well as [another relevant link](https://community.sdl.com/developers/language-developers/f/61/t/6971). 

  Thus, I had to change the User Projects Template location from (default location) `C:\Users\garchik2\Documents\Visual Studio 2017\Templates\ProjectTemplates` to (SDL SDK location) `C:\Program Files (x86)\Microsoft Visual Studio 15.0\Common7\IDE\Extensions\SDL\SDLAppStore2017\5.0\ProjectTemplates`.

  Here are some notes: 

  1. The directory `C:\ProgramData\SDL\SDK 5.0` also seems to be important. I think it contains sample projects.

  2. The most relevant SDL forum is [here](https://community.sdl.com/developers/language-developers/) ("Language Developers").

  3. One of the main SDL SDK developers has a very useful series of blog posts [here](https://romuluscrisan.com/openexchange-age-of-developers/).

  4. SDL also has (outdated?) [online help](http://producthelp.sdl.com/SDK/StudioIntegrationApi/4.0/html/60cb0474-a3eb-44ec-b7b3-cef989fd8fe1.htm).


