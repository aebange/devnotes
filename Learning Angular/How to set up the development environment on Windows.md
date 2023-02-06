# Setting up the dev environment for Windows
A step-by-step guide on how to install everything you'll need to work with our Angular tech stack
<br>
<br>
### Opening Notes:
This process is going to take some time. **Versions are important**, we want you to *use older versions* because newer 
versions may contain changes that break our procedures. Try to follow installation instructions in order
and **do not skim**. You will save yourself time if you take the time to read carefully, otherwise - have 
fun debugging stupid mistakes.

### Index:
1. [The Tools directory](#the-tools-directory)
2. [The PATH](#the-path)
3. [PostgreSQL 9.6.24](#postgresql-9624)
4. [OpenJDK 1.8](#openjdk-18)
5. [7-Zip](#7-zip)
6. [Maven 3.3.9](#maven-339)
7. [Git for Windows](#git-for-windows)
9. [Node Version Manager (NVM)](#node-version-manager-nvm)
10. [Node 12.5.0](#node-1250)
11. [Angular 9 CLI](#angular-9-cli)
12. [IntelliJ](#intellij)
13. [Postman](#postman)
14. [Putty](#putty)

___

## The tools directory
Let's start with the easy stuff. We need a folder in a smart location to hold our installed tools.
<br>
<br>

### Set up the tools directory
1. Open a command prompt (admin)
   1. Press ``Windows Key`` + ``R``
      1. The "Run" menu should appear
   2. Type ``cmd`` in the text bar
   3. Click "OK" 
   4. You should see a black box (command prompt) open up
2. Navigate to your C: directory
   1. NOTE: You can paste into the command prompt by right-clicking
   2. Type ``cd C:\\`` and hit enter (markdown forces me to use two backslashes)
      1. This will change your cursor/pointer location to C:\
   3. Type ``mkdir tools``
      1. This will create a new folder called "tools" in C:
      2. NOTE: If you create this folder as an Admin, every time you try to install/use something you'll need permission (not suggested)


___

## The PATH
A quick explanation for the un-initiated
<br>
<br>
**If you are familiar with the PATH, you may ignore this section.** I wanted to explain briefly what the PATH is for those
who do not know, as several times during this guide we will be adding items to the PATH.

The PATH is an *environment variable*. Items in the PATH can be accessed from *anywhere* in your computer via the
command prompt. For example, say you put a shortcut to Google Chrome in your PATH. This would
allow you to type ``chrome.exe`` into your command terminal from your tools folder, and the shortcut would run 
*even though your cursor/pointer for your command prompt was not in the right folder*. 

We add things to the PATH to make them accessible by ourselves, and by other programs/tools.

You can edit your path/environment variables by searching "environment variables" in Windows search or via this shortcut...

### Editing environment variables via the run menu
1. Press ``Windows Key`` + ``R``
2. Enter ``rundll32.exe sysdm.cpl,EditEnvironmentVariables`` (Save yourself some time and copy/paste)
3. Press "OK"


___

## PostgreSQL 9.6.24
Setting up our local SQL database
<br>
<br>
### Installation instructions
1. Go to [this download link to get the installer](https://www.enterprisedb.com/postgresql-tutorial-resources-training?uuid=91f8264f-8a2d-434a-9559-4dfa9b3b80b1&campaignId=701380000017oAXAAY)
   1. If that link is dead, use [this page instead](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
2. Run the MSI Installer
   1. Open the ``postgresql-9.6.24-1-windows-x64.exe`` file you just downloaded (should be in your downloads folder)
      1. NOTE: It may install Microsoft Visual C++ Redistributable, this is fine
   2. Press "Next" until you reach the selector for **installation directory**
      1. Select ``C:\tools`` - the folder we created earlier
         1. The final install path should be something like ``C:\tools\PostgreSQL\9.6``
      2. For the data directory, use the defaults
         1. ``C:\tools\PostgreSQL\9.6\data``
   3. For the password:
      1. NOTE: This is a local database, it's OK to have an easy-to-type password
      2. Make the password ``secret`` (you'll thank me later)
      3. Confirm it as ``secret``
   4. For the port:
      1. Use the default ``5432``
   5. Click through the defaults for the next few menus, but **stop when you see "Completing the postgresql setup wizard"**
      1. Un-check "Launch Stackbuilder at exit" - we don't care for this
      2. Press finish
3. PostgreSQL is now installed (but not deployed, we'll do that later)


___

## OpenJDK 1.8
Setting up our Java installation (Disclaimer, this one sucks)
<br>
<br>
### Installation Instructions
1. Get a RedHat account (if you don't have one)
   1. Go to [this link](https://sso.redhat.com/auth/) and register/login
   2. Do whatever registration stuff you need to, then go back to that link and login
2. Now you can download the OpenJDK installer (you need an account)
   1. Download [this file](https://developers.redhat.com/content-gateway/file/java-1.8.0-openjdk-1.8.0.332-2.b09.redhat.windows.x86_64.msi) to get the MSI installer
      1. If that link died, try [this page instead](https://developers.redhat.com/products/openjdk/download) and look for the build with 332 in it
3. Run the MSI Installer you just downloaded
   1. Settings do not matter, just click through the installer
4. Create the **JAVA_HOME** environment variable
   1. Open the environment variable editor and apply our changes
      1. Press ``Windows Key`` + ``R``
      2. Copy/paste ``rundll32.exe sysdm.cpl,EditEnvironmentVariables`` into the text bar
      3. Hit "OK"
         1. A window should open
      4. Press "New" (the one in the upper half of the window)
         1. Variable Name: ``JAVA_HOME``
         2. Value: ``C:\Program Files\RedHat\java-1.8.0-openjdk-1.8.0.332-2`` (or wherever it installed to)
      5. Press "OK"
5. Verify that Java is now accessible in the PATH
   1. Open a **NEW** command prompt (old prompts may not update env vars)
      1. Press ``Windows Key`` + ``R``
      2. Type in ``cmd``
      3. Hit ``enter``
   2. In the command prompt...
      1. Type ``echo %JAVA_HOME%`` and press ``enter``
      2. You should see the path you added to your JAVA_HOME value
         1. If nothing shows, you did it wrong
      3. Type ``java -version`` and press ``enter``
         1. You should see the java version (containing 1.8.something)


___

## 7-Zip
If you have Win-rar, that works too
<br>
<br>
### Installation Instructions
1. Download 7-Zip
   1. Download [this installer](https://www.7-zip.org/a/7z2201-x64.exe)
      1. If that link is dead, go to [this page](https://www.7-zip.org/download.html)
2. Run the installer
   1. Use the default settings and install the tool


___

## Maven 3.3.9
Setting up our Java dependency manager
<br>
<br>
### Installation Instructions
1. Download Maven
   1. Download [this zip file](https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.3.9/apache-maven-3.3.9-bin.zip)
      1. If the link is dead, go to [this page](https://maven.apache.org/download.cgi)
2. Unzip the .zip file with 7-zip (Windows un-zipper is too weak for this task)
   1. Right-click on ``apache-maven-3.3.9.bin.zip``
      1. Select 7-Zip -> Extract files...
   2. In the "Extract" popup window
      1. Extract To: ``c:\tools``
   3. Press "OK"
   4. Now, you should have a folder in ``c:\tools\apache-maven-3.3.9\`` containing the zip contents extracted
3. Create the **M2_HOME** environment variable
   1. Open the environment variable editor and apply our changes
      1. Press ``Windows Key`` + ``R``
      2. Copy/paste ``rundll32.exe sysdm.cpl,EditEnvironmentVariables`` into the text bar
      3. Hit "OK"
         1. A window should open
      4. Press "New" (the one in the upper half of the window)
         1. Variable Name: ``M2_HOME``
         2. Value: ``C:\tools\apache-maven-3.3.9`` (or wherever it installed to)
      5. Press "OK"
4. Add the **Maven\bin** directory to your PATH *(be careful, this one is different)*
   1. Open the environment variable editor and apply our changes
      1. Press ``Windows Key`` + ``R``
         1. Copy/paste ``rundll32.exe sysdm.cpl,EditEnvironmentVariables`` into the text bar
         2. Hit "OK"
            1. A window should open
         3. If you can see a variable in the list called "PATH", then click it
            2. With the "PATH" row highlighted, click the "Edit" button
               1. A new window should open, click "NEW"
                  1. Enter ``%M2_HOME%\bin``
                  2. Hit OK and click through the rest of the menus
         4. If PATH does not exist, click "New"
            1. Variable Name: ``PATH``
            2. Value: ``%M2_HOME%\bin``
         5. Hit OK and click through the rest of the menus
5. Verify that Maven works
   1. Open a **NEW** command prompt (old prompts may not update env vars)
      1. Press ``Windows Key`` + ``R``
      2. Type in ``cmd``
      3. Hit ``enter``
   2. Type ``mvn -version`` and press ``Enter`` 
   3. You should see "Apache Maven 3.3.9" and a bunch of other stuff appear
   4. Let's run your first **mvn** command as a test to make sure all is well
      1. Type in ``mvn help:effective-settings`` and press ``Enter``
      2. You should see a bunch of tags appear, followed by a block containing "BUILD SUCCESS"


___

## Git for Windows
Setting up our version control system
<br>
<br>
### Installation Instructions
1. Download Git for Windows
   1. Download [this installer](https://github.com/git-for-windows/git/releases/download/v2.39.1.windows.1/Git-2.39.1-64-bit.exe)
      1. If that link is dead, go to [this page](https://gitforwindows.org/)
2. Run the installer with default settings until it's finished


___

## Node Version Manager (NVM)
Setting up our version manager for node
<br>
<br>
### Installation Instructions
1. If you've installed Node before, please uninstall it now
   1. If you're unsure, check the Windows application wizard
      1. Press ``Windows Key`` + ``R``
      2. Enter ``appwiz.cpl`` and hit "OK"
      3. Search for an item called "Node", uninstall it by clicking on it and hitting "Uninstall" (if you find it)
2. Download the Node Version Manager
   1. Download [this installer](https://github.com/coreybutler/nvm-windows/releases/download/1.1.10/nvm-setup.exe)
      1. If that link is dead, go to [this page](https://github.com/nvm-sh/nvm)
3. Open the ``nvm-setup.exe`` installer you just downloaded (should be in your downloads folder)
   1. Install to ``c:\tools\nvm`` (first prompt)
   2. For the second directory prompt, leave as default
   3. Click through the installer/finish it
4. Add ``c:\tools\nvm`` to your PATH (to use nvm commands from anywhere)
   1. Open the environment variable editor and apply our changes
      1. Press ``Windows Key`` + ``R``
         1. Copy/paste ``rundll32.exe sysdm.cpl,EditEnvironmentVariables`` into the text bar
         2. Hit "OK"
            1. A window should open
         3. If you can see a variable in the list called "PATH", then click it
            2. With the "PATH" row highlighted, click the "Edit" button
               1. A new window should open, click "NEW"
                  1. Enter ``c:\tools\nvm``
                  2. Hit OK and click through the rest of the menus
         4. If PATH does not exist, click "New"
            1. Variable Name: ``PATH``
            2. Value: ``c:\tools\nvm``
         5. Hit OK and click through the rest of the menus
5. Verify that nvm is working correctly
   1. Open a new command prompt
      1. ``Windows Key`` + ``R``
      2. Enter ``cmd`` and hit "OK"
   2. In the command prompt...
      1. Enter ``nvm ls`` and hit ``Enter``
      2. You should see that no versions are currently installed


___

## Node 12.5.0
Setting up our Javascript environment
<br>
<br>
### Installation Instructions
1. Open a new command prompt
   1. ``Windows Key`` + ``R``
   2. Enter ``cmd`` and hit "OK"
2. Install Node 12.5.0
   1. Enter ``nvm install 12.5.0`` and hit ``Enter``
   2. Enter ``nvm use 12.5.0`` and hit ``Enter``
3. Verify Node is using the correct version
   1. Enter ``node --version``
      1. You should see that 12.5.0 is in-use


___

## Angular 9 CLI
Setting up our angular environment
<br>
<br>
### Installation Instructions
1. Open a new command prompt
   1. ``Windows Key`` + ``R``
   2. Enter ``cmd`` and hit "OK"
2. Uninstall any existing versions of Angular CLI
   1. Enter ``npm uninstall @angular/cli`` and hit ``Enter``
   2. Enter ``npm uninstall -g @angular/cli`` and hit ``Enter``
3. Install Angular 9 CLI
   1. Enter ``npm install -g @angular/cli@9.0.6`` and hit ``Enter``
4. Verify that Angular 9 CLI is installed
   1. Enter ``npm list -g --depth 0``
      1. You should see that Angular 9.0.6 is installed


___

## IntelliJ
Setting up our IDE, this is the hardest part
<br>
<br>
### IntelliJ Pro vs Community Edition
You do not need IntelliJ pro. **If you are a college student or have a .edu email, you can get pro for free [by going here.](https://www.jetbrains.com/community/education/#students)** 
Otherwise, the Community Edition is still a complete package. *The features offered by the Pro version will not be necessary 
for most projects.* However - for our project, you will want a tool that can view/edit PostgreSQL databases. 
This can be accomplished with free 3rd party applications like [pgAdmin](https://www.pgadmin.org/download/). 
The Pro version of IntelliJ comes with an integrated database tool that will fill this role otherwise.
<br>
<br>
### You *must* use IntelliJ 2021.2.4 or older**
Currently, we are relying on a 3rd-party plugin called "Multi-run" for debugging. Multi-run currently has been abandoned. 
We need to fork this plugin to fix it on newer versions of IntelliJ, but until we do this, we have to use an older version.
<br>
<br>
### Installation Instructions
1. Download IntelliJ
   1. Get the **Professional/Ultimate Edition** (2021.2.4)
      1. Download [this installer](https://download.jetbrains.com/idea/ideaIU-2021.2.4.exe)
         1. If that link is broken, find 2021.2.4 on [this page](https://www.jetbrains.com/idea/download/other.html)
   2. OR get the **Community Edition** (2021.2.4)
      1. Download [this installer](https://download.jetbrains.com/idea/ideaIC-2021.2.4.exe)
         1. If that link is broken, find 2021.2.4 on [this page](https://www.jetbrains.com/idea/download/other.html)
2. Run the Installer you just downloaded
   1. When asked where you want to install IntelliJ, select ``c:\tools\intellij``
   2. The rest of the menu options can be left to the default, finish the installation
<br>

### Configure IntelliJ settings
1. Make sure **Maven** is configured correctly
      1. Go to ``File`` -> ``Settings``
         1. Search for ``Maven``
            1. Under ``Build, Execution, Deployment`` -> ``Build Tools`` -> ``Maven``
               1. Maven home path: ``c:\tools\apache-maven-3.3.9``
               2. User Settings file: ``c:\tools\apache-maven-3.3.9\conf\settings.xml``
2. I suggest leaving spell checking enabled
3. Disable **Smart Quotes**
   1. Go to ``File`` -> ``Settings``
      1. Search for ``quote``
         1. Select ``Smart Keys`` on the left
            1. Uncheck ``Insert pair quote``
         2. Select ``HTML/CSS`` on the left
            1. Uncheck ``Add quotes for attribute values on typing '='``
4. If IntelliJ is running slowly, you can attempt to **disable un-used plugins** by going to ``File`` -> ``Settings`` -> ``Plugins`` -> ``Installed``
   1. This does not majorly improve performance, only attempt in extreme situations (*un-installing plugins one-by-one takes a long time*)
5. Disable **automatic updates**
   1. Go to``File`` -> ``Settings`` -> ``Appearance & Behavior`` -> ``System Settings`` -> ``Updates``
      1. Uncheck ``Automatically check updates for...``
6. Install the **Multi-run** plugin
   1. Go to ``File`` -> ``Settings`` and search ``Plugins``
      1. Click the ``Marketplace`` tab in the plugin menu on the left
         1. Search ``Multirun`` and install it (the logo is two green arrows)
         2. Make sure Multirun installed
            1. Click the ``Installed`` tab in the Plugins menu and ensure Multirun is enabled/checked
               1. NOTE: If this is grayed out, IntelliJ is still probably indexing. Try restarting
7. Tell IntelliJ **not to open the last project** when starting
   1. Go to ``File`` -> ``Settings`` and search ``reopen``
      1. In the ``System Settings`` menu on the left, uncheck ``Reopen projects on startup``
8. Disable the Javascript/Typescript Code Linter
   1. IntelliJ comes with a tool to suggest "best practices" when programming with JS/TS. But their "best practices" aren't necessarily the best
      1. Go to ``File`` -> ``Settings`` -> ``Editor`` -> ``Inspections`` -> ``JavaScript and Typescript`` -> ``Code quality tools``
      2. Uncheck **ESLint** and **TSLint**
9. (Optional) Install a theme plugin to make IntelliJ easier on the eyes
   1. Go to ``File`` -> ``Settings`` and search ``Plugins``
      1. Click the ``Marketplace`` tab in the plugin menu on the left
         1. Search ``Gradianto`` and install it (the logo is two green arrows)
            1. Your browser by default will have the theme changed to an ugly teal (in my opinion)
               1. If you go to ``File`` -> ``Settings`` -> ``Appearance & Behavior`` -> ``Appearance``
                  1. There is a drop-down menu for ``Theme``
                     1. If you look in that menu, you should now have "Gradianto" options - choose your favorite
                        1. NOTE: Sometimes IntelliJ needs a second to detect these, you may need to restart IntelliJ
<br>

### Configure the project/run-environment in IntelliJ
1. Clone the project using git
   1. Go to whatever tool/website you're using for version control online (Github, Gitlab, etc)
   2. Navigate to the project page and click the button on the right that provides the git command for cloning
      ![Image Depicting Github Link Location](https://i.imgur.com/gOiHi7J.png)
      1. NOTE: Depending on your security, you may need to use the ``HTTPS`` OR ``SSH`` method
   3. Open a new command prompt
      1. ``Windows Key`` + ``R``
      2. Type ``cmd`` and hit "OK"
   4. Navigate to the IntelliJ project directory
      1. Type ``cd %USERPROFILE%`` and hit ``Enter``
      2. Type ``cd IdeaProjects`` and hit ``Enter``
         1. NOTE: If ``IdeaProjects`` doesn't exist, create it
            1. Type ``mkdir IdeaProjects`` and hit ``Enter``
            2. Type ``cd IdeaProjects`` and hit ``Enter``
   5. Git clone your project
      1. Type ``git clone `` and paste the link you got from Github/Gitlab/whatever
         1. It should look something like ``git clone https://github.com/something``
         2. Hit ``Enter`` and hopefully it downloads the project
      2. Enter your project's folder by typing ``cd PROJECTNAMEHERE``
   6. Attempt to build the package with maven
      1. This is the ultimate test that you've installed everything correctly
      2. Enter ``mvn clean package -Pprod`` and hit ``Enter`` and hopefully everything builds successfully
2. Set up your project
   1. Open IntelliJ (if it's already open, go to ``File`` -> ``Open``)
   2. Open your project's folder
      1. Navigate to where your cloned project folder is (something like ``C:\Users\Your Name\IdeaProjects\YourProjectNameHere``)
      2. Select the folder and hit open
   3. IntelliJ will now open your project and begin indexing, this will take a moment
   4. Make sure that the folder name displayed in the project browser (the left side tabs) is correct
3. Set up your run configs
   1. Set up the Java back-end
      1. You should have a folder in your project called ``Backend``, expand that
      2. Find ``Backend\src\main\java\com.lessons\Application`` (we're looking for a Java class called Application)
      3. Right-click the Application file and hit "Debug"
         1. NOTE: If this is grayed out, IntelliJ is still probably indexing. Try restarting
      4. This should start debugging the back-end. You should see something in your IntelliJ debug terminal that says something like "Webapp is up"
         1. If you do see "Webapp is up" then it worked, otherwise you're gonna have to find the problem (good luck)
      5. Go to ``Run`` -> ``Edit Configurations`` (top of your screen)
         1. You should see "Spring Boot", expand that row to see "Application" under it
            ![An image depicting the spring boot run config](https://i.imgur.com/oZJomBm.png)
      6. Rename "Application" to something like ``App Backend`` (you may want to include your app's name - this name is up to you)
      7. Expand the "Environment" tab and add ``-Dspring.profiles.active=dev``
      8. Hit ``Apply`` and ``OK``
   2. Set upm your Angular Application
      1. Go to ``Run`` -> ``Edit Configurations``
         1. Select the ``+`` icon and in the dropdown click on ``NPM`` (red icon)
         2. In the window that opens...
            1. Name: ``Angular CLI Server`` 
            2. Command: ``run``
            3. Scripts: ``start``
            4. Package.json: **This should auto-populate, make sure it points to your package.json file in the frontend folder**
            5. Node Interpreter: ``~/.nvm/versions/node/v12.5.0/bin/node``
               ![An image depicting how the angular app should look](https://i.imgur.com/SkJNy4G.png)
         3. Hit ``Apply`` and ``OK``
   3. Set up your Frontend
      1. Go to ``Run`` -> ``Edit Configurations``
         1. Select the ``+`` icon and in the dropdown click on ``Javascript Debug``
         2. In the window that opens...
            1. Name: ``App Frontend`` (you may want to include your app's name - this name is up to you)
            2. URL: ``http://localhost:4200``
            3. Browser: ``Chrome`` 
               1. NOTE: **FIREFOX WILL NOT WORK HERE** DESPITE BEING LISTED, INTELLIJ DOES NOT SUPPORT IT STILL (2023)
            4. Check: ``Ensure breakpoints are detected when loading scripts``
               ![An image depicting how the frontend should look](https://i.imgur.com/ZDCDte3.png)
      2. Press ``Apply`` and ``OK``
   4. Set up multi-run (the full app)
      1. Go to ``Run`` -> ``Edit Configurations``
         1. Select the ``+`` icon and in the dropdown click on ``Multirun`` (two green arrows, if it's grayed out you may need to restart IntelliJ)
         2. In the window that opens...
            1. Name: ``Full webApp`` (you may want to include your app's name - this name is up to you)
            2. Hit the ``+`` (the one in the configuration tab this time)
               1. Select your **Backend**
            3. Hit the ``+`` (the one in the configuration tab this time)
               2. Select your **Angular CLI Server** 
            4. Hit the ``+`` (the one in the configuration tab this time)
               1. Select your **Frontend**
            5. Delay: ``5.0`` seconds (or 7.0 seconds if your PC sucks)
            6. Check the same boxes as the image below (1, 4, and 5)
               ![An image depicting how the multirun should look](https://i.imgur.com/ELipY7A.png)
         3. Hit ``Apply`` and ``OK``
4. Run the full webapp
   1. In the top right of IntelliJ, hit the dropdown menu to the left of the play button
   2. Select your Multirun ``Full webApp`` option
   3. Hit the debug button
   4. Hopefully your full webapp starts up, godspeed


___

## Postman
Setting up our API testing tool
<br>
<br>
### Installation Instructions
1. Download Postman
   1. Download [this installer](https://dl.pstmn.io/download/latest/win64)
      1. If that link is dead, go to [this page](https://www.postman.com/downloads/?utm_source=postman-home)
      2. NOTE: Version does not matter here
2. Open/run the installer you just downloaded
   1. The default settings should be fine for now


___

## Putty
Setting up our Telnet client
<br>
<br>
### Installation Instructions
1. Download Putty
   1. Download [this installer](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.78-installer.msi)
      1. If that link is dead, go to [this page](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
      2. NOTE: Version does not matter here
2. Open/run the installer you just downloaded
   1. The default settings should be fine for now