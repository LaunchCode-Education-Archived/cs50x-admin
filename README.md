# CS50x Admin

* [Setting up a repo for a hub city](./#Setting up a repo for a hub city)
* [Editing a Specific Repo](./#Editing a Specific Repo)

***

### Setting up a repo for a hub city

Let's say LaunchCode is expanding to Pleasantville, USA, and we want to host a CS50x class there.

Follow these steps to create the repository and course website for your new hub city:

##### Create a new Repository

1. Create this repo on github: LaunchCodeEducation/cs50x-pleasantville
2. In the new repo, add the following teams as contributors with `write` access:
    * CS50x Core
    * launch-ci
    * make a new team called CS50x Pleasantville, and invite the lead instructor to join

##### "Fork" the generic Repository

We want to fork cs50x-live, but Github does not allow you to fork your own repo. So we will fake it (<a href="http://bitdrift.com/post/4534738938/fork-your-own-project-on-github" target="_blank">see here</a> for explanation)

1. On your local machine, clone the cs50x-live repo, but give it a different name:

  ```
  git clone https://github.com/LaunchCodeEducation/cs50x-live.git cs50x-pleasantville
  ```
  
2. Next, edit your Git config file and replace the origin URL with your new URL:

  ```
  $ cd cs50x-pleasantville
  $ vim .git/config
    [remote "origin"]
      fetch = +refs/heads/*:refs/remotes/origin/*
      url = https://www.github.com/LaunchCodeEducation/cs50x-pleasantville.git # replace cs50x-live with cs50x-pleasantville
  ```
  
3. Add cs50x-live as the upstream source:

  ```
  $ git remote add upstream https://github.com/LaunchCodeEducation/cs50x-live.git
  ```
  
4. Push the new repo to GitHub:

  ```
  $ git push -u origin master
  ```
  
##### Set up GitHub Pages and Couscous

Next we will generate the course website using Github Pages, and a program called couscous which automatically converts your README markdown files into HTML.

1. Go back to cs50x-pleasantville on GitHub
1. Edit the `couscous.yml` file and fill in any placeholders. This is how we customize the course website to reflect our specific city by configuring things like links on the sidebar menu, and giving couscous a reference to our repository.
2. Still on GitHub, create a new branch called `gh-pages`
3. Make `gh-pages` the default branch instead of `master`. Do this in Settings / Branches.
4. Over on the generic cs50x-live repo, edit the <a href="https://github.com/LaunchCodeEducation/cs50x-live/blob/master/scripts/update-children.sh" target="_blank">update-children.sh</a> script to include our new repo as a child. Edit this line:
   ```
   CHILDREN=("cs50x-stlouis" "cs50x-kansascity" "cs50x-maryville" "cs50x-pleasantville") # add pleasantville here like this
   ```
so that the `CHILDREN` array now contains a string with the name of our new repo.
5. Over on our <a href="https://travis-ci.org/LaunchCodeEducation/cs50x-live/builds/" target="_blank">Travis builds feed</a>, we should now see that `cs50x-live master` is now running its build script.
6. If all goes well with the build script (green text on Travis), your class website shoudl now be up live! (Though it may take a few minutes to show up). The website will be at education.launchcode.org/cs50x-pleasantville

***

### Editing a Specific Repo

Let's say we want to add custom content to the website for a particular hub city, such as Pleasantville. For example, if we just created the repo following the steps above, we will still need to add in specific dates to the calendar page on the Pleasantville site.

Let's walk through how to do that:

There are two ways to do this: 
* editing files directly on GitHub
* editing files locally

We'll address each separately.

##### Editing Files Driectly on GitHub
1. Go to the repo, and edit any files you want to change
2. On your local machine, go to the cs50x-pleasantville repo
3. Locally, make sure you are on the `gh-pages` branch (create it if you haven't already done so using `-b`):
   ```
   $ git checkout -b gh-pages
   ```
4. Pull in your changes from the remote:
   ```
   $ git pull origin gh-pages
   ```
5. Finally, run our `update-website` shell script which is just a wrapper around the couscous command, `couscous deploy`, which converts all the markdown to html and pushes changes to the remote gh-pages branch:
   ```
   ./update-website.sh
   ```
   
##### Editing Files Locally
1. On your local machine, go to the cs50x-pleasantville repo
2. Locally, make sure you are on the `gh-pages` branch (create it if you haven't already done so using `-b`):
   ```
   $ git checkout -b gh-pages
   ```
3. Pull in any changes from the remote:
   ```
   $ git pull origin gh-pages
   ```
4. Check out a new branch to make your changes:
   ```
   $ git checkout -b develop
   ```
5. Make any changes you want to make
6. Merge the changes back into the `gh-pages` branch:
   ```
   $ git checkout gh-pages 
   $ git merge develop
   ```
7. Finally, run our `update-website` shell script which is just a wrapper around the couscous command, `couscous deploy`, which converts all the markdown to html and pushes changes to the remote gh-pages branch:
   ```
   ./update-website.sh
   ```
