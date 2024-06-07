Title: Factory Resetting the Yealink T48S
Status: draft


We had a new user start (A) and part of the onboarding process is obviously a desk phone.

We have a fleet of Yealink T48S phones that interact with RingCentral. When looking to create a user for the new employee, I found that a new user would cost us $35 so I edited a user from the employee that left to the new user which was successful.

I then grab the phone from the previous user and plug it in. Everything is setup from other phones perspective (the new user shows up with the correct name in other people's phones) but on the phone I just booted up the old employees name is still there. I read that I should factory reset the phone which, upon attempting to get to advanced settings, I did not know the password as they were provisioned by the previous System Administrator.

We have another user (B) test by changing their presence and still doesn't update.

That was until B worked from home and plugged their phone in at home and the presence updated.

This lead me to believe that the issue was with our firewall blocking a certain RC server for no apparent reason. I then rushed to forums/RC knowledgebase to find the culprit IP address. I couldn't find anything. At a dead end, and also reading how awful RingCentral customer service is, I dejectedly reached out to support. Letting them know of all that had lead up to this moment and politely requesting the IP ranges or similar to get this to work.

When the support person got back to me they ignored my request and gave me the following:

1. Reboot Handset
2. Try to connect to a different network (to bypass ant firewall / restrictions to perform phone updates)
3. If the phone is updated, reconnect the phone to the original network
4. If issue persist, Try to reset the phone to factory reset / reprovision the and set to the new user extension

After then asking if I could factory restore without the password, the RC support person said that it wasn't possible and that I should contact the last System Admin to get the password...Fantastic.

Through further research I found a video that said to hold the speaker button down when powering on and it will allow me to reset the firmware. This worked but I needed a USB or TFTP to do so. I tried with a USB at first but it just failed so I tried TFTP using TFTPD64 where I got the following logs.

```
Connection received from 192.168.0.100 on port 1037 [07/06 09:35:11.770]
Read request for file <T48S.rom>. Mode octet [07/06 09:35:11.776]
File <T48S.rom> : error 2 in system call CreateFile The system cannot find the file specified. [07/06 09:35:11.776]
```


Bingo! I now see the issue with the firmware, the file is named incorectly so I change the file name and get the following.

```
Connection received from 192.168.0.100 on port 1037 [07/06 09:36:00.300]
Read request for file <T48S.rom>. Mode octet [07/06 09:36:00.303]
OACK: <timeout=5,blksize=1468,> [07/06 09:36:00.303]
Using local port 60958 [07/06 09:36:00.303]
Peer returns ERROR <File too large> -> aborting transfer [07/06 09:36:00.308]
Connection received from 192.168.0.100 on port 1041 [07/06 09:36:00.390]
Read request for file <T48S.bin>. Mode octet [07/06 09:36:00.394]
File <T48S.bin> : error 2 in system call CreateFile The system cannot find the file specified. [07/06 09:36:00.394]
Connection received from 192.168.0.100 on port 1061 [07/06 09:36:00.411]
Read request for file <T48S.rfs>. Mode octet [07/06 09:36:00.414]
File <T48S.rfs> : error 2 in system call CreateFile The system cannot find the file specified. [07/06 09:36:00.414]
```

I grabbed my USB and changed the file name and it then got stuck on the "Start Updating" screen. Whilst this happened I found a 3CX forum post that said the following: 

1. Press and hold Speaker button
2. Turn phone off and on with speaker button held down until you see IP details
3. When you see IP details press OK - It will say starting update (dont wait)
4. Restart the phone again as normal
5. Use GUI to update FW with default details

I then restarted the phone and got stuck at the "Welcome Initializing" screen.