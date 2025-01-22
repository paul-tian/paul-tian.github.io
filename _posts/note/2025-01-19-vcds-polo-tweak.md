---
layout: post
title: Using VCDS for my POLO GTI 6C tweaks
categories: [Life]
description: 
keywords: vcds, volkswagen
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---


# Using VCDS for my POLO GTI 6C tweaks

Finally I got my own VCDS, which emptied my wallet LOL. I used it to tune my GTI so it could fit my requirements better.
This is a note for myself, and for those who might would like to have the same tweaks.

## Disabling Soundaktor

The solution is either directed or inspired by [this post](https://www.golfmk7.com/forums/index.php?threads/soundaktor-adjustment-via-vcds.318875/).

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "A9 (Structure Borne Sound)"
* Choose "Adaption 10"
* At Channel drop down choose "Volume of structure borne noise actuator"
* Enter a new value in the box (0= off; 100 = full volume)
* Close out with "do it"
* OK the code that is suggested by VCDS


## Disabling Hill Assistant

The solution is either directed or inspired by [this post](https://www.golfmk6.com/forums/index.php?threads/zvps-2012-vw-gti.225913/page-13#post-4614564).

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "03 (ABS Brakes)"
* Choose "Adaption 10"
* At Channel drop down choose "Hill-Start assistant"
* Choose a value in the drop down ("not activated" for disable this function)
* Close out with "do it"
* OK the code that is suggested by VCDS

## Disablingthe safety disclaimer message

The solution is either directed or inspired by [this post](https://forum.obdeleven.com/thread/18516/disable-safety-follow-traffic-regul).

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "5F (Information Electr.)"
* Choose "Long Coding", then "Long Coding Helper" and accept online helper (for adding description)
* Go for Byte 23
* disable the "initial disclaimer" then close the coding helper window
* Close out with "do it"
* OK the code that is suggested by VCDS

## DRIVING SCHOOL MODE 

The solution is either directed or inspired by [this post](https://forums.ross-tech.com/index.php?threads/11523/).

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "5F (Information Electr.)"
* Choose "Adaption 10"
* Select Vehicle function list CAN-Driving_school (it's about 40% down the channel list) - change to “Available”
* Select Vehicle menu operation-menu_ display_ driving_ school (it's about 75% down the channel list) - change to “activated”
* Select Vehicle menu operation-menu _display _driving _school _over _threshold _high (it's about 75% down the channel list) - change to “activated”
* Close out with "do it"
* OK the code that is suggested by VCDS

Driving School Mode can be accessed as follows:

Press "CAR"
Press "view" on the bottom LHS of the screen
Select "driving data" option from the list
Repeatedly press either the "<" or ">" symbols until the "Driving school mode" screen appears.


##  Auto lock-unlock after start and stop (done)

The solution is either directed or inspired by [this post](https://www.youtube.com/watch?v=d7_QbCYHb8g).

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "09 (Cent. Elect.)"
* Choose "Adaption 10"
* Select "automatisches Verriegeln bei Geschwindigkeit"  - change to "active", this will enable the auto-lock when speed is higher than 15kph
* Select "-automatisches Entriegeln" - change to "active", this will enable the auto-unlock when open the door or pull out the key when stop the engine
* Select "Menuesteuerung ZV Autolock-Unlock" - change to "Adjustable", this will enable some adjustable settings for related functions in MIB2 settings
* Close out with "do it"
* OK the code that is suggested by VCDS


## LOCK ACOUSTIC CONFIRMATION (done)

The solution is either directed or inspired by [this post](https://www.youtube.com/watch?v=LOMha8pPNWA).

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "09 (Cent. Elect.)"
* Choose "Adaption 10"
* Select "Akustische Rueckmeldung entriegeln"  - change to "active", this will enable the beep sound when unlock the car (if you don't like it you can keep it "not active")
* Select "Akustische Rueckmeldung verriegeln" - change to "active", this will enable the beep sound when lock the car
* Select "Menuesteuerung Akustische Rueckmeldung" - change to "active", this will enable some adjustable settings for related functions in MIB2 settings
* Close out with "do it"
* OK the code that is suggested by VCDS



## TMC/TP

The solution is either directed or inspired by [this post](https://www.vwwatercooled.com.au/forums/f238/no-tp-122433-3.html).

However, TMC also needs the corresponding map to work, but VW stops providing the TMC in the maps in some regions.




Select Control Unit 5F (Information Electrical)
Adaptations
Change Fee Based Traffic Information (TMC) value to ‘1024’
long coding Byte 13, enable bit 0 and bit 3.


Next, in the normal radio settings, enable ‘Traffic Programme (TP)’ and RDS


## Automatic Coming Home Lights

The solution is either directed or inspired by [this post](https://forum.obdeleven.com/thread/734/coming-home-lights).

If your car has coming home/leaving home (CH/LH) enabled but you can only see the leaving home light (ignite when you unlock),
you can try to manually turn on the coming home light to test

* keep the light settings at auto
* turn off the engine and take out the key
* pull the light stalk towards driver position (the temporary high beam on) once
* open the dirver door
Then you should see the front lights ignite, this is the manual way to turn on CH lights.

To switch to automatic turn on, 
From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "09 (Cent. Elect.)"
* Choose "Adaption 10"
* Select "Komfortbeleuchtung-Coming Home Verbaustatus"  - change from manual to automatc
* Close out with "do it"
* OK the code that is suggested by VCDS

Notict the CH/LH should need the light switch at auto to work.

## Enable Off Road Display

The solution is either directed or inspired by [this post](https://forums.ross-tech.com/index.php?threads/5822/).

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "5F (Information Electr.)"
* Choose "Adaption 10"
* Select "Car_Function_Adaptations_Gen2-menu_display_compass"  - change to "active"
* Select "Car_Function_Adaptations_Gen2-menu_display_compass_over_threshold_high"  - change to "active"
* Select "Car_Function_List_BAP_Gen2-compass_0x15"  - change to "active"
* Close out with "do it"
* OK the code that is suggested by VCDS

To show this display the method is the same as driving school
Notice each of the gauge can be swiped up and down for different functions


##  Adjustable comfort opening

The solution is either directed or inspired by [this post](https://forums.ross-tech.com/index.php?threads/24983/).

9- Central electronics
Adaptations
2-Access control 2- comfort opening > active
3- Access control 3- comfort closing > active
Key fob…
7- Access control 2- funk komfort oeffnen > active
6- Access control 2- funk komfort schilessen > active
Menu
27 access control 2- Menuesteuerung komfortbedienung einstellbar >adjustableKessy
20- access control 2-Kessy komfort schilessen > active

## Keep window operable after off

The solution is either directed or inspired by [this post](https://uk-polos.net/viewtopic.php?t=78709).


select Control Units

then select Central Electrics

in Security Access enter 31347

Select Adaptations

Scroll down to the bottom of the list and select ZV Komfort

look for entry Freigabenachlauf_FH_bei_Tueroeffnen and Tap that line (it will probably state 'Active)

Select Not Active



## Show volume to be replenished

The solution is either directed or inspired by [this post](https://www.youtube.com/watch?v=vSFYRPwo0rw).

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "17 (Instruments)"
* Choose "Long Coding", then "Long Coding Helper" and accept online helper (for adding description)
* Go for Byte 10
* enable the "display volume to be replenished" then close the coding helper window
* Close out with "do it"
* OK the code that is suggested by VCDS


## Adjust steering wheel assisting power

From VCDS main menu: 
* Choose "Select Control Module"
* Choose "Installed" -> "44 (Steering Assist)"
* Choose "Adaption 10"
* Select "Support power" 
* choose a new value to fit the requirements
* Close out with "do it"
* OK the code that is suggested by VCDS


[homepage](/)
