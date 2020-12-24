---
title: "Project Oceanside"
excerpt: "Simulator for heap operations in the Zelda 64 games"
layout: single
author_profile: true
header:
  teaser: https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Tsunami_by_hokusai_19th_century.jpg/525px-Tsunami_by_hokusai_19th_century.jpg
toc: true
toc_label: "Project Oceanside"
toc_icon: "cogs"
---
**[View the project page on Github](https://github.com/kyleburnette/Project-Oceanside)**  
## Overview

**Programming Languages:** C++  
**Tools:** Visual Studio  

Project Oceanside was my first large C++ project. It is a simulator for the actor heap data structure maintained by the engine of the Zelda games on the Nintendo 64. I prioritized the speed of C++ in this project over the simplicity of Python unlike the previous heap simulators that had been made.

The core data structure in Project Oceanside is a doubly linked list that is very similar to the game's actor heap. Elements in the list can be nodes, actors (data), actor overlays (code), or other assorted items like emulated memory leaks.

The Z64 games have the concept of a "scene," which is a collection of "rooms." There are exceptions, but by and large, each room contains a list of actors.

Users of Project Oceanside can build a JSON file containing the definition of a scene. This would include all of the rooms in the scene as well as every actor in each room. In addition, each actor has a few parameters, such as whether it can be destroyed (deallocated from the heap) or not. Some items in the game are more permanent objects, so configurations like these were required to simulate effectively.

Currently, Project Oceanside randomly generates permutations of actions that players can perform in the game. It then checks the location in the emulated memory of the relevant actors to see if they fulfill the required conditions.

## Motivation

In October 2019, a massive new glitch was found in [The Legend of Zelda: Ocarina of Time](https://en.wikipedia.org/wiki/The_Legend_of_Zelda:_Ocarina_of_Time) and [The Legend of Zelda: Majora's Mask](https://en.wikipedia.org/wiki/The_Legend_of_Zelda:_Majora%27s_Mask). This glitch eventually came to be known as Stale Reference Manipulation, and it completely changed the way that people [speedran](https://en.wikipedia.org/wiki/Speedrun) the games.

SRM is a use-after-free exploit. To make SRM useful for speedrunning, the community needed to find ways to manipulate the actor heap to make the dangling pointer line up with useful actors. Unfortunately, testing this by hand is extremely slow. This is where Project Oceanside came in. It is able to simulate actions in the game and check if the required conditions are met.

## Strengths and Improvements

First, I'd like to say that I am very proud of this project. It found many, many solutions to various situations in the games, so from a pure utility standpoint, it was a massive success. It was also my first larger-scale C++ project, so it being useful was very encouraging.

The program is fast and doesn't use very much memory at all (less than 2 MB). I made sure to construct objects using the JSON file once at the start of the program to avoid slow disk-reads. Also, the customizability of the JSON file gives the program a lot of flexibility.

There are, however, things I would fix if I were to start this project today. The project uses raw pointers throughout, and although I managed the memory effectively, I realize I should be using managed pointers. In addition, I did not get to implement all of the features that I wanted. There were planned features for customizable options in the JSON file that did not make it into the "final" project. Finally, the usability of the program is somewhat limited. A few options are hardcoded in to the program, and changing them requires the program to be recompiled. In the future I would like to pull all of this out to a settings file.

Another improvement I know can be made is the way permutations are tested. I went with a purely random approach,
but a more structured search may have been more useful. It's hard to say what sort of algorithm I should have used,
though, since the actions you can perform in the game have such chaotic effects on the end-state of the heap,
so random permutations weren't the worst idea, but I know it could be made more efficient.