# cs50x-admin
instructions on how to manage content and systems for CS50x

TODO make this page nicer


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
4. On your local machine, go to your repo, pull in changes, then switch to a gh-pages branch (then pull again from the remote gh-pages branch, even though it should be the same, just to be safe)
  ```
  $ git pull origin master
  $ git checkout -b gh-pages
  $ git pull origin gh-pages
  ```
6. Next, run our `update-website` shell script which is just a wrapper around the couscous command, `couscous deploy`, which converts all the markdown to html and pushes changes to the remote gh-pages branch:
  ```
  ./update-website.sh
  ```
