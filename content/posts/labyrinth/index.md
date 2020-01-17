---
title: "Procedurally Generated Maze Game Fun"
date: 2019-03-15T21:28:41-04:00
description: "Everyone loves a good maze but, how might a computer generate one. Well, you can generate a maze using a simple algorithm called recursive backtracking. And yes, the algorithm is as cool as its name."
categories: ["tech"]
dropCap: true
displayInMenu: false
displayInList: true
draft: true
resources:
- name: featuredImage
  src: "labyrinthMaze.png"
  params:
    description: "The maze game window with the solution to the maze painted over it"
---

Back in high school, I made a maze game for a local programming competition. The mazes are generated procedurally, it keeps score, and it can generate mazes of different sizes. Had my high school self thought to document and package the game sooner than a half hour before the deadline, I think it would have done well in the competition. Anyways, the maze game runs anywhere with Java and is built with a number of cool algorithms.

## Procedural Maze Generation
Each maze is procedurally generated using an algorithm called recursive backtracking. It is one of the more simple maze generation algorithms but it produces great results. The algorithm can be broken down into three basic steps:

1. Pick a starting point and begin a random walk, keeping track of where you have been
2. Once you hit a dead end, backtrack until there is room to begin your random walk again
3. Repeat until there is nowhere left to walk and you have backtracked to the origin



## Verifying That the Maze Is Solvable
Recursive backtracking provides no guarantee that the maze is solvable. So, before the game lets the user attempt an impossible maze, it verifies that the maze has a solution using an efficient path finding algorithm called A* (pronounced "A Star").


{{<image src="labyrinth.png" alt="Labyrinth maze game window">}}