---
layout: default
---

# Individual Projects

### Sudoku Solver

> Python, tkinter

A sudoku game created in python using the modules tkinter and numpy.

This was my first time using tkinter, so it was interesting exploring the module to create a GUI. Numpy was used to create a matrix representation of the sudoku board.

Aside from the grid, the interface consists of four buttons: Solve, New Board, Reset Board, and Solve.

The solve button makes use of the [recursive backtracking algorithm](https://en.wikipedia.org/wiki/Sudoku_solving_algorithms#Backtracking), trying numbers 1-9. If the board cannot be solved then the algorithm goes back and tries different numbers until the board is solved.

In the future I would create a more sophisticated board generating algorithm, as the current one ends up with trends such as 1,2,3,4,5,6,7,8,9 in the first line and consecutive numbers appearing often.

<img src="./sudoku board.png">

### Sorting Algorithm Visualiser (Unfinished)

I was interested in the computational theory behind sorting algorithms, as well as how these algorithms could be effectively visualised. I researched insertion sort, merge sort, selection sort, quick sort and pancake sort. From this research I compiled a document of [my findings](https://nbviewer.org/github/moaylesbury/Sorting-Algorithm-Visualiser/blob/master/Research/Sorting%20Algorithm%20Research.pdf), including how the algorithms work and their time complexities.

To get an idea of how these algorithms actually work in practice, I implemented their functionalities in Python and tried different test cases. From here I moved onto creating a web applciation using React. This was my first experience creating visual elements in JavaScript that move as internal data changes 

### Bright Internship Work Sample

> Python

During the internship experience with the Bright Network, I received a seminar from Google employees who spoke about the company and the technology industry. They presented us with a brief for a task to implement a command line application with Python that replicates a number of YouTube core functionalities by parsing user commands through the terminal. This task was given a strict deadline of one day, requiring fast implementation with a focus on object-oriented, readable code.

I implemented a video libary class to fetch specific videos, or all videos (from an internal list created by parsing a CSV file). This class was heavily relied on by the main implementation, the video player class. The video player class allowed for playing, pausing, stopping and continuing videos, playlist creation, deletion and viewing, as well as video searching and flagging. Each function relied upon other functions and made sure conditions were in place before carrying out certain actions. This includes making sure a video is playing if the user tries to resume, checking the amount of flags on a video before playing, and ensuring playlists exist beore adding videos to them.

## Blender Image Composition with Physically Based Rendering and Photoshop

> Blender, pbrt-v3, Photoshop

The goal of this project was to create a scene of carefully chosen and placed objects, and create a realistic looking composition with the following 3D models: a [doom combat scene](https://www.artec3d.com/3d-models/doom-combat-scene), a [bovine heart](https://www.artec3d.com/3d-models/bovine-heart), a [sea shell](https://www.artec3d.com/3d-models/sea-shell), a [pipe bend](https://www.artec3d.com/3d-models/pipe-bend), and a [dragon](https://www.artec3d.com/3d-models/dragon). These were all models I chose as I found them to be interesting.

I began by taking the original image 

<img src="./cg/original file.jpg">

Then, in order to cast realistic shadows and lighting, I measured the relative distance between all objects and shelf lengths, as well as the distance of the light source. All of these measurements were then used to model the scene in Blender. Images were also taken in order to texture the objects. Then the models were imported into Blender and placed, and I created textures and exported the scene using the [pbrt-v3](https://pbrt.org/users-guide) rendering system. Terminal was then used with pbrt in order to render the scene, which produced the following image.

<img src="./cg/colour corrected render.png">

This render allowed scene specific shadows and highlights to be formed. From here, the original image and the rendered image were taken into Photoshop, and a composite was created. Colour correcting and touchups were applied in order to achieve the best possible final product.

<img src="./cg/final composite image.png">
