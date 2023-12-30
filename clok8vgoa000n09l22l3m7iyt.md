---
title: "Git and github"
datePublished: Sat Nov 04 2023 16:14:10 GMT+0000 (Coordinated Universal Time)
cuid: clok8vgoa000n09l22l3m7iyt
slug: git-and-github

---

## What is Git?

**Git** is an **open-source distributed version control system**. It is designed to handle minor to major projects with high speed and efficiency. It is developed to co-ordinate the work among the developers. The version control allows us to track and work together with our team members at the same workspace.

Git is foundation of many services like **GitHub** and **GitLab**, but we can use Git without using any other Git services. Git can be used **privately** and **publicly**.

Git was created by **Linus Torvalds** in **2005** to develop Linux Kernel. It is also used as an important distributed version-control tool for **the DevOps**.

Git is easy to learn, and has fast performance. It is superior to other SCM tools like Subversion, CVS, Perforce, and ClearCase.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699114303973/92af1ca7-41ee-426a-9529-ffac1f867941.jpeg align="center")

* **Open Source**  
    Git is an **open-source tool**. It is released under the **GPL** (General Public License) license.
    
* **Scalable**  
    Git is **scalable**, which means when the number of users increases, the Git can easily handle such situations.
    
* **Distributed**  
    One of Git's great features is that it is **distributed**. Distributed means that instead of switching the project to another machine, we can create a "clone" of the entire repository. Also, instead of just having one central repository that you send changes to, every user has their own repository that contains the entire commit history of the project. We do not need to connect to the remote repository; the change is just stored on our local repository. If necessary, we can push these changes to a remote repository.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1699114343646/36cc3f6c-1b9e-4d69-9bab-eaa95c6abf24.jpeg align="center")
    
    * **Security**  
        Git is secure. It uses the **SHA1 (Secure Hash Function)** to name and identify objects within its repository. Files and commits are checked and retrieved by its checksum at the time of checkout. It stores its history in such a way that the ID of particular commits depends upon the complete development history leading up to that commit. Once it is published, one cannot make changes to its old version.
        
    * **Speed**  
        Git is very **fast**, so it can complete all the tasks in a while. Most of the git operations are done on the local repository, so it provides a **huge speed**. Also, a centralized version control system continually communicates with a server somewhere.  
        Performance tests conducted by Mozilla showed that it was **extremely fast compared to other VCSs**. Fetching version history from a locally stored repository is much faster than fetching it from the remote server. The **core part of Git** is **written in C**, which **ignores** runtime overheads associated with other high-level languages.  
        Git was developed to work on the Linux kernel; therefore, it is **capable** enough to **handle large** **repositories** effectively. From the beginning, **speed** and **performance** have been Git's primary goals.
        
    * **Supports non-linear development**  
        Git supports **seamless branching and merging**, which helps in visualizing and navigating a non-linear development. A branch in Git represents a single commit. We can construct the full branch structure with the help of its parental commit.
        
    * **Branching and Merging**  
        **Branching and merging** are the **great feature**s of Git, which makes it different from the other SCM tools. Git allows the **creation of multiple branches** without affecting each other. We can perform tasks like **creation**, **deletion**, and **merging** on branches, and these tasks take a few seconds only. Below are some features that can be achieved by branching:
        
        * We can **create a separate branch** for a new module of the project, commit and delete it whenever we want.
            
        * We can have a **production branch**, which always has what goes into production and can be merged for testing in the test branch.
            
        * We can create a **demo branch** for the experiment and check if it is working. We can also remove it if needed.
            
        * The core benefit of branching is if we want to push something to a remote repository, we do not have to push all of our branches. We can select a few of our branches, or all of them together.
            
    * **Data Assurance**  
        The Git data model ensures the **cryptographic integrity** of every unit of our project. It provides a **unique commit ID** to every commit through a **SHA algorithm**. We can **retrieve** and **update** the commit by commit ID. Most of the centralized version control systems do not provide such integrity by default.
        
    * **Staging Area**  
        The **Staging area** is also a **unique functionality** of Git. It can be considered as a **preview of our next commit**, moreover, an **intermediate area** where commits can be formatted and reviewed before completion. When you make a commit, Git takes changes that are in the staging area and make them as a new commit. We are allowed to add and remove changes from the staging area. The staging area can be considered as a place where Git stores the changes.  
        Although, Git doesn't have a dedicated staging directory where it can store some objects representing file changes (blobs). Instead of this, it uses a file called index.
        
    
    ![Features of Git](https://static.javatpoint.com/tutorial/git/images/features-of-git3.png align="left")
    
    * Another feature of Git that makes it apart from other SCM tools is that **it is possible to quickly stage some of our files and commit them without committing other modified files in our working directory.**
        
    * **Maintain the clean history**  
        Git facilitates with Git Rebase; It is one of the most helpful features of Git. It fetches the latest commits from the master branch and puts our code on top of that. Thus, it maintains a clean history of the project.