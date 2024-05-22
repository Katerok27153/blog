---
title: Comparative analysis of file systems (FAT, NTFS, ext4)
summary: Computer Architecture
tags:
  - Deep Learning
date: '2024-05-9T00:00:00Z'

# Optional external URL for project (replaces project detail page).
external_link: ''

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
---

## Introduction

### What is a file system?

It is rare for every user of computer electronic devices, but they have to deal with such a concept as "choosing a file system". Most often this happens when it is necessary to format external drives (flash drives, microSD), install operating systems, restore data to
problematic media, including hard drives. Windows users are asked to choose the file system type, FAT32 or NTFS, and the formatting method (fast/deep). Additionally, you can set the cluster size. When using Linux and macOS, the names of the file systems may
differ. A logical question arises: what is a file system and what is its
purpose? The main function of the VZU is the storage of information. Information on external sites
The media is stored in the form of files. The file system handles the ordering and processing of these files. File system (English file system) — the order that determines the method
organizing, storing and naming data in external memory, and providing a user-friendly interface when working with such data. In simple words, a file system is a system for storing files and organizing directories. The file system determines how files will be encoded, stored on disk, and read by the computer. And, if the disk is an array of clusters, then the file system is an instruction for filling these clusters with information. Each operating system has its own type
of file organization, that is, its own file system.

### What are file, clusters and directory?

The objects of the file system are files and directories. From the user's point of view, a file is a unit of storage of logically related information; text, graphics, audio, video. From the point of view of organizing data storage on disk, a file is a chain of interconnected clusters. Each
Depending on the size, the file receives one or more clusters for storing its data, which can be located in a row, one after another, or scattered throughout the disk. Usually a cluster weighs a few bytes, and how much exactly depends on the size of the disk and on the settings. When a person sets up a file system, they can also choose the cluster size:

- if you make the cluster smaller than recommended,
more files will fit on the drive. After all, then there will be no situations when the cluster
is actually only partially filled — the entire space will be
involved;
- if you increase the cluster size, file access will be faster. The files
will be divided into fewer parts. The computer will have
to access fewer clusters — this will increase the speed.

The file system manages clusters, how they will be recorded and
stored, and how connections will be organized between them. It also divides and
distributes files into clusters, manages writing and reading.
A directory (directory, directory, folder) is an object in the file system
that simplifies the organization of files. The directory is implemented as a special file, where
information about other files and directories on the storage medium is recorded —
names, physical location, attributes, etc. It is customary to say that
a directory contains files or other directories, although in reality it
only refers to them, the physical placement of data on the disk is usually not
related to the placement of the directory.

### Basic File system functions

To know what is stored where, the system has a file table: it contains
information about all files. The file system determines how to organize this
the table. The methods vary depending on the OS, so different operating
systems have different file systems.

Also the file system:
- determines the size of the clusters — blocks of information into which
the file is divided;
- connects "pieces" of information from different clusters into single files;
- keeps track of which memory cells are currently free, occupied or unavailable;
- provides application programs with access to files;
- Providing work with files and data (creating, deleting, changing
attributes, reading and writing, searching, etc.)
- monitors the integrity and security of files, creates
recovery points;
- stores information about files, including name, size, and creation date.

## The main part

### FAT file system

FAT (English File Allocation Table "file allocation table") is a classic
file system architecture, which, due to its simplicity, is still widely
used for flash drives. It is used in floppy disks, memory cards and
some other media. Previously, it was also used on hard
drives.

#### The history of FAT creation

Developed by Bill Gates and Mark McDonald in 1976-1977.
It was used as the main file system in
the MS-DOS and Windows 9x operating systems. The FAT file system is a
file allocation table, which specifies:
- the addresses of the logical disk sections intended
for file placement;
- free areas of disk space;
- defective areas of the disk.
Files are usually organized into chains of clusters on disk. This structure
is determined by the root directory, which stores information about each file,
including its length and location on the disk.

#### FAT structure

1. Boot Sector (boot sector)
The Boot Sector stores information about the file system, including the code
needed to start booting the operating system, BIOS settings, and
other important data. It plays a key role when starting the computer and
starting to work with the file system.
2. File Allocation Table
The File Allocation Table contains records of files and free clusters on disk,
determining which clusters are occupied and which are available for writing new data.
FAT is an important part of the file system that controls the distribution
and placement of files on disk.
3. Root Directory (root directory)
The Root Directory stores records of each file and subdirectory on the disk. This
is a kind of table of disk contents that allows the operating system
to find files and directories.
4. Data Area
The Data Area is the actual storage of files and directories on
disk. This is where the data itself is placed, which the user saves in
the file system. The data area includes clusters that
are combined to store files.

The FAT file system always fills the free space on the disk
sequentially from the beginning to the end. When creating a new file or enlarging
an existing one, it searches for the very first free cluster in
the file allocation table. If some files were deleted during operation and others changed in
size, the resulting empty clusters will be scattered across the disk.
If the clusters containing the file data are not arranged in a row, then the file
turns out to be fragmented.

#### Different versions of FAT

There are four versions of FAT — FAT12, FAT16, FAT32 and exFAT (FAT64).
After the advent of large-volume hard drives (in those days
, disks with a size of 10-20 MB were considered large), the number of clusters increased, and 12 digits became
insufficient to store their numbers. A new 16-bit
file allocation table format was developed, where two bytes were allocated to store the number of one cluster
. The old file system developed for floppy disks was called FAT12, and the new one was called FAT-16.
The FAT-16 table, enlarged in size, no longer fits in one sector,
however, with large disk volumes, this disadvantage did not play a significant role. As before, two copies of the FAT table were stored on the disk for insurance.
However, when the disk size began to be measured in hundreds of MB and even in gigabytes,
the FAT-16 file system became inefficient again. In order for cluster numbers
to fit into 16 digits, when formatting large disks, you have
to increase the cluster size to 16 KB or even more. This caused problems when
it was necessary to store a large number of small files on the disk. Since
The storage space for files is allocated in clusters, even for a very
small file, you have to allocate too much disk memory.
As a result, another, apparently the last
attempt to improve the FAT file system was made - the cell size
of the file allocation table was increased to 32. This made it possible to format disks
of hundreds of MB and units of GB using a relatively
small cluster size. The new file system became known as FAT-32.
The most popular version of this file system is FAT32. She's pretty
the old, current version appeared back in the 90s. Then there were no such large
files and drives as now, and this was reflected in its features.

#### FAT32 Features

- the maximum file size in the FAT32 file system is 4 GB. 
You won't be able to record larger files like long videos into it;
- the system works quickly with large files, but is slower to handle a
lot of small ones;
- from the inside, the structure of the system is a hierarchical table with
data. There are three sections — the service section for system files, the table
pointers to search for data and the actual data space;
- FAT32 does not have modern encryption and data protection mechanisms.
FAT32 is not suitable for modern operating systems. At the same time, the system is fast,
convenient to work with, it is recognized and read by almost all devices. Therefore, now it
is used mainly for flash drives and memory cards.
Although FAT has its drawbacks, it has been widely adopted in many
systems due to its simplicity and versatility. However, with the development
of technology, more modern file systems (such as NTFS and ext4) have appeared,
which have greater reliability and functionality.

### NTFS file system

NTFS (English new technology file system — "new
technology file system") is a standard file system for
Microsoft's Windows NT family of operating systems.

#### History of NTFS creation

The New Technology File System was developed back in 1993, however, like FAT32,
it is still used today. The similarity with FAT is also evident in the fact that space
is divided into clusters of a given size. However
, it is the structure that provides NTFS with high flexibility.

#### NTFS structure

1. The first 12% of the NTFS–managed disk is allocated to the so-called MFT zone - the space into which the MFT metafile grows. It is not possible to write any
data to this area.
2. The remaining 88% of the disk is a regular file storage space
.
The basis of the NTFS volume structure is the MFT (Master File Table) – the main
file table, which contains at least one entry for each
volume file, including one entry for itself, i.e. it is a directory of all
available files, and small files (less than 1500 bytes) are stored
directly in the MFT – this significantly speeds up access to them
The internal structure of the catalog is a binary tree. To
search for a file with a given name in a linear directory, such as FAT,
for example, the operating system has to look through all the catalog items until it
He will find the right one. The binary tree arranges the file names in such a way that
the file search is carried out in a faster way — by obtaining
two-digit answers to questions about the position of the file. The question that the binary
tree is able to answer is: in which group, relative to this element,
is the name you are looking for — higher or lower? We start with such a question for
the middle element, and each answer narrows the search area by an average of two times. The files are,
say, just sorted alphabetically, and the answer to the question is carried out
the obvious way is by comparing the initial letters. The search area, narrowed in half
, begins to be explored in a similar way, starting again with the middle
element. Accordingly, the search engine does not have to view the entire chain
of files in the directory.

#### NTFS Features

- NTFS has logging, that is, information about file operations
is recorded in a special log.
- The system can work with large files, but the file name should be no
more than 255 characters.
- From the inside, the FS looks like a binary tree: the tree
-like data structure makes it easier to find the necessary information.
- There is data encryption, caching and an integrity protection system: any
operations with files either go to the end or are completely canceled. That is,
if the computer suddenly turns off in the middle of recording a file, there will be no "broken"
information, the recording will simply be canceled entirely.
- The NTFS file system has added the ability, which is not present in
the characteristics of the FAT file system, to open files
whose names do not use English letters, allowing you to use any
characters of the Unicode encoding standard "UTF". Thus, the limitations are
There is no use of symbols in the names of any complex languages, for example, Hindi
or Korean.

### Ext4 file system

Ext4 (English fourth extended file system, ext4fs) is a journaled file
system used primarily in operating systems with the Linux kernel,
created on the basis of ext3 in 2006.

#### History of ext4 creation

Most modern Linux distributions use
the Ext4 file system by default. Previous versions used Ext3, even earlier Ext2
, and if you go back far enough, then Ext.
Before there was an Ext file system, there was a file system
MINIX. If you are not familiar with the history of Unix, there used
to be a small MINIX operating system that ran on an IBM PC. Andrew
Tannenbaum developed it for training and released the source code in 1987. This
the operating system was not free. It was attached to the book that stood
69 dollars. However, it was not very expensive, so in the nineties MINIX
began to be implemented everywhere. Thanks to which the young Linus Torvalds developed
its Linux kernel is based on MINIX and released it in 1992.
MINIX had its own file system, which was used by the first versions
Linux. It could work with up to 64 megabytes of storage, and the file names
could not exceed 14 characters in size. In 1991, the average size of hard drives was 40-140
megabytes, so something else was needed for Linux. It is for this reason that
Remy Card developed the first Ext family file system in 1992. She
solved most of the MINIX problems. The new file system used a new
layer of VFS in the Linux kernel and could now work with disks up to 2 gigabytes, and
file names could consist of 255 characters. But Ext had one drawback. It had
only one timestamp for the file, instead of the current three: creation
dates, access dates, and modification dates.
Remy Card created Ext very quickly, and over the next year he developed Ext2 for
her replacement. It was already a serious commercial-grade file system. It
was quickly implemented in the Linux kernel and MINIX, and then in external modules that
made it available for Windows and macOS. Here
, the file system limits were increased again, but it still had one more problem. Like all file
systems of that time, when the power is turned off at the time of recording, the file system
it became inoperable.
Ext3 was developed to fix this. It has already used
logging, as has Microsoft's file system, NTFS. The magazine is
a special place on the disk where all operations with the file system are recorded.
Moreover, for the file system itself, they are applied only after they are
completely written to disk. If an error occurs, the file system simply
rolls back to the previous version from the log.
Then, in 2006, the Ext4 file system was announced and
another developer was already working on it. His name is Theodore Tso. Ext4 logging is also
supported. This file system got into the Linux kernel two years after
the announcement. The file system has significantly expanded the capabilities of Ext4, but still
it relied on old technology.
There are three levels of logging for Ext4:
1. The magazine is the safest mode. Data and
metadata are written to the log before they are saved to disk. This ensures
the complete safety of the recorded file, but reduces performance.
2. Ordered - this mode is used by default in many
distributions. The metadata is written to the log, but the data to be written
is immediately written to the file system. The order of work here is as follows:
first, metadata is written to the log, then data is written to
the file system, and only after that the metadata is also written to the
file system. In case of a failure, the new metadata is only in the log and
the file system can be very easily restored,
only those files that are written at the time of the failure will be damaged, all others
will remain in order.
3. Write-back is a less secure logging method. Here
, too, only metadata is written to the log, but they can
be written to the file system along with the data to improve performance. Despite
the fact that files recorded during a crash may be lost, for
This mode guarantees the security of the file system as a whole.

#### Ext4 structure

1. Superblock
Superblock contains important metadata about the file system, such as
file system size, number of blocks, type and version.
2. Inode Table
The Inode (Index Node) stores the metadata of files, except for their actual
contents. This includes various file attributes such as
access permissions, creation date and time, block numbers, etc.
3. Block Group Descriptor
This block contains information about block groups such as superblock, inode tables, and data blocks. It provides additional metadata for
file system, helping with the organization and management of blocks and inodes.
4. Data Blocks
The data area is the storage location for the files and directories themselves. This
is where the actual data representing files and directories are placed.
5. Directory Structure
The directory structure ensures that the file system is
organized hierarchically, allowing users to create and manage
files and subdirectories.
6. Journal
The log is used to maintain the integrity of the file system and
recover from failures. It records upcoming operations before their
execution, ensuring data integrity in the event of an emergency
shutdown.

#### Ext4 Features

- EXT starting with EXT3 — journaled systems, this means that all
changes occurring on the drive are recorded in a special
log. Therefore, the system is considered to be quite stable.
- Information in EXT is stored in bitmaps, that is
, sequences of bits. And the contents of the folders are presented in the form
of tree structures and linked lists.
 Extents have been added to EXT4 - a new way to write information to
continuous areas on the disk. This method improves productivity
and speed.
- High scalability: ext4 can support up to 1 EB (exabyte)
of disk space and up to 16 TB (terabyte) of file size, which significantly
exceeds the limitations of other file systems.
- High performance: ext4 uses various technologies to
speed up data reading and writing, such as multithreaded I/O,
pre-allocation, deferred allocation, expandable blocks,
etc., which allows it to work efficiently with large files and high
load.
- Reliable data integrity: ext4 protects data from
damage in case of failures or power outages using
logging or checksums, which allows it to quickly
recover data without loss or error.
- Flexible configuration: ext4 allows the user to select various
parameters and options when creating or mounting a file system,
such as block size, logging type, enabling or disabling
individual features, etc., which allows it to adapt to different
needs and conditions.

### Comparing file systems

Operating system support:
- The FAT file system (File Allocation Table) was developed in 1977 and
It is used in Windows and MS-DOS operating systems.
- The NTFS (New Technology File System) file system was developed
by Microsoft for Windows NT and higher operating systems.
- The ext4 (Fourth Extended Filesystem) file system is the standard
file system for most Linux distributions.
Scalability and file size:
- The FAT file system uses a File Allocation table
Table), which stores information about which blocks on the hard disk
are occupied by files. The file size in FAT is limited to 4 GB, and the partition size is 2 TB.
- The NTFS file system uses a more complex structure that
includes file allocation tables, a file system log, and other
elements. The file size in NTFS is limited to 16 EB (exabytes), and
the partition size is up to 256 TB.
- The ext4 file system uses an indexed structure that
allows you to quickly find files on disk. The file size in ext4 is limited to 16
TB, and the partition size is up to 1 EB (exabyte).
Functionality and complexity of the structure:
- FAT has a simple structure, its capabilities are limited compared to
modern file systems. There is no support for multi
-user mode and reliable data protection.
- NTFS has a complex structure, provides advanced
functionality, includes support for multi-user mode,
data protection and other functions.
- Ext4 is more complex than FAT, supports
large files and volumes well, and is suitable for modern systems.
Logging and Recovery:
- FAT: Has no logging mechanisms and is therefore less resilient
to failures.
- NTFS: Enables logging to ensure data integrity and
recovery.
- Ext4: Also includes logging mechanisms to ensure
reliability and crash recovery.
Security and access to data:
- FAT: Limited security and
access control capabilities.
- NTFS: Includes an extensive access system, file and folder encryption,
and file access auditing.
- Ext4: Has limited access control and security
features compared to NTFS.
Comparing the FAT, NTFS and ext4 file systems, it can be concluded that NTFS and
ext4 provide more advanced features compared to FAT. NTFS
It has a more complex structure than FAT and ext4, and supports large
file and partition sizes, multi-user mode and reliable data protection. Ext4
It also supports large file and partition sizes, as well
as multi-user mode and reliable data protection, but is the standard
file system for most Linux distributions. In general, the choice of file
system depends on the specific requirements of the user and the operating system on
which it will be used.

## Conclusion

File systems play an important role in organizing and storing data on
computers and other devices. Each of the considered file systems
has its own characteristics and applications in various operating systems. All
systems have their own strengths and weaknesses. There is no ideal file system, but there are
those that are better or worse suited for certain operating systems, goals and technologies.
Understanding and choosing the right file system will help ensure
reliability, security and efficient use of data.
