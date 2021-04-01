# Berzerk: Remake Platform Study

## Mason Porter-Brown & Marco Scanni
## 3/29/2021
## CMSC 335
## Prof. O’Hara


# Some Background
![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTQzmC3X8TKATcqgIFkqjVv1v-Bx6nX3rvtoDyN6PfGRxmHxA-dUdcxaqzTx_bcrLwy7f3EGKXp&usqp=CAc)


Berzerk is one of the most notable Atari 2600 games of all time not only because of its originality in gameplay and story, but also because of the eerie urban legends surrounding it. Playing as the last survivor of a group of earthlings on the planet “Mazeon”, the player’s goal is to survive as long as possible within endless mazes filled with robots called “Automazeons.” As one would expect of a game with this kind of story, it consists of nerve-racking gameplay that also requires quick thinking and precision. Part of the game’s lure, however, is it’s strange history, as urban legends have it that the original Berzerk, an arcade game released in 1980, was supposedly linked to a few bizarre deaths of those who have played it, making it the first ever video game to supposedly be linked to a player’s death. We recreated the game in Lua, attempting to not only accurately remake the movement and speed required to attain a high score, but also maintain the stressful yet skillful atmosphere from the original game.

## Berzerk’s Strange Legend
![](https://paper-attachments.dropbox.com/s_181572A71A868F323AA7F88F30BFC38095EE17D5A294F7DAD4D4BBE16746F963_1617316453344_file.jpeg)


Berzerk was first released in November of 1980 as an arcade game by Stern Electronics, and then was later released in August of 1982 on the Atari 2600. The timing of the Atari release was just months after one of the most ominous occurrences in gaming history. See, Berzerk gained much of its popularity through its supposed connection to the deaths of 3 different people. The most notable of the deaths was on April 3rd, 1982. Peter Bukowski of Calumet City, Illinois, a healthy 18 year old, decided to end his Saturday evening at Friar Tuck’s Game room. After earning two high scores on the Berzerk machine in the arcade, Peter went on to beat his own high score a third time. Shortly after entering his name into the machine for all to see, Peter backed away from the game and suddenly dropped to the floor. Ambulances took him to the hospital where he was pronounced dead on arrival. Of course, many then speculated that the game was in fact cursed, and Evil Otto, the game’s boss, killed the boy. 


![](https://paper-attachments.dropbox.com/s_181572A71A868F323AA7F88F30BFC38095EE17D5A294F7DAD4D4BBE16746F963_1617316424536_file.jpeg)


It was later found that he had a rare heart condition that would put his body in a deathly state after too much strenuous physical activity, and since he spent that day walking 4 miles through snow before getting to the arcade, he fell very sick by the time he got there, but decided to play Berzerk anyways. He died just a short time after. Despite the health implications, the gaming community did not stray from speculation, and created more controversy around the game, which ultimately solidified the game as one of the most eerie and cursed games of all time. We’d like to say that we excluded Evil Otto from our remake in order to minimize the deaths involved with our version, but the reality is that we didn’t have enough time.

# Remaking Berzerk
![](https://paper-attachments.dropbox.com/s_7B9D70DACA340B5A0F8EFE6663DA36A0A2905E4577324D17B3250601F9E9EEF6_1617315879076_Screen+Shot+2021-04-01+at+6.24.06+PM.png)


The original Berzerk was geared towards creating a nerve-wracking environment, and as the game progresses, it gets increasingly chaotic and difficult to predict. This, in combination with the games usage of randomness, contribute towards the player’s constant state of being on guard. For example, upon entering a new room, it is technically possible for the player to become wedged between two robots, in which case escaping death is impossible. Occurrences like those fill the player with a constant level of uncertainty in play.

In order to live up to Berzerk’s exciting and unpredictable nature, we implemented randomness and elements of the game that increase in difficulty over time. For example, the motion of the robots, which correlates to the shooting pattern’s of the robots, is heavily reliant on RNG. In the original game, it seemed that there was no particular order to the way in which the robots moved. 

## A Closer Look

The robots have only two states of movement. Either they are idle, or they move in order to line up with the player in either the X or Y axes. The robots can only shoot when they are aligned with the player in the X or Y axes. Because they can shoot while idle or while in motion, the player has to be aware of not only the robots that are in motion to line up, but also the robots that are idle that they may cross. 


    if pickRobot == true then
      for i= 1,#robots.arr do
        random = math.random(0,10)
        if random > 9 then
         robots.arr[i].move = true
        end
      end
      pickRobot = false
    end

This code snippet is used within the method that controls the motion of the robots. When a new set of robots are being chosen to seek, the robots array is looped through and a random number generator decides whether or not a robot will be in motion. It is important to note that the pickRobot boolean is necessary, because without it, the movement of the robots would be jittery and would not function well. The pickRobot boolean ensures that the currently selected robots will stay in motion until they line up with the player. The pickRobot boolean is reset to true again once a certain number of robots have been killed, thus ensuring that at any given moment, there are a fair number of robots in motion. 

Furthermore, in order to simulate the random generation of maps, we created 16 unique maps based on actual maps from the original game.  

![](https://lh3.googleusercontent.com/vKVYAhlyt2cz6pL4XK659njean1YD2r8KQ6Iu_6Opa1JSuEAnfAev8mCvEjnzUUG-ekJvpnAIteguhnnvuFkEk2nbZut1yPhQ0fil7nR06T38MWC5vlHR3ZDhtQcQwI7OX5WtCAX)


The game is set up so that neither the player nor the Robots can go through walls. Every room has the same 4 entrances/exits. This allows us to pick any of the maps from the pool when generating a new one. 

In order to generate a new map, the players X or Y value must exceed the limits of the walls. Thus, this can only occur when the player walks through any of the four openings on the walls of each map. The new map is generated in a method called limits(), which is responsible for wrapping the player around the X and Y axis when they exceed the limits.

Here is an example:

****
    if p1.x > 231 then
      p1.x = 0   
      mapGlobal.mapNum = math.random(0,15)  
      spawnRobots = true
    end

The Tic-80 window is 240 x 136 pixels. When the player exceeds the X boundary, the global indicator for the map is changed to a new random map from 0-15 (picking from 16 maps). The map is changed on the screen, and then the player appears to enter from the opposite side of the room. 

# Final Thoughts

Overall, our goal throughout this project was to capture the atmosphere of the Atari 2600 version of Berzerk, however we decided to improve the game in a few ways. Berzerk on the Atari 2600 had only 4 maps that it randomly switched between. We decided to improve this aspect of the game, and ended up creating 16 different maps to switch between. The player movement in the Atari version was also frustratingly slow at times, especially when moving along the Y-axis, so we decided to speed up the players movement slightly. Through preservation of sound, AI behavior, and better controls and map generation we feel that we were able to capture what made Berzerk so good during its time, while also adding to its replay-ability by improving on minor aspects that we felt could use tuning.


# Work Cited

“Berzerk - Atari - Atari 2600.” *AtariAge*, Stern Electronics, atariage.com/manual_html_page.php?SoftwareID=866. 

 Perrin, Steve. “Berzerk: The Killer Arcade Game?” *Little Bits of Gaming & Movies*, 30 Oct. 2019, littlebitsofgaming.com/2019/10/27/berzerk-the-killer-arcade-game/. 

# 
# 

