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

> React, JavaScript

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

## Netflix Clone using AstraDB and GraphQL

> AstraDB, GraphQL

This project was part of the DataStax development series. I created an AstraDB instance, and used GraphQL to create a movie and genre tabls, and populate it with a CSV dataset. This database was then conected to Netlify and the instance was deployed.

<img src="./ui.png">

## Implementation of Basic, Stop-And-Wait, Go-Back-N, and Selective Repeat Data Transmission Protocols

> Linux, virtual machine, Python, Vagrant

I used the Linux VM traffic control utility to emulate a link between two communicating processes; a sender and receiver. The TC utility allowed the kernel packet scheduler to emulate packet delay and loss, and limit bandwidth for UDP or TCP applications. Because sender and receiver processes for this are within the same VM, they communicate with each other through the loopback interface (lo).

This project implemented four protocol frameworks; basic transmission under ideal conditions, stop-and-wait, go-back-N, and selective repeat. There are two Python files for each respective protocol, named Sender1.py - Sender4.py and Receiver1.py - Receiver4.py.

### Basic transmission

This implementation allows transferring a file from the sender to the receiver on localhost over UDP as a sequence of small messages with 1KB maximum payload using the loopback interface, configured with 10Mbps bandwidth, 5ms one-way propagation delay (10ms round-trip) and 0% packet loss rate. Each 1KB message has a 3 byte header; a 2 byte sequence number and 1 byte end-of-file flag indicating the last message. It simply sends messages sequentially.

The receiver stores the transmitted data (removing headers) into a local file. The transfer is successful if the sent and received files match on a binary level, which was checked using the diff command.

{% highlight python %}python3 Sender1.py <RemoteHost IP> <Port> <Filename>{% endhighlight %}

{% highlight python %}python3 Receiver1.py <Port> <Filename>{% endhighlight %}

### Stop-and-wait

The Sender1 and Receiver1 programs were extended to implement the [Stop-and-Wait](https://www.isi.edu/nsnam/DIRECTED_RESEARCH/DR_HYUNAH/D-Research/stop-n-wait.html#:~:text=%22stop%2Dn%2Dwait%22,under%20unreliable%20packet%20delivery%20system.&text=After%20transmitting%20one%20packet%2C%20the,before%20transmitting%20the%20next%20one.) protocol. This code was based upon two finite state machines; one for the sender and one for the receiver. This protocol introduces the acknowledgement (ACK) message used by the receiver to inform the sender that a message had been received. ACK messages are 2 bytes, holding a sequence number. Duplicates were deleted at the receiver end using sequence numbers. A 5% packet loss rate was used, with the rest of TC configuration parameters as
before. The sender outputs the number of retransmissions and throughput (in Kbytes/second). 

{% highlight python %}python3 Sender2.py <RemoteHost IP> <Port> <Filename> <RetryTimeout (ms)>{% endhighlight %}
{% highlight python %}python3 Receiver2.py <Port> <Filename>{% endhighlight %}





### Go-back-N

Sender2.py and Receiver2.py from Part 2 were extended to implement the [Go-Back-N](https://www.baeldung.com/cs/networking-go-back-n-protocol) protocol, allowing the sender window size to be greater than 1.

Different window sizes were experimented with (increasing in powers of 2 starting from 1), as well as different one-way propagation delay values (5ms, 25ms and 100ms). 10Mbps bandwidth and 5% packet loss rate in each direction were used for the other parameters.

{% highlight python %}
python3 Sender3.py <RemoteHost IP> <Port> <Filename> <RetryTimeout (ms)>
<WindowSize>
{% endhighlight %}

{% highlight python %} python3 Receiver3.py <Port> <Filename>{% endhighlight %}

### Selective repeat

Extended Sender3.py and Receiver3.py to implement the [Selective Repeat](https://www.geeksforgeeks.org/sliding-window-protocol-set-3-selective-repeat/) protocol. The sender must output throughput (in Kbytes/second).

The TC link parameters were set to 10Mbps bandwidth, 25ms one-way propagation delay and 5% packet loss rate, experiment with different window size values.

{% highlight python %}python3 Sender4.py <RemoteHost IP> <Port> <Filename> <RetryTimeout>
<WindowSize>{% endhighlight %}

{% highlight python %}python3 Receiver4.py <Port> <Filename> <WindowSize>{% endhighlight %}


