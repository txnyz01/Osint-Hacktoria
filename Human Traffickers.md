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

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045722413.png?raw=true)

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045729519.png?raw=true)

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

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045738460.png?raw=true)

After a few minutes we have the password. `nopassword`

We now have one password of two. Lets get the second one the same way.

```bash
./john/run/gpg2john database_backup.zip > zip.hash
./john/run/john --wordlist=rockyou.txt zip.hash
```
And after a few minutes we have our second password. `995511335577_y`

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045746514.png?raw=true)

### $\color{cyan}{Exploration}$

Equiped with all the password we need so far lets explore the files.

Oprning the message first with the following command.

```bash
gpg -d message.gpg > message.txt
```

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045755337.png?raw=true)

Reading trough the decripted file we can gather a bit of information. Joso seems to be sending a picture to Seniha, and that picture is the one we need. But it is all text? Not really an image is it? 

Don't worry! Thats easy! We can just use [base64.guru](https://base64.guru/converter/decode/image/png) and convert it back to a png file we can download.

![Imgur](https://github.com/txnyz01/imges/blob/main/image_2023-06-10_045808541.png?raw=true)

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

### Location-2:

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/bd4800a0-4380-492e-bf9a-3f719358f716)

What do we see:
  * There is a T-junction
  * Road name: `Puesta Del Sol Drive`
  * Traffic light
  * Its deserty
  * The buildings look american. So chances are its in America.

Well lets type the road in Google maps and see what we can find.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/93eb53b4-c451-45a5-a46e-f1b705b4c89b)

Looking trough all of them with terrain view, the only promising one was in `Victorville, CA` as it matched the deserty type terrain.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/9fe8aa2c-22a3-4475-b858-b990e03aff48)

Lets check out these three junctions. We are looking for a sign, trafficlight around those points of interest.

![Screenshot from 2023-06-10 01-40-04](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/2a1b8dd4-cb53-4953-a71a-ceaa29a6b5a7)
> 16062 Village Dr, Victorville, California

### Location-3:

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/006d036e-c914-48e2-8a77-442b721a2a83)

What do we see:
  * Two roadsigns `Woburn Way` & `Parkfield Road`
  * We are mostlikely in Australia as there is a flag, not always but chances are high.
  * And we are on `Parkfield Road` as one of the signs is pointing towards `Woburn Way`

Lets take a look at Google Maps. It was the first result.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/fc76dc94-09e4-45c7-9a5b-aed0adc8ac83)

Two points of interest.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/41f079dc-bad4-46be-8539-87ddb54e89ca)

![Screenshot from 2023-06-10 01-50-43](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/23d7d36a-bc9d-4cda-af90-4b5da2d86d64)
> 30 Parkfield Rd, Kelmscott, Australia

### Location-4:

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/8b154bfe-1a61-4fae-bd6c-793061aba51d)

What do we see:
  * A realtor sign with the name `Lee Ivans` & phone number `250-575-5455`
  * End of the road

We can Google the phone to see where abouts it is.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/41fec2d1-1a89-4b12-af7c-b69942f36820)

So it is in `Kelowna, Canada`.

I looked trough their facebook and website and I could not find the neighbouring house at all. So the only option left is to go trough all hilly areas and look at the ends of the roads.

![Screenshot from 2023-06-10 02-17-47](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/0c61e3e9-b0c6-4ce4-8b34-99a259abfe2c)
> 601 Horn Cres, Kelowna, Canada

### Location-5:

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/4fe2beb2-9639-42fd-8e19-5584ddacb3f6)

What do we see:
  * Road name starting with Bea
  * Van with `Pro Roofing` written on it
  * Left hand driving
  * Square stop sign

So we are looking for a english speaking, left hand driving country who's road stop signs are in a square font.

Could be, Australia, New Zealand, Singapore or the UK. By Googling each country's road stop sings, we can find out it is actually New Zealand. So lets check out `pro roofing new zealand` in Google.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/2e7f5195-01b6-4410-b6df-7607b350a0e0)

So we are looking for a road starting with `Bea` in `Aukland, New Zealand`.

![Screenshot from 2023-06-10 02-31-27](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/d91d825d-c9b0-4aa4-806c-b849025091d6)
> 39 Beaconsfield St, Auckland, New Zealand

Now as per the instructions, we need to combine the first 4 letters of each streetname, to get our code.
> newivillparkhornbeac

Turning it into MD5 Hash, we get our link.
> 5f44dd447ef33db456abf0ff9a3931f4 | bit.ly/5f44dd447ef33db456abf0ff9a3931f4

### $\color{cyan}{Mission\ 3}$

Files:
  * CHECKSUM
  * INSTRUCTIONS.txt
  * Password_Vault.kdbx.7z (password protected)

`kdbx` is a keepass file, so we need to install that to be able to continue. We can do that with the following command.

```bash
sudo apt install keepassxc
```

Confirming the CHECKSUM to make sure the file is fine, now we need to bruteforce the password, like we did with the previouse files.

```bash
# First we hash it
./john/run/keepass2john Password_Vault.kdbx > keepass.hash
# Now we brute force it
./john/run/john --wordlist=rockyou.txt keepass.hash
```

And we get the password `shadow1`. Lets login with our newly aquired password and dig deeper.

![Screenshot from 2023-06-10 02-55-44](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/ff609308-346b-432d-950e-5dbe69e80888)

There seem to be a lot of social media accounts, lets take a look at all of them.

The only one that seemed to be still active is twitter, and there is quite a bit of information.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/d84d6a05-6cea-436d-8eb0-7c4a62bb4d69)

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/f0723f6d-1498-4ab9-9c72-afe864c6695c)

Lets see what we got.
 * It is in `South Africa`
 * By the beach
 * There is a sign with a `circular logo` and we can make out `table 210`

Lets google `table 210 in south africa`.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/424d3a04-644b-4ff6-b632-4e0aecd7d751)

Would you look at that. `Alex/Kenton 210` in `Kenton-on-Sea`.

![Screenshot from 2023-06-10 03-04-25](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/292ebd23-da27-4304-908d-8be497637e87)
> Round Table Alex/Kenton 210 Club

As per the instructions, we need to format our findings and hash them, to get our next mission.
> south-africa-kenton-on-sea | e959dd95e27b6cf16fef9573be43a22d | bit.ly/e959dd95e27b6cf16fef9573be43a22d

### $\color{cyan}{Mission\ 4:\ Finale}$

Files: 
 * INSTRUCTIONS.txt
 * captives-location.png
 * ransome-note.png

Reading trough the note tells us that we need to find the location on the other image. So lets take a look and figure it out.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/4029142d-8db3-4a1a-a3d0-111867fda3c9)

Instantly we see a logo we can find. Lets use Google Lens to find it. Sometime Google Lens doesnt quite pick out what we want it to, like in this case it thought the logo was just text saying `t`. So I made a zoomed in screenshot of the logo and used that.

![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/b4d481ba-dbe7-4a5a-b2ec-bb14e6704a28)

That it! Lets look at their site and see if we can find store locations.

![Screenshot from 2023-06-10 03-15-03](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/eacbf987-088e-419e-baf3-d22236ad4ef7)

Now the tidious part, checking each location until we find the right one. But we found it.

![Screenshot from 2023-06-10 03-18-28](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/dd22719b-783c-4a09-b045-b670f28956d3)
![image](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/d21fc543-94bd-437a-9a7f-9dc8809f575c)
> Brickfield Rd, Cape Town, South Africa

Formated
> south-africa-cape-town-brickfield-rd

If we hash it we get the password for the flag. 

![Screenshot from 2023-06-10 03-20-29](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/4c391671-d0ec-4406-b2a0-2e50c6fcc470)

![Screenshot_3](https://github.com/txnyz01/Osint-Hacktoria/assets/33939134/ea7f58fd-902b-46bc-b473-10dae9b6bc2a)
