---
title: Working with git remotes
layout: post
---

These instructions will be from the ground up. If this ain't your first repository rodeo, well, then, just go ahead and do your thang! 

### Stick a fork in it!
1. Sign in to Github.
2. Fork the repo - click that little green button! 

Now you've got two references to the same project, one listed under `someoneelse/coolproject`, and a new one that all yours at `madgorillascientist/coolproject`... or whatever your name is. And right now, they're the same. And they're both not on your computer yet. Let's fix that.

3. Go to your Terminal, `cd` and `mkdir` your way to the place where you'd like to keep this project (I keep mine, speaking in absolute terms, at /Users/ia/dev/coolproject.) 

You're there? OK. Now, you may be thinking, "Clone! Clones!". And yep, you can do that. If you want, clone away. You won't break anything. But what I'll suggest is to start with a pull. Why? Because it's a little bit more transparent/thorough a way to see what's going on. 

4. Still in your special project directory place? Good. Then we're going to make a git repository of your empty folder and connect it with these two repositories in outer space. 

This'll make that directory you're in officially git-ed... got it? 
```
$ git init 
```


### Outer space
Now you've got to tell git that there are are __remotes__ that you want to this project to talk to. Remotes, with an s. The first of these remotes is going to be someoneelse/coolproject, the official master plan to rule them all. I use the convention of calling this repository 'upstream'. The second will be your own copy of it, which is yours. This one I call 'origin'.
```
$ git remote add upstream https://github.com/someoneelse/coolproject.git
$ git remote add origin https://github.com/madgorillascientist/coolproject.git
```
If this didn't work it probably because Github doesn't know that that computer is yours. You'll want to do some googling about adding a public key to your Github profile. If it DID work, awesome. Onwards. 

Now we're going to actually _get_ the code from the remote repos onto your machine so you can hack away at your leisure. 

Getting the remote via 'git pull' actually does two commands at the same time: 

1. 'git fetch', which grabs all the code that you don't have from the remote repo, and 
2. 'git merge', which, well, merges the two together. 

Right now, that's going to fetch everything you don't have, which is everything, and merge it into, well, nothing. Which is OK.  
```
$ git pull upstream master
```
And for a little science project, you can run `$ git pull origin master` and see what happens.

Now take a peek at your local project directory; you should have it all there in working order. 

### Branches save lives

Just hold on one minute, hotshot, before you start hacking away. There's just one thing left to do. Branch! A little branching never hurt anyone, and it has saved the lives and destinies of many a soul. 

This'll create a new branch called 'madgorillascientist-exploratory-mission' and put your HEAD on it. What's a HEAD? A HEAD is where your head's at. Where your gitty little eyes are at. 
```
$ git checkout -b madgorillascientist-exploratory-mission
```

Alright - go for it! Type stuff! Make weird improvements! Make a comment on the strange code in front of you. Do it! 

Now, just cuz, let's try putting those changes in outer space. 

This adds everything that has been modified to _the stage_, as I call it. Well, to _staging_, as they say. (Note that instead of a '.', which adds everything that has changed, you can instead use 'thatonefile.md' or whatever, and stage on a per-file basis. For now, '.' will be just fine. 
```shell
$ git add . 
$ git commit -m "Arooo000oo0ooooo00oooooo0ooooooooo" # A good first commit message is important.  
```


### Push it, push it good! Ahhh...

Alright, you're staged and committed and you've officially dun work on the repo. Outer space time. Push them changes to your 'origin'. This is _your_ outer space version of the repo.
```
$ git push origin madgorillascientist-exploratory-mission
```

Pushing to outerspace is great. It keeps your code safe from cataclysmic events like spilling coffee on your computer. It can make your work shareable. It can set you up for deploying a project to the actual internet sometimes (or at least getcha in that mindset). 

### Pull me, please.
Now, your remote (aka madgoriallascientist/coolproject aka origin) is different than the main remote (aka someoneelse/coolproject aka upstream). Because I'm sure that whatever changes you made are fully awesome, it's time to incorporate those changes into the big dog. 

Another little green button! This time it's 'Pull Request' we're after. This is going to ask the big dog if he'd like to incorporate your changes. Click it! Ask him! He loves attention. What's gonna happen is I - me - and the other admins of the big dog are gonna see a little blue dot pop up telling us that we have a notification about a pull request. We're gonna look at, discuss amongst ourselves, confer, check the gallup polls, and then if you're comment is up to snuff and fully qualified awesome (odds are on your side, kid), then we're gonna accept and merge the request and you're in! It's live! It happened! 

So we accept the request. Let's say it's been a particularly busy day and your request wasn't the only one we accepted. Whoa! That means the big dog now has your changes, and the changes from some other heartful volunteer slob. But you only have the changes the _you_ made?! Yep, fresh, you've gotta stay fresh with this kind of thing. 

You've got options my friend. If you're feeling bold (and I often am), and fast (always inclined), then you can run another `$ git pull upstream master`, which'll get and commit all the upstream stuff you don't already have. If you're feeling a little tentative, maybe because you've been working on some _even more_ stuff, then break that `pull` down into littler pieces by running `$ git fetch`, which grabs everything you don't have (remember: it's going to come in from the upstream:master branch and into your local:master branch; not necessarily the branch you're working on) so you can look at it and make sure it doesn't fuck up what you've got going in the meantime - and, if it all checks out and looks like it won't break your junk, then you can `add` it, `commit` it, and finally `merge` it into your branch.  

A la...
```
# Fetch the big dog.
$ git fetch upstream master
# Let's say you're still on branch madgorillascientist-exploratory-mission, for the sake of argument...
# You can see the newest freshest dog on master
$ git checkout master
# You can see at a glance the changes it'll introduce:
$ git status
# You can see the difference between the old and new masters 
$ git diff HEAD..master
# You can see the difference between the freshy and your gorilla branch
$ git diff HEAD..madgorillascientist-exploratory-mission

# Look ok? (You're still on master, remember).
$ git add .
$ git commit -m "fresh big dog incoming"
# Now if you run 
$ git diff master..madgorillascientist-exploratory-mission # you'll see the same thing as 
# you saw above with $ git diff HEAD..i don't want to type that whole long thing out again
# Alright, so you want those changes in your long-ass-branch-name too?
$ git checkout madgorillascientist-exploratory-mission
$ git merge master
```

Bear with me, we're almost through the circuit. Now you've got the freshest of the fresh in your custom branch, but, once more, your machine isn't synced with it's remote 'origin'. `$ git push origin madgorillascientist-exploratory-mission`. Do it. Done. 

And that's how it goes. 

----

## Things I tend to do
Git-wise...

- Run at least a `git fetch upstream master` (if not `pull`) every time I sit down to begin work on the project, just to make sure I'm up to snuff. 
- I branch a lot. I didn't get into branches much in this run through, but enough to say that they can save your ass, and will never break your ass. Use em. 
- I `push` a lot. I've got an alias as `gpo` which pushes to origin (so its fasttttt), and my global git config is set up to default to push to the corresponding branch on the remote (so I don't have to worry about accidentally pushing my -exploratory-mission-branch to master)