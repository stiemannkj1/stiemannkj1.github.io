---
layout: post
title: Version Control
permalink: /version-control/
excerpt_separator: <!--end-excerpt-->
tags: [version-control, revision-control, git, computer-science, programming, software, software-engineering, coding]
comments_issue_number: 10
---

<img alt="Version Control the Missing Link" src="/images/version-control/version-control.png" />

## Missing Links in Computer Science Curricula: Part 1.

Version Control is an integral part of software development. Allowing developers to track changes to software, version control systems give programmers the power to collaborate, discover erroneous changes, and compare versions of software. When rightly used, version control (also called source control) empowers developers to make and share complex changes without fear of losing previous implementations. Developers who use revision control can always look back on and---if necessary---revert to previous versions of the software. Unfortunately, I received two-and-a-half years of Computer Science education without hearing about version control once.<sup>1</sup>

<!--end-excerpt-->

Source control management should be taught in CS 101 or 102. As soon as students have completed three or four solo programming projects in a non-toy language, they should learn the basics of revision control tools like <a target="_blank" href="https://en.wikipedia.org/wiki/Git_(software)">`git`</a>, <a target="_blank" href="https://en.wikipedia.org/wiki/Apache_Subversion">Subversion</a> and/or <a target="_blank" href="https://en.wikipedia.org/wiki/Mercurial">Mercurial</a>. Not only would learning revision control better prepare them for a career in Software Engineering, but it would also help them to manage school projects. Understanding `git` in school would make group projects much easier for students. If you are reading this as a student or recent graduate who was not taught or has not learned about revision control, allow me to provide a brief introduction and some helpful resources for further learning.

Since I have used `git` every day for the past three years, I will use `git` examples to introduce you to version control. <a target="_blank" href="https://git-scm.com/book/en/v2/Getting-Started-Installing-Git">`git` is also free</a> and <a target="_blank" href="https://www.google.com/trends/explore#q=git%2C%20%2Fm%2F08441_%2C%20%2Fm%2F012ct9&cmpt=q&tz=Etc%2FGMT%2B4">extremely popular</a>, so it has plenty of documentation and helpful Q&As on StackOverflow. However, the point of this tutorial is to teach you about the benefits and basic usage of version control systems. Although <a target="_blank" href="https://github.com/dotnetchris/Git-Hg-Rosetta-Stone/wiki">the commands and functionality may differ</a>, the overall lessons about version control will transfer to other programs like Mercurial or Subversion. My examples will be command line examples, but if you want to use a `git`-client like <a target="_blank" href="http://www.syntevo.com/smartgit/">SmartGit</a>,<sup>2</sup> I doubt that it would be difficult for you to follow along.<sup>3</sup>

### Part 1: Creating the Repository

Since the purpose of version control is to track the history of a project, the first thing you should do is create some files or a program which you want to track. I have created an example program called <a target="_blank" href="https://github.com/stiemannkj1/example-git-project#get-day-program">`get-day`</a>, which outputs the day of the week for an input date string, to serve this purpose, so <a target="_blank" href="https://github.com/stiemannkj1/example-git-project/archive/part1.zip">download and unzip the example project</a>. Then open up a terminal on <a target="_blank" href="https://discussions.apple.com/thread/3223989?tstart=0">OSX</a> or <a target="_blank" href="http://askubuntu.com/questions/38162/what-is-a-terminal-and-how-do-i-open-and-use-it">Linux</a> or <a target="_blank" href="http://www.howtogeek.com/235101/10-ways-to-open-the-command-prompt-in-windows-10/">**`cmd.exe`** on Windows</a>. Next, navigate to the unzipped **`example-git-project-part1`** (if you simply downloaded it and unzipped it, it will be under your **`Downloads`** folder):

```
# You may need to navigate to the Downloads folder first:
# On OSX/Linux: `cd ~/Downloads`
# On Windows: `cd %HOMEPATH%/Downloads`
cd example-git-project-part1
```

Now that you have some files you want to track, you can create a `git` repository in this
 directory:

```
git init
```

Next, you need to tell `git` which files it should track:

```
git add .
```

The "`.`" tells `git` to track all the files in this directory, but you could also list files individually as arguments to `git add`. Next, commit these files to `git`'s history:

```
git commit -m "Initial commit."
```

Now `git` has a historical reference point for these files. As long as you do not rewrite the history of this project, you will always be able to obtain the original files and rebuild the original binary represented by those files.

Next I'll show you how to add a feature to the example `get-day` program in order to demonstrate branching, `diff`ing, merging, and more.

### Part 2: Adding a Feature

In order to let users specify a custom date pattern for the `get-day` program, you are going to add another command-line option called `--pattern`. When tracking new features with `git`, it is best to branch off from `master` and do your work on a separate branch:

```
git checkout -b add-pattern-option
```

Now that you have checked out a new branch (called `add-pattern-option`), you can make changes without affecting the `master` branch. This means that you can make all your mistakes on the `add-pattern-option` branch and add the code to `master` only once it is completely ready. If you decide you do not like the changes, you can even delete the branch and `master` will be unaffected.

Since you have checked out your branch, you need to add your new feature. For the sake of this example, you can copy the new contents of **`GetDayProgram.java`** from <a target="_blank" href="https://raw.githubusercontent.com/stiemannkj1/example-git-project/part2/src/main/java/io/github/stiemannkj1/example/git/project/GetDayProgram.java">here</a> and overwrite the original contents in your IDE or editor.

Before you do anything else, you should get a high-level view of what has changed in your repository:

```
git status
```

`git status` displays the following information:

```
On branch add-pattern-option
Your branch is up-to-date with 'origin/add-pattern-option'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   src/main/java/io/github/stiemannkj1/example/git/project/GetDayProgram.java

no changes added to commit (use "git add" and/or "git commit -a")
```

So `git` knows that you have changed the **`GetDayProgram.java`** file. You can also view more detailed information about changes in the file:

```
git diff
```

Here is a snippet of the `git diff` output:

```
         try {
-            LocalDate date = LocalDate.parse(dateString);
+            LocalDate date = LocalDate.parse(dateString, DateTimeFormatter.ofPattern(datePattern));
             System.out.println(date.getDayOfWeek());
         } catch (DateTimeParseException e) {
-            System.err.println("Error: date string must be of the format: yyyy-MM-dd.");
+            System.err.println("Error: date string must be of the format: " + datePattern);
+        } catch (IllegalArgumentException e) {
+            System.err.println("Error: invalid date pattern. " + e.getMessage());
+        } finally {
             System.exit(1);
         }
```

`git diff` shows which lines have been removed, added or remain unchanged. Removed lines start with "`-`". Added lines start with "`+`". Unchanged lines start with neither. For example, in your changes, you have removed the following line:

```
-            LocalDate date = LocalDate.parse(dateString);
```

And added the following line in its place:

```
+            LocalDate date = LocalDate.parse(dateString, DateTimeFormatter.ofPattern(datePattern));
```

The added line shows how you have added the ability to customized the date pattern. If you want to learn more about how to read `git diff`'s output (also called a unified `diff`), check out <a target="_blank" href="http://stackoverflow.com/questions/2529441/how-to-read-the-output-from-git-diff">this Q&A on StackOverflow</a>.

Now that you have made your changes and reviewed them via `git diff`, you can commit them:

```
git commit -a -m "Added --pattern command line option to make the date pattern configurable."
```

Since you have committed the changes, `git status` will no longer show any unchanged files.

Now you can check how different your current branch, `add-pattern-option`, is from the `master` branch which only contains the original files:

```
git diff master
```

Since `master` is still at the initial commit, the `git diff` output will be very similar to the `git diff` output above. None of your changes so far have affected the `master` branch, so now would be a good time to test the new feature and ensure that it works. Once you have done (or skipped) that, you are ready to add your new `--pattern` option feature to the `master` branch. First you must go back to the `master` branch:

```
git checkout master
```

Now you can merge your `add-pattern-option` changes into `master`:

```
git merge add-pattern-option
```

Since you merged your feature branch into `master`, `master` contains the new feature and its history.

To see everything what you have accomplished, you can check the commit log:

```
git log
```

`git log` displays all the commit hashes<sup>4</sup> with their authors, dates, and messages. Here is the output of `git log` on my example repo:

```
commit b6c508e4379644731bc0210af309f1be3ce9ef47
Author: Kyle J. Stiemann <kyle.j.stiemann@example.com>
Date:   Mon Jul 11 21:39:59 2016 -0400

    Added --pattern command line option to make the date pattern configurable.

commit 81a59d9429e2f2611ed9196f7163876796bd48c5
Author: Kyle J. Stiemann <kyle.j.stiemann@example.com>
Date:   Sat Jul 9 14:21:04 2016 -0400

    Initial commit.
```

As you can see, `git` has two commits worth of history: one for the initial `get-day` program and one for the additional `--pattern` feature. If you ever need to review, access, or rebuild the `get-day` program without the `--pattern` feature, you can simply copy the hash of the initial commit and do a `git checkout` on that hash. For more information on `git log`, check out <a target="_blank" href="https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History">*2.3 Git Basics - Viewing the Commit History*</a>.

### Conclusion

Congratulations! You made it to the end. Hopefully, my tutorial helped you get started with version control and `git`. Please let me know in the comments if anything was confusing or vague or especially helpful. Version control is extremely powerful, and these examples have barely demonstrated the simplest use cases. I highly recommend playing around with `git` (or your version control system of choice) in my example project<sup>5</sup> or your own projects. In my humble opinion, the best way to learn about version control is to use version control.

### Further learning:

- Other `git` tutorials:

    - <a target="_blank" href="https://git-scm.com/book/en/v2">https://git-scm.com/book/en/v2</a>
    - <a target="_blank" href="https://try.github.io/levels/1/challenges/1">https://try.github.io/levels/1/challenges/1</a>

- Mercurial tutorials:

    - <a target="_blank" href="http://hginit.com/01.html">http://hginit.com/01.html</a>
    - <a target="_blank" href="https://www.mercurial-scm.org/quickstart">https://www.mercurial-scm.org/quickstart</a>

- Subversion tutorials:

    - <a target="_blank" href="https://subversion.apache.org/quick-start">https://subversion.apache.org/quick-start</a>
    - <a target="_blank" href="http://svnbook.red-bean.com/en/1.7/svn.intro.quickstart.html">http://svnbook.red-bean.com/en/1.7/svn.intro.quickstart.html</a>

- Linus Torvalds (creator of `git` and Linux) talks about `git`'s design: <a target="_blank" href="https://www.youtube.com/watch?v=4XpnKHJAok8">https://www.youtube.com/watch?v=4XpnKHJAok8</a>. This talk is **extremely** opinionated and biased towards `git`, but I found it interesting and somewhat helpful for understanding version control (especially distributed version control).

- <a target="_blank" href="https://github.com">github.com</a>

    - Free unlimited public `git` and Subversion repositories.

- <a target="_blank" href="https://bitbucket.org">bitbucket.org</a>

    - Free unlimited private `git` and Mercurial repositories.

---

1. I did a quick survey of the CS curricula for three colleges and three universities. Five* of the six schools did not mention version control in their CS curricula. Sources:

    - <a target="_blank" href="http://catalog.ccm.edu/credit/areasofstudy/computerscience/#courseinventory">County College of Morris (CCM) CS course descriptions</a>
    - Grove City College (GCC) <a target="_blank" href="http://www2.gcc.edu/registrar/advising/Status_Sheets/2015/Computer%20Science-2019.pdf">CS Schedule</a> and <a target="_blank" href="http://www.gcc.edu/Documents/Academics/Bulletin/2016-17/2016-17-Catalog.pdf">Catalogue</a>
    - <a target="_blank" href="https://www.seminolestate.edu/computers/courses">Seminole State College (SSC) CS course descriptions</a>
    - <a target="_blank" href="http://www.shu.edu/academics/upload/Undegraduate_Catalogue_Seton_Hall_University_2015-16.pdf#page=394">Seton Hall University (SHU) Course Catalogue</a>
    - <a target="_blank" href="http://www.tesu.edu/documents/catalog-ug.pdf">Thomas Edison State University (TESU) Course Catalogue</a>

    The University of Central Florida (UCF) mentions version control in the course description for "Software Development I" (CEN 3024). However, CEN 3024 is an elective, and it has Object Oriented Programming (COP 3330) as a prerequisite, which has Introduction to Programming with C (COP 3223C) as a prerequisite. So students are still not guaranteed to learn version control at UCF. Sources:

    -  UCF <a target="_blank" href="http://catalog.ucf.edu/content/documents/ucf_courses_and_descriptions.pdf">Course Catalogue</a>, <a target="_blank" href="http://www.cs.ucf.edu/CS/requirements.php">CS requirements</a>, and <a target="_blank" href="http://www.cs.ucf.edu/files/CS/CS-Brochure-10162015.pdf">unintelligible CS brochure</a>.

    I'm not convinced that version control needs more than a few class periods worth of instruction, and I want to give the schools that I didn't attend the benefit of the doubt in case they teach a little version control but don't feel the need to mention it in the course description. However, I'm still surprised that version control is only mentioned in only one of these schools' catalogues. And I'm flabbergasted that a software engineering tool **that I use every day** was not taught (or even mentioned) at Grove City College (where I spent two years) or in Thomas Edison State University's "Software Engineering" (CIS-351-OL) course.

    \* Four of those five schools were the schools I attended. Short version of my college career: started early college at CCM, spent two decent years at GCC but ran out of money, did three worthless classes at SSC, and finished at TESU.

2. <a target="_blank" href="http://www.syntevo.com/blog/?p=4037">SmartGit is free for non-commercial use.</a>
3. I actually learned `git` via SmartGit. Afterwards I moved to the command-line for various reasons, but SmartGit is more user-friendly and accessible. It is a great way to get started.
4. A `git` commit hash is a unique Secure Hash Algorithm 1 (SHA-1) identifier for a commit.
5. You can obtain the complete project like so:

        git clone https://github.com/stiemannkj1/example-git-project.git
