## Some Useful Linux Commands
For people who are familiar with basic linux commands can follow along easily, if not then go through this  
[maker's basic linux command tutorial for beginners](https://maker.pro/linux/tutorial/basic-linux-commands-for-beginners) all the basic commands from `pwd` to `locate`

After that there are a few commands and things that comes in handy which you should learn  
### AutoCompletion (Very very useful)
While command length gets longer and more complicated to remember linux helps to autocomplete them
```bash
$ roslau
## This is not a complete command but tap `TAB` once
$ roslaunch
## it gets completed  

## Also completes file and directory name
$ cd ~/catkin_ws/
## `TAB` twice you will be listed with the the files and folders in it
build/ devel/ src/ logs/  

$ cd ~/catkin_ws/bu
## `TAB`once now fills the most suitable one aswell
$ cd ~/catkin_ws/build
```

### Copy a directory
```bash
$ cp -r dir1/ dir2    ## copies the whole directory dir1 into a new directory dir2
$ cp -r dir1/ dir3/   ## copies the whole directory dir1 into into dir3/dir1
```
### Change Executable Permission (Necessary for python scripts)
Any file doesn't have access to run or execute or could be Read-only to change this we use `chmod`
```bash
$ chmod +x run.py     ## Adds execution rights to the file
$ chmod +a hist.log   ## Adds permission to the current user to edit the files
```  
### Some quick shortcuts  
- UP arrow (last command)
- CTRL+ALT+T (New Terminal window)
- CTRL+SHIFT+T (New Terminal Tab in same window)
- CTRL+R (Reverse search lookup any command you ran in the past :) has a limit though)
- CTRL+C (To stop execution of current command)
- CTRL+Z (If CTRL+C fails this forcefully stops it but it stills runs in the backrground)(To kill it forever use px|aux grep)
- CTRL+E (Takes cursor to end of line)
- CTRL+A (Cursor to start of line)
- CTRL+U (Clears line before cursor)
- CTRL+K (Clears line after cursor)
- CTRL+W (Clears upto start of last word before cursor)

### BashRC
So for things that you have to always run when you open your terminal you can save it in bashrc which helps you reuse them either as a Variable, Function or boot_scripts.
```bash
$ sudo gedit .bashrc  ## opens it and scroll it to the end
## Now add the following commands if not present

source /home/<your-username>/catkin_ws/devel/setup.bash
## This adds the source setup files for your local ros packages everytime your terminal opens

alias envon='function _envon(){ source ${PWD}/$1/bin/activate;};_envon'
## Macros to run this function, basically for a virtualenv rather than manually sourceing its activate just use `envon venv` or virtualenv name

alias cb='codeblocks'
## Another macros to run codeblocks with command `cb`
```
Add your favourites to it and make it the most productive experience

### Grep
Now after learning to locate a file with patterns with `locate` its time to get patterns inside files
```bash
$ grep -nr "QuadruppedController"   ## Finds where a particular word exists in the whole codebase
$ grep -nr . -e "*Quad*Controller"  ## Finds with pattern  
```

### Nano
I would highly recommend learning a cli based text editor either `nano`, `vim` or `vi` as it helps increasing efficiency of coding sessions. Its a gradually growth you may find it jarring but would be change completely how you code.

- **Vi** : - Follow this [tutorial by ryan](https://ryanstutorials.net/linuxtutorial/vi.php)
- **Nano**: - Much simpler editor [tutorial](https://www.howtogeek.com/howto/42980/the-beginners-guide-to-nano-the-linux-command-line-text-editor/)
