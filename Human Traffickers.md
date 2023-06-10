![Hacktoria Logo](https://hacktoria.com/wp-content/uploads/2023/06/hacktoria-logo-v-wide-blue.png)
# Human Traffickers CTF

## Prologue
For Joso, Sehina, and Marek it was just another day sitting in the abandoned factory, awaiting the arrival of a new batch of transports, that will be delivered to various locations around the world. As part of a splinter cell of a large trafficking ring they were part of the day-to-day operations of transporting all to the next leg of their journey. They had once considered making a run for it, but having seen what happens to those that try to leave the organization they decided to just keep their heads down and do what is asked of them.

After a few hours waiting in the cold breezy shadows a transport pulled in and in the container were 26 new bodies to process. There were 26 in this group, which was an unusual quantity, they had never seen numbers this high. “I wonder why there is so many?”, mumbled Sehina. “No time for that now Sehina”, rasped Joso, “we need to process these lambs and arrange the transportation for the locations they are expected to be delivered to. You all know what happens if we are late! My leg has never been the same since that time (sigh) I do not want to go through that again, so snap out of it and let’s get this shit done!”

Marek noticed that many of the humans are in bad shape, malnourished and dehydrated and asked what should be done about it. Joso growled, “Just give them a couple of bottles of water each, it isn’t our job to look after them, we just need to move them on quickly before the next batch arrives. Come on, get on with it!!!”

Asshole muttered Marek under his breath, luckily Joso didn’t hear him, however Sehina did, but just chuckled to herself. They are well into the processing of the batch when Joso’s phone goes off. The person on the line says, “Get this lot processed and clear the fuck out, pronto. I have word that you are going to be raided. Whatever happens do not get arrested, if you do, you know what will happen to you? We can reach you anywhere, anytime, just you remember that.”
Marek asks what the call was about. Joso’s face was very pale as he said, “We need to clear out of here very quickly!!! We are apparently going to be raided. As soon as they are all loaded onto the transport destroy everything and proceed with the contingency plan. You remember the plan, right?

“This shit just got real” said Marek to Seniha, “I hope you haven’t forgotten the plan that Joso laid out for this exact scenario.” “No of course not”, she said. “Okay. Go now and I will finish up here. See you soon, take care of yourself.”
The transport rolls off into the dark and Marek stays behind to burn everything, leaving nothing but a smouldering mess on the floor. “Good luck finding anything that ties this place to us pigs!” Marek laughed as he walked into the darkness. “I’m outta here…”

## Briefing
![Briefing](https://hacktoria.com/wp-content/uploads/2023/05/news-report.jpg)
Greetings, Special Agent K.

We have a very urgent matter handed to us from the joint task force, MET Police and Interpol. They have joined forces to bring down a notorious human trafficking gang. Unfortunately, their sting operation went side-ways in a big way. Somehow the gang knew about the sting and managed to clear out their operations before the teams arrived, leaving them with nothing but their dicks in their hands!

One of the task force agents did find a very battered Computer Hard Drive, that they handed to their Cyber CSI team to try and recover anything that could help with the investigation, however all they were able to recover was a password protected “Database Backup” file and a file named “message.gpg”. This is where Hacktoria comes in, I do not need to tell you how delicate and important this is, egg on face springs to mind, so let’s make some sense of this and see if you can retrieve any useful data to aid in tracking down the whereabouts of the gang.
As our best Agent at this Data Forensics stuff this falls with you.

It is the belief of the task force analysts that the gang will retreat to one of their “Safe-Houses”, re-grouping and letting the heat die down before resuming operations. We need to locate the safe house before they are in the wind for good.

As always, Special Agent K. The Contract is yours, if you choose to accept.

## Materials

[Download the FLAGFILE](https://hacktoria.com/wp-content/contracts/flags/2023/flagfile-human-traffickers.zip)

[Download the MATERIAL](https://hacktoria.com/wp-content/contracts/items/2023/human-traffickers/mission-files-01-human-traffickers.zip)

The password for the flagfile is the answer to the final step of the mission, as per the format stated below.

Names as per Google Maps and English, using lowercase letters and hyphens only.

| Password Format | country-name-city-name-street-name |
|---|---|
|Password Example | dominican-republic-la-romana-minerva-miabal |

## Walktrough
### $\color{cyan}{Mission\ 1}$

Files in the ZIP:
* CHECKSUMS
* INSTRUCTIONS.txt
* message.gpg (password protected)
* database_backup.zip

After reading the instructions, out first task is to verify the CHECKSUM of the files presented and then geolocate the "safe house" location.

### $\color{cyan}{Files}$

![Imgur](https://i.imgur.com/7VB2icb.png)

![Imgur](https://i.imgur.com/dG25iA3.png)

### $\color{cyan}{Cracking\ Them\ Open}$

Now that the files have been verified lets dig in.

Well.. Not so fast, we dont have the passwords and we cant access them. So lets use the handy tool called [John The Ripper](https://github.com/openwall/john)

We can install it by pulling it from the github above, I had some problems running it but I figured it out.

Here is the way to install it:

```bash
git clone "https://github.com/magnumripper/JohnTheRipper.git"
cd JohnTheRipper/src 
./configure 
cd .. && cd run
sudo make
cd .. && cd src
sudo make -s clean && sudo make -sj4 
```

After this it should work fine, at least it worked for me. Now back to the mission.

Lets begin with message.gpg, first we will need to generate a `hash`, we do this by running the command bellow.

```bash
./john/run/gpg2john message.gpg > gpg.hash
```

Now we can bruteforce the hash with out trusty password list I found on github. [Link](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwj0_peR2bf_AhUUnVwKHRhZAW8QFnoECA4QAQ&url=https%3A%2F%2Fgithub.com%2Fbrannondorsey%2Fnaive-hashcat%2Freleases%2Fdownload%2Fdata%2Frockyou.txt&usg=AOvVaw3snAERl1mU6Ccr4WFEazBd)

```bash
./john/run/john --wordlist=rockyou.txt gpg.hash
```

![Imgur](https://i.imgur.com/uDJ8RCA.png)

After a few minutes we have the password. `nopassword`

We now have one password of two. Lets get the second one the same way.

```bash
./john/run/gpg2john database_backup.zip > zip.hash
./john/run/john --wordlist=rockyou.txt zip.hash
```
And after a few minutes we have our second password. `995511335577_y`

![Imgur](https://i.imgur.com/qUZVYWn.png)

### $\color{cyan}{Exploration}$

Equiped with all the password we need so far lets explore the files.

Oprning the message first with the following command.

```bash
gpg -d message.gpg > message.txt
```

![Imgur](https://i.imgur.com/GWdcbk4.png)

Reading trough the decripted file we can gather a bit of information. Joso seems to be sending a picture to Seniha, and that picture is the one we need. But it is all text? Not really an image is it? 

Don't worry! Thats easy! We can just use [base64.guru](https://base64.guru/converter/decode/image/png) and convert it back to a png file we can download.

![Imgur](https://i.imgur.com/axNPImN.png)

But sadly the image doesn't really tell us much. By the building layout and structure we can assume it is somewhere in the United Kingdom. 

Lets see what is inside the database. Lets set up all the things we need to explore it.

```bash
sudo apt install mysql-client mysql-service
sudo service mysql start
sudo mysql
```

We first create a blank sql table.

```bash
USE mysql;

CREATE DATABASE assets;
CREATE DATABASE backout;
CREATE DATABASE cells;
CREATE DATABASE hermitage;
CREATE DATABASE insiders;
CREATE DATABASE transport;
CREATE DATABASE routes;

SHOW DATABASES;
exit
```

With the layout created we can now fill it up by running the following command.

```bash
sudo mysql assets < assets_database.sql
# And we do this for all of them
```

As we can see from the names of the databases, there is ont that sticks out. Thats right its `hermitage` as that is another word for hideout or a "Safe House".

```bash
sudo mysql
# Lets pull up the database we have created
USE hermitage;
# and lets see what it contains
SHOW TABLES;
SELECT * FROM house_locales;
exit
```
It should show like this.

![image](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045818668.png?raw=true)

Those are just random three words! How can we use that to find them? 

Well thank you for asking. Those three random words are probably used on [What3Words](https://what3words.com).

After going trough all of them we finally got the right one, and we got the adress we needed. 

![image](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045825705.png?raw=true)

As the instructions specified we need to write down the address in a specivic way. 
`road-name-village`
So we end up with:
> swan-view-pulborough

And if we turn it to MD5 Hash we get the link we need. We can do this with the following command.

```bash
echo -n swan-view-pulborough | md5sum
# 0796204fd7c432598f2d55911d97cdf4
```

Leading to:
> bit.ly/0796204fd7c432598f2d55911d97cdf4

### $\color{cyan}{Mission\ 2}$

The new zip file contains the following files:
* INSTRUCTIONS.txt
* locale-1.jpeg
* locale-2.jpeg
* locale-3.jpeg
* locale-4.jpeg
* locale-5.jpeg

### Location-1:

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045842689.png?raw=true)

What can we see? Some data points:
* Electric box
* Advert, seems new
* A parking restriction sign

And the position of them is important. So lets reverse image search it.

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045850128.png?raw=true)

BINGO! The first result looks just like out image, but a different graffiti at the time, lets take a look.

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045630293.png?raw=true)

Looks to be it, lets go to Google Maps and find it.

![maps](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045521929.png?raw=true)
> 29 New Inn Yard, London, England
