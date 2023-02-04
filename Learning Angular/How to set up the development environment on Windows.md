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
1. The Tools directory
2. The PATH
3. PostgreSQL 9.6.24
4. OpenJDK 1.8
5. Maven 3.3.9
6. Git for Windows
7. Nssm
8. Node Version Manager (NVM)
9. Node 12.5.0
10. Angular 9 CLI
11. 7-Zip
12. IntelliJ
13. Postman (optional)
14. Putty (optional)
15. Sublime Text (optional)
16. Notepad++ (optional)

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
   3. Hold ``ctrl`` + ``shift`` and press ``enter`` (or click ok)
   4. You should see a black box (command prompt) open up
2. Navigate to your C: directory
   1. NOTE: You can paste into the command prompt by right-clicking
   2. Type ``cd C:\\`` and hit enter (markdown forces me to use two backslashes)
      1. This will change your cursor/pointer location to C:\
   3. Type ``mkdir tools``
      1. This will create a new folder called "tools" in C:


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
   1. Run the ``postgresql-9.6.24-1-windows-x64.exe`` file you just downloaded
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
WIP