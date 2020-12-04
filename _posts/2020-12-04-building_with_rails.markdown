---
layout: post
title:      "Building with Rails"
date:       2020-12-04 20:09:09 +0000
permalink:  building_with_rails
---


![](https://static.wikia.nocookie.net/tinytoons/images/7/76/ThirteenSomething-WileECoyotePaintsATunnel.JPG/revision/latest?cb=20160805110930)

Thus far, my journey through Flatiron School has been pretty smooth. The logic of programming makes sense, and it is just a matter of learning the new rules and applying them. Easy! Upon reaching the Rails project, I knew a couple things:

1. I found Sinatra easy to work with
2. The requirements for the Rails Project were a bit harder than those for the Sinatra Project
3. Rails either automates, or allows gems that automate, many of the friction points from working on Sinatra.

With these three things in mind, I was ready to go, confident in a breeze through the week.

##Pride before the Fall
Work was going great. I was building out my User model, getting its controller set up, and starting to write the sign up views. Several hours in was when I realized my mistake. I still had hours of work ahead of me, and Devise is a gem which exists.

Oops!

One pitfall of having so many tools at my disposal, I had forgotten about the one that would actually make my life easier! At this point in my build, the most efficient option was to delete the entire thing and start over. It was at this point that VSCode decided it wouldn't work anymore. Kids, always keep your applications up to date. There are few feelings like accepting the loss of hours of work with the promise of more efficiency, only to discover that now you cannot work at all. 

The issue, I later learned, was the order in which I created my local files and my github repository. On previous projects I had linked an empty folder on my computer to the repo, then filled it in later. Apparently with rails, that doesn't work. After switching to generating my rails project fully and THEN connecting it to github with `git init`, it started to work again. The tunnel painted on the rock opened and let me in.

Things were smooth sailing through the instalation of Devise, and then came OmniAuth. You see, the guide that I was using had typos. I honestly don't know whether they were in fact typos, or if during one of the updates to the software the syntax changed in a couple places. Devise worked like a charm for logging in. While I dislike not having easy access to the UserController, it did in half an hour what would have taken me a day to do by hand. When I tried to log in through GitHub, however, all the app did was rerender the sign_up view. After putting a `binding.pry` in the `signup_params`, I learned that OmniAuth wasn't even getting to that point. It couldn't reach the Registrations#create action in the first place. A real challenge when I couldn't easily access that controller either (thanks, Devise). It was here that I learned of the typo, which perceptive readers may have already found.

You see, the guide I used said to write a `signup_params` method for the Registrations controller to use when signing a user in.  It was here that I learned the joys of `rails routes`. After digging deep and putting `binding.pry` on almost everything, someone recommended  `rails routes` just to see what the routes were. I know about that command already, but I had been using rails for a while, and I thought I understood the conventional names for the routes, so I didn't need it. Had I used it earlier, I would have learned that Devise doesn't say "signup". It says "sign_up". So the params method should have been `sign_up_params`. That one change, and suddenly my app worked.

The moral of this story is to always check your routes. Even if you know your routes, check them in case they change. Hubris is a tough thing. It can force us to bash our heads against that rock wall as we refuse to see that maybe the roadrunner is just an extradimensional being that can phase through matter.

Seriously, though, failure and bashing your head against a wall is the best way to learn. While it can be frustrating, it is in friction and failure that we learn our shortcomings, and only in overcoming them can we improve.

After those initial problems with my build, the rest went pretty smoothly. I haven't lost faith in myself as a coder, and I know that come the next project, I will run straight at that tunnel, painted into the side of the wall.

![](https://s3.crackedcdn.com/phpimages/article/6/3/8/558638_v2.jpg)
