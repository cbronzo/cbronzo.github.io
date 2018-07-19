---
layout: post
title:      "Building My First CLI Application from Scratch!"
date:       2018-07-18 20:40:27 -0400
permalink:  building_my_first_cli_application_from_scratch
---

This week, I built my own custom Command Line Interface (CLI) Application that pulled data from an external website! I felt very confident and was thinking I could finish it in just a few hours -- but  I grossly underestimaed how long it would take me for my first time! I even created a local repository when I didn't need to...apparentley we haven't been taught that just yet. No wonder it took me so long....


The building of it wasn't necessarily the hard part, the de-bugging was! I would fix one thing, and it would break another....or I would get so close to completion and then realize I need to refactor an entire set of code, and by doing that it would affect other related pieces. Sometimes I was missing something as simple as a comma and would spend forever grooming through lines of code, only for it to be such a simple fix. Haha, I learned so much by building this CLI but mostly I learned about the actual process of coding, and how much more of a mental game it truly is.

**
What does my CLI do?**

I scraped the data from "[Orange County's 21 Hottest Restaurants, Spring 2018](https://la.eater.com/maps/orange-county-restaurants-santa-ana-irvine-dana-point-newport-beach-costa-mesa)" on Eater LA's site. The page lists the restaurants as well as the details of the said restaurant, and so I wanted my CLI to give a user the list of these restarurants, and then allow them to choose which ones they would like to learn more about.

When the gem is run, it first outputs:

```
Welcome to 'Orange County's 21 Hottest Restaurants' Guide! 
Type 'guide' to see the list of OC Restaurants.
Otherwise, type 'exit'.

```

Then a user can either type 'guide' or 'exit.' If they type guide, they'll see:

```
YUM! Here are 21 of the OC's best & hottest restaurants! Dig in!
Please select a restaurant by entering its number (1-21) to learn more!
To exit, type 'exit'.
1. Portos Bakery & Cafe
2. Pour Vida
3. Garlic and Chives
4. Burritos La Palma - Santa Ana
5. Taco Maria
6. Vaca
7. Mesa
8. Blackmarket Bakery
9. Oo Toro Sushi
10. Bluegold & LSXO
11. SeaSalt Woodfire Grill
12. Puesto
13. SOCIAL
14. Lido Bottle Works
15. Pizzeria Sapori
16. The Royal Hen
17. Ironwood
18. Tackle Box Local Grub Shack
19. Marche Moderne
20. Hendrix Restaurant and Bar
21. Bourbon Steak Orange County
```

They then have the option to select a resturant by number, or exit. And if they type an invalid response here, or anywhere in the CLI, they will get an error message to try again.

Say, a user enters '15' for Pizzeria Sapori (which I can see from my apartment - woohoo), this is what they would see:

```
Below, you will find the details for Pizzeria Sapori.
Description: The old-school Italian destination Sapori got a nice boost with the opening of Pizzeria Sapori next door. The attached casual offshoot features blistery pies and plenty to drink, making it a draw for marina types and locals looking to relax while still dining well.
Address: 1080 Bayside Dr Newport Beach, CA 92660
Phone Number: (949) 644-4220
Website: http://www.saporinb.com/
To select another restaurant, please enter its number, or enter 'back' to go back to the guide.
To exit, type 'exit'.

```

Some of the resturant details were not listed in the Eater LA article, so in that case, I had to make a conditional where if nothing was provided, it would put "Sorry, no __  available."

From here, they can just keep entering numbers to see other restaurants' info or they can hit 'back' or 'exit.' 

It may seem simple, and rudimentary but a lot of logic (and time) went into creating this.

**The Code:**



