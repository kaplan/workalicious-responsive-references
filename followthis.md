# Responsive Grid Layouts

<a name="top"></a>


[version control for working](#working-version-control)   
[setup for styles](#setup-styles)  
[sass for responsive or mobile first](#responsive-mobile)  

- - -

**Note: started to consider using git.workalio.us for code research and development files and thinking about repo.worklici.us for client or site work.** 

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


<a name="basic-workflow"></a>
### workflow: make file modifications    


<a name="push-dev"></a>
### push to remote (ie: dev)  
