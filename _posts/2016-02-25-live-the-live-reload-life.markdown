---
title: Live the Live Reload Life
layout: post
date: 2016-02-25 20:30:28
comments: true
---
Live-reload. Once you go... you never go back.

> I typed a bunch of stuff here, but really[ Matt Brictson does a far better job explaining and implementing it.](https://mattbrictson.com/lightning-fast-sass-reloading-in-rails) I use his way now. 

### ? 
So you've got the site your building served locally on, say, port 3000, and you can visit localhost:3000 and see it. Yep. 

And you've got your code, which you're editing with Sublime Text, Atom, or whatever. 

_Without_ live reload you'll make changes to your code and then switch over to the browser and hit command-R to refresh, wait a second, and there're your new changes. 

_With_ live-reload you'll make changes to your code, hit Save, and then look over at your browser and that change you just made will be there (or at least the browser will be already reloading). 

It's the way to do it, no doubt.

### + Grunt, no sweat. 
How I do it is with __Grunt__. Grunt is what they call a task runner&trade;. Grunt is a javascript task runner. 

That means you can tell Mr. Grunt to do things in Javascript and he'll go out and do them for you. 

For example, he likes to do things like 
- run SCSS compilers (which translate SCSS into standard CSS),
- compress (or uglify, or minify) Javascript so that it loads faster in production (yep, another number-crunching translation job),
- manage serving development projects locally, 
- and more!,
- and, of course, handle the magic and completely necessary job of live-reloading your local webpage when you make changes to the code.

So what's this going to look like? 

Well, you'll serve your website in development like normal. Then, in a separate little terminal window, you'll run `$ grunt serve`, and Grunt will start watching for changes to the kinds of files you want to trigger reloads. (When I'm using Rails, I have Grunt watch for saved changes to DOM, javascript, and style sheets. I don't watch for changes to my .rb models, controllers, or any config files.)

But how?? 

What's gonna happen is you'll stick a little piece of JavaScript in the head of your `index.html` file (or wherever the magic happens on the base of your site - with Rails: `application.html.erb`)

That JavaScript will be _listening_ to a specific strange port. Yea - a port like localhost:3000 kind of a port. Say, 6574. 

Grunt is going to listen for changes in your code by hearing when you hit `Save` - maybe it does this by watching for `modified at` headers in the files or something. I don't know. But it will watch. 

And when Grunt hears (or sees? Grunt is a weird animal...) something change, he send a little blurp to that port (the 6574 one) saying "It happened! It happened!". Then the JavaScript will somehow cause the page to reload. 

That's about what I make of it. 

Now, some servers you're going to fire up will have this shit _built in_. Like Jekyll. Some Gulp server setups do it right out of the metaphysical box. Chances are higher you're going to get it baked in if you're using a generator to build a probably JavaScript-heavy app, like... [Ionic] stuff, some [MEAN] stuff, some [Angular] stuff... 


### ++ The gists of it. 
This goes in `index.html`, in the head.
{% highlight html %}
<script src="http://localhost:35729/livereload.js"></script>
{% endhighlight %}

Now, yep, you'll need a bunch of JS Grunt stuff to make it happen. 

~~I like to use [Bower] to handle installing JS packages and stuff. If you don't know that is, google it. It might be important.~~
Node. It's Node that's going to do it. Bower is for other stuff. Use NPM. It's also cool. You can probably run something like `npm install grunt grunt-contrib-watch --save-dev` to get the Grunt dependencies. 

{% highlight json %}
// /package.json
{
  "name": "yep",
  "version": "0.0.0",
  "dependencies": {},
  "devDependencies": {
    "grunt": "~0.4.1",
    "grunt-contrib-jshint": "^0.11.0",
    "grunt-contrib-nodeunit": "^0.4.1",
    "grunt-contrib-watch": "*"
  },
  "engines": {
    "node": ">=0.8.0"
  }
}
{% endhighlight %}

And here's a gist from a Rails project.
{% highlight js %}
// /Gruntfile.js
'use strict';

module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    nodeunit: {
      files: ['test/**/*_test.js'],
    },
    jshint: {
      options: {
        jshintrc: '.jshintrc'
      },
      gruntfile: {
        src: 'Gruntfile.js'
      },
      lib: {
        src: ['lib/**/*.js']
      },
      test: {
        src: ['test/**/*.js']
      },
    },
    watch: {
      gruntfile: {
        files: '<%= jshint.gruntfile.src %>',
        tasks: ['jshint:gruntfile']
      },
      lib: {
        files: '<%= jshint.lib.src %>',
        tasks: ['jshint:lib', 'nodeunit']
      },
      test: {
        files: '<%= jshint.test.src %>',
        tasks: ['jshint:test', 'nodeunit']
      },
      coffeefiles: {
        files: ['app/assets/javascripts/*.js*'],
        options: {
          livereload: true,
        }
      },
      // reload on *html file change
      templatefiles: {
        files: ['app/assets/javascripts/*.html'],
        options: {
          livereload: true,
        }
      },
      // reload on erb file change
      erbfiles: {
        files: ['app/views/{,*/}*.erb'],
        options: {
          livereload: true
        }
      },
      // reload on CSS file change
      cssfiles: {
        files: ['app/assets/stylesheets/*.css'],
        options: {
          livereload: true,
        }
      },
      // reload on SCSS files change
      scssfiles: {
        files: ['app/assets/stylesheets/*.scss'],
        options: {
          livereload: true,
        }
      },
      // reload on any change in stylesheets
      scssfoldersfiles: {
        files: ['app/assets/stylesheets/*/*.scss'],
        options: {
          livereload: true,
        }
      }

    },
  });

  // // These plugins provide necessary tasks.
  grunt.loadNpmTasks('grunt-contrib-nodeunit');
  grunt.loadNpmTasks('grunt-contrib-jshint');
  grunt.loadNpmTasks('grunt-contrib-watch');

  // Default task.
  grunt.registerTask('default', ['jshint', 'nodeunit']);

};
{% endhighlight %}

Finally, once your server's up and running, it's `$ grunt watch` in a new terminal window. 

### +++ References to people who know more than I do. 


