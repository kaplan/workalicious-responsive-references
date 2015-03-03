# Responsive Grid Layouts



<a name="top"></a>

[version control for working](#working-version-control)   
[directory setup](#directory-setup)  
[setup for styles](#setup-styles)   
[sass for responsive or mobile first](#responsive-mobile)   
[rake for sass/coffee watch](#rake-watching)  
[ba-debug for logging](#debug-js)  
[gsap and jquery](#gsap-jquery)  

- - -

**Note: started to consider using git.workalio.us for code research and development files and thinking about repo.worklicio.us for client or site work.** 

<a name="working-version-control"></a>
## git for development files
  * [start a working directory locally for development](#start-working)
  * [git init in that new working directory](#git-init-working)
  * [create a bare git repo for the working directory](#create-bare-repo-locally)  
  * [upload to git.workalicio.us using `scp`](#upload-local-bare-repo)  
  * [`ssh` in and update hooks and create a post-update](#create-hooks)  
  * [add a git remote of repo on workalicio.us to the working directory](#add-remote-for-dev)  
  * [workflow: test by make changes locally, `git add` and `git commit`](#basic-workflow)    
  * [push to the remote repository](#push-dev)

[Back to Top](#top)  

<a name="start-working"></a>
### git init, add and commit in working directory
For Code Research directories I was thinking I didn't need an enclosing html or public directory like I would setup with site or domain, but we'll see...  

    ~/Code/code_resources/responsive-grid-layout $ git init
    Initialized empty Git repository in /Users/dave/Code/code_resources/responsive-grid-layout/.git/
    ~/Code/code_resources/responsive-grid-layout $ git add .
    ~/Code/code_resources/responsive-grid-layout $ git commit -m "initial commit."
    [master (root-commit) f2363b6] initial commit.
     2 files changed, 19 insertions(+)
     create mode 100644 followthis.md
     create mode 100644 readme.md


<a name="create-bare-repo-locally"></a>
### create bare git repo
There is a single folder in the `~/Code` directory for `--bare` repositories. For sites you'll use a `--bare` repository in the `~/Sites` directory root.  

    ~/Code $ cd __bare-git.workalicio.us-git/
    ~/Code/__bare-git.workalicio.us-git $ git clone --bare ~/Code/code_resources/responsive-grid-layout/ yo.responsive-grids.dev.git
    Cloning into bare repository 'yo.responsive-grids.dev.git'...
    done.

Then make the git-daemon-export-ok file.

    ~/Code/__bare-git.workalicio.us-git $ cd yo.responsive-grids.dev.git/
    ~/Code/__bare-git.workalicio.us-git/yo.responsive-grids.dev.git (master)$ touch git-daemon-export-ok

<a name="upload-local-bare-repo"></a>
### upload the bare repo to MT from the __bare directory    
From within the directory of local bare repositories `~/Code/__bare-git.workalicio.us-git` , upload the new bare repository to `git.workalicio.us`  

**Note: example code for project name and .git**

    # it looks like this, note the same name for the repo on the server
    $ scp -r baregitrepo.git/ workalicio.us@s103841.gridserver.com:domains/git.workalicio.us/html/baregitrepo.git  
    # instead of s103..., you should also be able to use 
    $ scp -r baregitrepo.git/ workalicio.us@workalicio.us:domains/git.workalicio.us/html/baregitrepo.git

    ~/Code/__bare-git.workalicio.us-git $ scp -r yo.responsive-grids.dev.git/ workalicio.us@workalicio.us:domains/git.workalicio.us/html/yo.responsive-grids.dev.git
    config                                    100%  192     0.2KB/s   00:00    
    description                               100%   73     0.1KB/s   00:00    
    git-daemon-export-ok                      100%    0     0.0KB/s   00:00    
    HEAD                                      100%   23     0.0KB/s   00:00    
    applypatch-msg.sample                     100%  452     0.4KB/s   00:00    
    commit-msg.sample                         100%  896     0.9KB/s   00:00    
    post-update.sample                        100%  189     0.2KB/s   00:00    
    pre-applypatch.sample                     100%  398     0.4KB/s   00:00    
    pre-commit.sample                         100% 1642     1.6KB/s   00:00    
    pre-push.sample                           100% 1348     1.3KB/s   00:00    
    pre-rebase.sample                         100% 4951     4.8KB/s   00:00    
    prepare-commit-msg.sample                 100% 1239     1.2KB/s   00:00    
    update.sample                             100% 3611     3.5KB/s   00:00    
    exclude                                   100%  240     0.2KB/s   00:00    
    7ff9535c6762833e75761cbee13d172e91b4a6    100%  130     0.1KB/s   00:00    
    1c332d18909590dfec5c0384eeb4b8af51e93d    100%   89     0.1KB/s   00:00    
    ffeb9ffb59ac09a5e34b54ecd3c036d01fb05d    100%  319     0.3KB/s   00:00    
    363b6404329e345ae112ddfbc80ec6808776f7    100%  135     0.1KB/s   00:00    
    packed-refs  

<a name="create-hooks"></a>
### ssh into the workalicio.us repository and to complete the setup.    

    ~/Code/code_resources/responsive-grid-layout (master)$ ssh workalicio.us@workalicio.us
    Linux n29 3.14.27mtv15 #1 SMP Wed Dec 17 20:20:27 PST 2014 x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    workalicio.us@n29:~$ cd domains/git.workalicio.us/html/
    workalicio.us@n29:~/domains/git.workalicio.us/html$ ls
    dv_work.git  grids.git  sass-scss.git  yo.responsive-grids.dev.git
    workalicio.us@n29:~/domains/git.workalicio.us/html$ cd yo.responsive-grids.dev.git/
    workalicio.us@n29:~/domains/git.workalicio.us/html/yo.responsive-grids.dev.git$ git --bare update-server-info
    workalicio.us@n29:~/domains/git.workalicio.us/html/yo.responsive-grids.dev.git$ cd hooks/
    workalicio.us@n29:~/domains/git.workalicio.us/html/yo.responsive-grids.dev.git/hooks$ mv post-update.sample post-update
    workalicio.us@n29:~/domains/git.workalicio.us/html/yo.responsive-grids.dev.git/hooks$ chmod a+x post-update 
    workalicio.us@n29:~/domains/git.workalicio.us/html/yo.responsive-grids.dev.git/hooks$ 

- - -

<a name="add-remote-for-dev"></a>
### add workalicio.us repo as a remote  
    
    ~/Code/code_resources/responsive-grid-layout (master)$ git remote -v
    ~/Code/code_resources/responsive-grid-layout (master)$ git remote add dev ssh://workalicio.us@workalicio.us/home/103841/domains/git.workalicio.us/html/yo.responsive-grids.dev.git
    ~/Code/code_resources/responsive-grid-layout (master)$ git remote -v
    dev ssh://workalicio.us@workalicio.us/home/103841/domains/git.workalicio.us/html/yo.responsive-grids.dev.git (fetch)
    dev ssh://workalicio.us@workalicio.us/home/103841/domains/git.workalicio.us/html/yo.responsive-grids.dev.git (push)

<a name="basic-workflow"></a>
### workflow: make file modifications    

    ~/Code/code_resources/responsive-grid-layout (master)$ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   followthis.md

    no changes added to commit (use "git add" and/or "git commit -a")
    ~/Code/code_resources/responsive-grid-layout (master)$ git add .
    ~/Code/code_resources/responsive-grid-layout (master)$ git commit -m "adding git dev setup to followthis markdown file."
    [master 0deade7] adding git dev setup to followthis markdown file.
     1 file changed, 113 insertions(+), 1 deletion(-)

<a name="push-dev"></a>
### push to remote (ie: dev)  

    ~/Code/code_resources/responsive-grid-layout (master)$ git push dev master
    Counting objects: 3, done.
    Delta compression using up to 8 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 2.29 KiB | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To ssh://workalicio.us@workalicio.us/home/103841/domains/git.workalicio.us/html/yo.responsive-grids.dev.git
       f2363b6..0deade7  master -> master

[Return to Nav: git for development files](#working-version-control)  

- - - 

[Back to Top](#top)  

<a name="directory-setup"></a>
## project directory setup  

The index will list all the example html files, Sass and CoffeeScript will be compiled using a Rakefile. You could use a Cakefile for just CoffeeScript, or the `watch` bash_profile command for Sass, but because this project is using both Sass and CoffeeScript it's a good opportunity for `rake`. 

    ~/Code/code_resources/responsive-grid-layout (master)$ ls
    followthis.md   index.html  readme.md
    responsive-grid-layout (master)$ mkdir stylesheets && touch stylesheets/main.scss
    responsive-grid-layout (master)$ mkdir stylesheets/sass
    responsive-grid-layout (master)$ mkdir stylesheets/sass/partials
    responsive-grid-layout (master)$ mkdir stylesheets/sass/vendor
    responsive-grid-layout (master)$ mkdir js
    responsive-grid-layout (master)$ mkdir js/vendor
    responsive-grid-layout (master)$ mkdir js/vendor/gsap
    responsive-grid-layout (master)$ mkdir js/vendor/jquery
    responsive-grid-layout (master)$ mkdir src
    responsive-grid-layout (master)$ touch src/site.coffee
    responsive-grid-layout (master)$ touch Rakefile
    responsive-grid-layout (master)$ touch Cakefile


[Back to Top](#top) 

<a name="setup-styles"></a>
## setup for style  

### sass and bourbon  
If you have not installed sass and bourbon:  

    $ gem install sass
    $ gem install bourbon

Make sure style directory and `main.scss` files setup. _be sure the main.scss or any .scss are in the `sass` directory._   
Install Bourbon into your project’s `stylesheets/sass` directory by generating the bourbon folder:

    responsive-grid-layout (master)$ cd stylesheets/sass/
    responsive-grid-layout/stylesheets/sass (master)$ bourbon install
    Bourbon files installed to bourbon/

- - -

### base and normalize  
Grab a copy of `_normalize.scss` and add it to `stylesheets/partials`. I did a git clone into the code_resources of [normalize from github](https://github.com/necolas/normalize.css/). Then I duplicated the original `.css` and renamed as a partial `_normalize.scss`. (.scss files can be plain .css that's just sass-friendly)
    
    # something like this:  
    $ mv /normalize.css stylesheets/sass/partials/_normalize.scss


Make a `_base.scss`, `_colors.scss` and `_typography.scss` within the `stylesheets/partials`

    responsive-grid-layout (master)$ touch stylesheets/sass/partials/_base.scss
    responsive-grid-layout (master)$ touch stylesheets/sass/partials/_colors.scss
    responsive-grid-layout (master)$ touch stylesheets/sass/partials/_typography.scss

- - - 

<a name="rake-watching"></a>
## rake for sass/coffee watching  
This project uses a Rakefile to start the commands for watching changes to the .scss and coffee files in their respective directories. The original technique comes from [Nick Q's post](http://quaran.to/blog/2013/01/09/use-jekyll-scss-coffeescript-without-plugins/). In the terminal you can run `rake -T` and see the `desc`. Then just type `rake` and the `:default` task will run with a `spawn()` for `sass ... ` and `coffee ... `. The convience with the `rake` task is that it fires up both scss and coffee, which you could do indvidually. 

**The other watch and compile techniques for scss** would be to run `watch` (an alias setup in the .bash_profile) from the stylesheet folder. For coffee you can use the Cakefile, which has a bunch of compling options depending on what you want for output, but it's the same approach. 
    
    # ~/.bash_profile
    alias watch='sass --watch stylesheets/sass:stylesheets'    


**The other watch and compile techniques for coffeescript** are a bit more complicated because you have more options. Running `cake` in the terminal from the working directory will show you the tasks.

    To see a list of all tasks/options, run "cake"
    ~/Code/code_resources/responsive-grid-layout (master)$ cake
    Cakefile defines the following tasks:

    cake build                # Build lib/ from src/ with optional directory
    cake watch                # Watch src/ for changes
    cake watch:options        # Watch src/ directory for changes and now with options for output dir
    cake watch:bare           # Watch src/ directory for changes
    cake open                 # Open index.html
    cake task:withDefaults    # Description of task

      -o, --output       output directory
      -e, --environment  set the environment for `task:withDefaults` 

**To use the Cakefile for `watch:options`** where you want to specify the output directory you can do this:  

    $ cake -o js watch:options      

- - - 

[Back to Top](#top) 

<a name="debug-js"></a>
## ba-debug for logging and debugging javascript 

> JavaScript Debug: A simple wrapper for console.log
This code provides a simple wrapper for the console's logging methods, and was created to allow a very easy-to-use, cross-browser logging solution, without requiring excessive or unwieldy object detection. If a console object is not detected, all logged messages will be stored internally until a logging callback is added. If a console object is detected, but doesn't have any of the debug, info, warn, and error logging methods, log will be used in their place. For convenience, some of the less common console methods will be passed through to the console object if they are detected, otherwise they will simply fail gracefully.

[Debug's page](http://benalman.com/projects/javascript-debug-console-log/)  
[Debug's full documentation](http://benalman.com/code/projects/javascript-debug/docs/files/ba-debug-js.html)  
[Debug on Github](https://github.com/cowboy/javascript-debug) 

    // Instead of this:
    window.console && console.log
    && console.log( this, 'that', { the: 'other' } );
     
    // Just do this:
    debug.log( this, 'that', { the: 'other' } );

- - - 

<a name="gsap-jquery"></a>
## add the awesome gsap and jquery frameworks  
In the the `js/vendor/` directory add `gsap` and `jquery` from your machine's code source files.  

If you download jQuery and Modernizr and GSAP you'd put those in `js/vendor`. **Note if you copy over or download vendor js files you may need to modify the permissions, if you get a 403 in the browser** with either a 644 or 755. Still need to learn more about `chmod` and setting permissions in general, checkout the Linux books you bought.

    js/vendor/jquery $ ls -al
    drwxr-xr-x  5 dave  staff     170 Feb 20 12:54 .
    drwxr-xr-x  7 dave  staff     238 Feb 20 13:38 ..
    -rw-r--r--@ 1 dave  staff  284184 Feb 20 12:53 jquery-1.11.2.js
    -rw-r-----@ 1 dave  staff   95931 Feb 20 12:53 jquery-1.11.2.min.js
    -rw-r-----@ 1 dave  staff  142010 Feb 20 12:53 jquery-1.11.2.min.map

To updated permissions with `chmod`:  

    $ chmod -R 755 vendor/jquery/
    $ chmod -R 755 vendor/gsap/

**Make sure you've added the `<script>`s to the bottom of the page.** 

    <!-- Ben Alman's JavaScript-Debug -->
    <script src="js/vendor/ba-debug.js"></script>
    
    <!-- jQuery -->
    <script src="js/vendor/jquery/jquery-1.11.2.js"></script>

    <!-- GSAP -->
    <script src="js/vendor/gsap/TweenMax.js"></script>

    <!-- Compiled JS -->
    <script src="js/site.js"></script>



### Mixins    
Another way to write a mixin in SASS:  

    @mixin red-text
        color: #ff0000
    
    # could be written as:
    =red-text
        color: #ff0000

Then add `+red-text` to your selectors