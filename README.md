# ImageViewer
Google Chrome offline webpage for casually viewing folders of images.  
  
Required:  
L> Web browser [currently only new-ish versions of Google Chrome]  
L> A folder of many images [but intended for ≤10,000 images]  
  
Use case and reasoning  
On my computer, I have a folder named "pixivRoot". Inside are many folders and one text file. The folders are named after pixiv artists, and the contents of those folders are drawings by the respective artist. I use an Apple laptop, so the "Shifting Tiles" screensaver is nice for casually viewing random images from my pixivRoot folder, but there aren't enough options for my liking. There also seems to be a randomization issue when using that screensaver with multiple monitors. I wanted to make a similar service, but change a few things for the better [in my opinion] and also offer many options to change how the service works.  
A list of important changes that make this service different from the "Shifting Tiles" Apple OS screensaver:  
• Option to set the number of desired image rows.  
• Option to set how often new images appear.  
• Ability to pause.  
• Ability to see the file path of an image.  
• More predictability on which images will disappear when.  
  
!!! Browsers will not run this app locally  
• Aside from debating the reasoning, browser will not allow some file-related features when JavaScript is run locally [you save an html file with JavaScript in it and run it on your computer]. That means this .html page has to be running on a server. Unless/Until I think of a better place to host this service, you can view it here : [http://htmlpreview.github.io/?https://github.com/sliceofcake/ImageViewer/blob/master/folder.html](http://htmlpreview.github.io/?https://github.com/sliceofcake/ImageViewer/blob/master/folder.html). Here's one of the few places to find information on this : [https://developer.mozilla.org/en-US/docs/Web/API/FileError](https://developer.mozilla.org/en-US/docs/Web/API/FileError) see the section "Don't run your app from file://", which says "For security reasons, browsers do not allow you to run your app from file://. In fact, many of the powerful storage APIs (such as File System, BlobBuilder, and FileReader) throw errors if you run the app locally from file://. When you're just testing your app, and you don't want to set up a web server, you can bypass the security restriction on Chrome. Just start Chrome with the --allow-file-access-from-files flag. Use the flag only for testing purposes."  
  
!!! Google Chrome memory reclamation issue  
• As of October 2016, Google Chrome has what I'm going to call a bug - Chrome won't free up computer memory in a reasonable fashion. Although I make attempts to deload images that go offscreen, memory seems to steadily climb as an equal amount of new images are brought in, whereas memory ~should~ stay at about a fixed amount, since we're equally loading and deloading images. I have put a large effort into investigating the issue and have concluded that Google Chrome has a memory reclamation issue. This issue will affect you if you keep the image viewer open for a long period of time [technically -> by how many images have been brought in while the service has been running]. You will notice your memory usage go up over time. Refreshing the page will not free that memory - you must close the tab. Furthermore, the "GPU Process" item in Google Chrome's Task Manager will steadily use more memory, and will seemingly not free that memory until you close the browser. As a positive, if you use a small pool of images, they'll eventually all be seen by Chrome and as a result, memory usage for the webpage will cap off.  
  
Notes  
• When the webpage goes out of sight, it will pause itself. This is intended.  
  
A note from me to you about using a service like this  
Always be skeptical about using a service which has you drag and drop files onto it. Although I'm not sure regarding specifically the way that I collect images here, in general whenever you drag and drop files onto a webpage, you should assume that that webpage is able to send those files to a server without your knowledge nor approval. If you know how to read JavaScript, or you know someone else who does, you could take a look at such code to see if it looks safe to use [no malicious sending to a server somewhere]. The only way to know for sure that such a thing isn't happening is to disable your network connection when using the service. This is obviously unreasonable in most casual settings, so your only remaining options are to either trust the service developers for some strong reason, or only ever drag and drop files that you don't consider to be private or confidential, so that even if they do get sent off to a server somewhere without your knowledge or approval, that wouldn't be that bad of a thing to happen.  
  
Everything that could go wrong, within reason  
• It freezes : if the image viewer is playing, press the spacebar twice. The first time will pause, the second time will play. If it's paused, press the spacebar once to play. This version isn't diamond-solid, so it's possible that the code could impatiently advance even without the images being loaded, resulting in issues. Telling the service to play again [optionally after first telling it to pause] will give it time to reevaluate its loaded images and properly fill them. Eventually, this service should reach the point where all these possible race conditions are gotten rid of, but for now, this unfortunate event could occur, especially with large images.  