# 9attouset Rmed Writeup
**Description:**
My kitty is very crazy as hell and loves "HIDE & SEEK" so much.

**Attachment:**
[OTC1](../Files/OTC1.apk)

## Solution:

As we can see our file is an apk so we have to use `JADX` to decompile our file.

After decompiling our file with JADX it turns out that our apk file contains two activities: MainActivity and FlagActivity so it seems we can somehow access to the FlagActivity so let's try opening our apk, personally, I use `Blue Stacks` to open an apk file, it is an android emulator that helps you to open your android apps in a virtual machine.

Let's run our apk:

![app1](../Ressources/app1.png)

So after opening the apk it seems like we can not access the flag activity directly from the apk.

But something interesting in the AndroidManifest.xml

```xml
<activity
    android:name="com.example.otc1.FlagActivity"
    android:exported="true"/>
<activity
```
it seems like this activity can be accessible but with a dynamic tool, in this case, we will use `ADB` to access to the flag activity, these are the commands to access the flag activity:

    adb connect 127.0.0.1:5555

I used Blue Stacks so it will depend if you will use Android Studio emulator.

    abd -s 127.0.0.1:5555 shell

This command will open adb shell.

    p3s:/ $ su

This command will make the user a super user.

All we have to do is to find the package name of the activity, in this case it is "com.example.otc1".

All we need now is this command to start flag activity:

    p3s:/ # am start -n com.example.otc1/.FlagActivity
    Starting: Intent { cmp=com.example.otc1/.FlagActivity }

Running this command will lead us to the flag activity:

![app2](../Ressources/app2.png)

And we got our flag:

    FL1TZ{Sp1nn1ng_C4t_Oiiai_Oiiiai}

***Author: OTC***