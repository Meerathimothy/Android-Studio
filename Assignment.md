
                                  DIVA Android App 

adb devices - command will show us status of any android device running on our system or not.

As VM which we started earlier is running, now it’s time to install DIVA application. Run command
given below

adb install diva-beta.apk

You will get success status printed on command line

Icon of DIVA app will also apprear on Android Studio

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/8e6aa4b2-80aa-48fd-8d8f-b23a4fe9c55a)

Click on the DIVA app Icon to launch the application

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/11a8bc46-1156-4732-bf89-b4abf80a3840)

                                    1. Insecure Logging

The mobile application logs sensitive data, including user actions, API responses  or error 
messages, without proper security controls.This can lead to unintentional exposure of sensitive
information if an attacker gains access to the device or intercepts the log files.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/d8a8217d-8657-4777-b45e-66a6a5591b3e)

adb shell - shell will get open.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/ba64b314-0eec-46c5-9758-b9567a301c75)

logcat - logs will start appearing.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/d337e9b0-e185-4ae7-a873-959df1b3e513)

Now go to the Android Studio and there enter credit card number

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/b3b543f0-73bb-48ec-90d3-797a5ff7e317)


Now go back the the command line where logs are appearing you will find there Credit Card 
Number in plain text.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/8f8f7a1d-f691-4f0c-a611-6197760fb3f2)

                                    2. Hardcoding Issues - Part 1

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/89495635-995f-4d20-a3ea-aa5b01217267)

As this is hardcoding challenge this mean the Vendor Key is hardcoded in the application. In order to get the hardcoded key we need to do Reverse Engineering of this application.

First convert the APK file into RAR file by only changing the extension.

Then Extract the RAR file. You will get DEX file along with other files and folders

Now convert this classes.dex file into jar file with Dex2Jar tool. Go to Command Prompt and enter following command:

d2j-dex2jar classes.dex

A converted jar file with same name as dex file will appear in the folder

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/17515d6f-a868-47a1-99f3-4451e0cd8af1)

Now access this JAR file with JD-GUI which is Java Decompiler,.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/b837ade1-e3e8-4af5-8eba-ad4940cdfa72)

Now we got the vendor’s secret key. Enter the Secret key to get access in app.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/9440e0a8-5f01-4f5d-82ce-49514f471727)

                           3.INSECURE DATA STORAGE - PART 1

In order to complete this challenge we have to move around in Directories with the help of shell. But first review the source code of this activity. We can see that credentials are stored in Shared Preferences

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/fd518fc3-7d60-461d-aebf-471e5524c3a6)

After accessing shell with adb go to /data/data/. This is the location where packages of all installed applications are stored.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/f52dec21-e890-4368-9d78-faaa4195fbd8)

First enter username and password and save them before try to find them in these directories.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/a68dc1ee-7d05-4bc7-84ae-3dbb78430a9e)

view contents of XML file to get username and password 

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/cec5cf51-8eca-4bd2-88e1-3690c83f9d31)


                        4. INSECURE DATA STORAGE - PART 2

This is similar challenge to previous one but credentials are stored in different location.

Before going to solve this challenge save credentials from in application.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/184cf523-07cc-4ce6-a4cc-f8c8471606c3)

This time credentials were stored in database ids2 and in its myuser table. In order to access database files from command line we have a tool in platform tools folder named “sqlite3”

sqlite3 <database_name>

sqlite3 ids2

sqlite>.tables

This .tables command will show all of the tables available in that particular database.

to exit from this tool .exit command is used.

sqlite>.exit

This will return you to your previous shell on which it iswere working

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/577f6a7f-f699-4a4a-a529-e33761d37223)

                                 5.  INSECURE DATA STORAGE - PART 3
                           
Enter the credentials from application.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/800f2bee-aad5-4975-8aa2-ccdadebb5c61)

The credentials were stored in temporary file. We are accessing those temporary file from shell

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/a6e75924-9b74-41ed-9ea6-63294d64f267)

                           6.INSECURE DATA STORAGE - PART 4

Enter the credentials from application.

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/9e3e72be-04d1-4212-9eb4-efad8ab0aeba)

The app is storing credentials in external storage

                              7. INPUT VALIDATION ISSUES - PART 1:

This challenge is about SQL Injection. First try to enter single quote (‘) as input and check result.
Try to enter single quote twice (‘’) and then check result.


![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/479ae7ff-9d40-4f1c-8cd4-e708c90c814b)

                            8. INPUT VALIDATION ISSUES - PART 2

In this challenge we have to access local files using URL.First let’s try to access Google

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/b9fca1f2-c46f-41eb-ad4b-5618e4ee44c1)

Trying to change the URL and try to access a file from device.
file:////etc/hosts

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/c85d91bd-76d5-42a7-b824-2eb81d9102dc)

try to access a file from shared preferences where credentials are stored.

file:///data/data/jakhar.aseem.diva/shared_prefs/jakhar.aseem.diva_preferences.xml

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/b35ac6c4-6731-4a96-b339-22620c01d1c6)

                            9. ACCESS CONTROL ISSUES - Part 1:

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/bbb81ffa-7a7a-4e91-80db-c88f5e265712)

Accessing credentials from “View API Credentials” Button is completely legal. We need to check that can we directly access credentials without going through this activity or this checkpoint. In order to do that first we have to get the name of activity which will appear after it, for that we take the help of logcat. Run logcat command from android shell then click on “View API Credentials” Button a log will generated related to this which gives us name of next activity 


![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/4491d0d0-f275-4f5a-b40f-8d9be2d5f8cc)

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/e2bf4355-29a2-4426-8535-dfe306a9cdb8)

As we got the activity name. Now let’s try to access it from adb activity manager directly.
adb shell am start -n jakhar.aseem.diva/.APICredsActivity

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/ab253db6-25a5-474b-b514-434490f443d1)

As you can see we got access of API Credentials without any restriction or authentication. This also means that other apps can also access these credentials

                            10. ACCESS CONTROL ISSUES - PART 2

First we have to de compile the application. We use APKTOOL for it.On Command Line type the following command:

apktool_2.4.1.jar d diva-beta.apk

A folder of application name will be created in same directory of apk. From there open AndroidManifest.XML file

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/22902b8e-2023-44e5-8df5-a1c199e455ac)

open the Java code file and inspect there about any new thing

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/2a88c6d9-8397-4b49-b94b-0eb3f0185583)

Search this highlighted parameter in R.class

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/5f9961e3-4bd3-4835-aadd-778bb4bda315)

In order to get the value of this chk_pin we have to inspect the strings.XML file which is located in application decompiled folder /res/values/string.xml.Let’s try to access the credentials with these details we have found and disabling check pin.

adb shell am start -n jakhar.aseem.diva/.APICreds2Activity -a jakhar.aseem.diva.action.View_CREDS2 --ez check_pin false

![image](https://github.com/Meerathimothy/Android-Studio/assets/57287429/edd2fb7a-201b-4d85-b0cb-948676a0644d)
