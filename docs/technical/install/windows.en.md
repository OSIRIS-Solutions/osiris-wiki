---
tags:
  - Windows
  - installation
  - XAMPP
  - MongoDB
---

# Windows

## Installation under Windows (for Dummies)

> This guide has not been officially tested and may not work in the future. It is only intended as a guide for installing OSIRIS under Windows.

Many thanks to **Robin Lange** from IWH, who provided us with the following installation instructions for Windows. According to him, it is written "for dummies", i.e. for someone who has never had anything to do with web servers before.



## Required programmes

* [XAMPP](https://www.apachefriends.org/download.html)
* [MongoDB Database](https://www.mongodb.com/try/download/community)
* [MongoDB PHP Driver](https://pecl.php.net/package/mongodb)
* [Git for Windows](https://git-scm.com/download/win)
* [Composer](https://getcomposer.org/download/)



## Instructions


1. download and install [XAMPP](https://www.apachefriends.org/download.html). It is important that you choose a version with PHP 8.1.x (PHP 8.2 is not yet supported by the Mongo PHP Driver for Windows). If you want the website to be accessible to everyone in the network/wifi later, you must allow access to the network in the Windows firewall at the end of the installation.  
Once XAMPP has been installed, start the programme and click on "Start" for Apache. Then enter [localhost](http://localhost) in the browser. The standard Apache page should now be displayed.  

2. download the [MongoDB database](https://www.mongodb.com/try/download/community) as .msi and install it. The latest version (6.0) is compatible with OSIRIS. Leave all default settings in the installation menu as they are.  

3. download the [MongoDB PHP Driver](https://pecl.php.net/package/mongodb). To do this, scroll to version 1.13.0 in the list and click on the blue Windows box with "DLL". At the bottom of the list, select the PHP 8.1 Thread Safe version for your system (usually x64). The DLL file must then be copied to the following folder in your Xampp directory ...\xampp\php\ext
If this is successful, you still need to activate the driver. To do this, search for the php.ini file in the directory ...\xampp\php (:exclamation: Attention: if you do not display the file extensions in Windows, the file may simply be called "php". As there are several such files, it makes sense to display the file extensions).  
Open the php.ini file with the editor of your choice (e.g. Notepad++) and add the line "extension=php\_mongodb.dll". For the sake of clarity, you should insert the line where the other extensions are listed (approximately line 950). However, this is not absolutely necessary.  

4. install [Git for Windows](https://git-scm.com/download/win). Leave all default settings in the installation menu. Then open the folder ...\xampp\htdocs.  
The website is loaded into this folder. First delete all files in the folder. Then right-click in the empty directory and click on "Git Bash Here". A command window opens. Enter the following command:
`git clone https://github.com/JKoblitz/osiris.git`
:exclamation: Watch out! You cannot paste the code here with Ctrl+V. If you try to do this anyway, an "invisible character" will form and the subsequent code will no longer work. However, you can insert the code by left-clicking.
All files from the Git repository should now be copied into the directory.  

5. finally, install [Composer](https://getcomposer.org/download/) with the .exe file. All default settings can be left as they are. Then open the Windows command prompt (cmd) by entering "cmd" in the search bar, for example.  
In the terminal, you must now navigate to the Xampp directory of the website. This is done with the command `cd`. If Xampp is installed by default, the command is `cd C:\xampp\htdocs`. If you are now in the directory, enter `Composer update`. If everything works, Composer will load the missing libraries.  

6. if not already done, start XAMPP and click on "Start" or "Restart" for Apache. Enter [localhost](http://localhost) in the browser and the website should appear.  

7. further settings can be made in the config file (such as administrator, ldap, auth, ...). See the section [Configure OSIRIS](../configure/index.md).


