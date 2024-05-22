---
title: Version control. Git.

# Summary for listings and search engines
summary: In this post, I will tell you about the git version control system

# Link this post with a project
projects: []

# Date published
date: '2024-03-12T00:00:00Z'

# Date updated
lastmod: '2024-03-12T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin

tags:
  - Academic

categories:
  - Demo

---

## About the version control system

What is a "version control system" and why is it important? A version control system is a system that records changes to a file or set of files over time and allows you to revert to a specific version later. For file version control, this book will use the source code of the software as an example, although in fact you can use version control for almost any type of file.

If you are a graphic or web designer and want to save every version of an image or layout (most likely you will), a version control system (hereinafter referred to as VCS) is just what you need. It allows you to return files to the state they were in before the changes, return the project to its original state, see the changes, see who last changed something and caused the problem, who set the task and when, and much more. Using VCS also means in general that if you break something or lose files, you can safely fix everything. In addition to everything, you will get it all without any extra effort.

## Local version control systems

Many people use copying files to a separate directory as a version control method (perhaps even a time-stamped directory, if they are smart enough). This approach is very common because of its simplicity, but it is incredibly prone to errors. You can easily forget which directory you are in and accidentally change the wrong file or copy the wrong files that you wanted.

In order to solve this problem, programmers have long ago developed local VCS with a simple database that stores records of all changes in files, thereby monitoring revisions.

![Local version control](1.png)

One of the most popular VCS was the RCS system, which is still distributed with many computers today. RCS stores sets of patches (differences between files) on disk in a special format, using which it can recreate the state of each file at a given time.

## Centralized version control systems

Следующая серьёзная проблема, с которой сталкиваются люди, — это необходимость взаимодействовать с другими разработчиками. Для того, чтобы разобраться с ней, были разработаны централизованные системы контроля версий (Centralized Version Control System, далее CVCS). Такие системы, как CVS, Subversion и Perforce, используют единственный сервер, содержащий все версии файлов, и некоторое количество клиентов, которые получают файлы из этого централизованного хранилища. Применение CVCS являлось стандартом на протяжении многих лет.

![Centralized version control](2.png)

This approach has many advantages, especially over local VCS. For example, all project developers know to a certain extent what each of them is doing. Administrators have full control over who can do what, and it is much easier to administer CVCS than to operate local databases on each client.

Despite this, this approach also has serious disadvantages. The most obvious disadvantage is a single point of failure represented by a centralized server. If this server goes down for an hour, then during that time no one will be able to use version control to save the changes they are working on, and no one will be able to share these changes with other developers. If the hard disk on which the central database is stored is damaged, and timely backups are missing, you will lose everything — the entire history of the project, not counting the single repository snapshots that were saved on local developer machines. Local VCS suffer from the same problem: when the entire project history is stored in one place, you risk losing everything.

## Distributed version control systems

This is where Distributed Version Control Systems (hereinafter DVCS) come into play. In DVCS (such as Git, Mercurial, Bazaar, or Darcs), clients don't just download a snapshot of all files (the state of the files at a certain point in time) — they completely copy the repository. In this case, if one of the servers through which the developers exchanged data dies, any client repository can be copied to another server to continue working. Each copy of the repository is a complete backup of all data.

![Distributed version control](3.png)

Moreover, many DVCS can simultaneously interact with several remote repositories, so you can work with different groups of people using different approaches at the same time within the same project. This allows you to apply several approaches to development at once, for example, hierarchical models, which is completely impossible in centralized systems.
