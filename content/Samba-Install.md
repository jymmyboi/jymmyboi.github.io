Title: Trying to make an AD server with Debian
Date: 2025-11-07
Slug: ADwithDebian

I have been thinking of finally starting my homelab and now I actually work in a proper IT job, now is as good a time as ever.

Starting with the idea of an Active Directory domain, I cannot afford Windows. Debian it is.


### 11:30pm
I have no idea about partitioning with Debian so I am just sort of flying by the seat of my pants right now. It's taking it's sweet time installing the base system. I am pretty sure I am encrypting it right!

I am just sort of reading the prompts and guessing. FWIW this is just a test so that when I actually do spin up my proper physical server in my office. I can hit the ground running with a DC and then set up all my following containers/servers.

### 11:45pm
I have at the very least changed the network settings to be static.

### 11:50pm
Turns out you need to create a proper virtual switch to talk to the network. 

### 11:58pm
Changing back to dhcp to see what the VM gets on the 'external' switch.

### 12:04am
Weirdly the disk was still a source for installing. I have now since removed this. This could have been my issue with the switch. It might not have been a network issue at all. Will check now.

### 12:15am
Got IP working. Fuck me that took too long. Dumb cunt behaviour.

### 12:30am
Got all the packages installed. Will continue tomorrow night I think.