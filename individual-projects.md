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

## L4 firewall in OpenFlow and Port Mirroring in OpenFlow

  > Python, OpenFlow, Minimet, Ryu

The purpose of this project was to create an L4 firewall to prevent TCP connections from being initiated by external hosts, while allowing for those initiated by the internal hosts, and in another network to allow all the packets to go through the switch, but for every TCP connection initiated by an external host, you want to collect first ten packets coming from the external network.
  
### L4 Firewall
  
An OpenFlow controller was implemented on an OpenFlow switch with Ryu, using Python and Mininet to emulate the network.
  
The emulated network consists of one internal host, one external host and one switch. The switch has two ports; port 1 and port 2 which connect to the internal and external host, respectively. 
  
If an IPv4 TCP packet arrives, insert flow into the switch. If port 2 ovserves a returning packet for this connection arriving, it must forward it to port 1 to insert the corresponding flow. These flows must match against input switch port, network layer protocol, source IP address, destination IP address, transport layer protocol, source port and destination port.
  
If a packet comes from port 2 and the corresponding flow entry exists in the maintained hash table, which is the four-tuple of the packet with source and destination addresses and ports swapped, does not exist, it should be dropped, otherwise it should be forwarded to switch port 1 and the flow entry is inserted in the switch, again so that the controller does not have to process packets in the same flow (port 2 to 1). 
  
  
### Port Mirroring in OpenFlow
  
The purpose of this task, in a separate network, is to allow all packets to go through. For every TCP connecton initiated by an external host, the first ten incoming packets are collected.
  
There is a slightly different network topology where the switch has one more port (port 3) connected to another host, h3, that collects the first ten packets in every TCP connection initiated by an external host.

The controller works as follows: TCP IPv4 packets coming from port 1 (internal packets) have their flow inserted unconditionally.
  
  
  
  
Further, it must insert the flow when it sees 10th packet coming from port 2 in the connection, counting from the TCP packet with the SYN flag set and the ACK flag unset; the controller must process the first 10 packets while duplicating and forwarding (i.e., mirroring) these packets to switch port 3 (in addition to forwarding them to port 1).
  
  
  
  
If the packet comes from port 2 and contains the TCP SYN flag and does not contain the ACK flag (i.e., active TCP connection from the external network), you would add the key, which consists of a tuple of (<src_ip><dst_ip><src_port><dst_port>), to the dictionary (hash table) self.ht with the value 1 (indicate packet count). Then also add a new action, which sends the packet to port 3 (in addition to the action that sends to port 1), to the actions list (acts). However, since the controller wants to keep seeing the packets that belong to this TCP connection and arrive at switch port 2, it does not add the flow to the switch yet; every such a packet will increment the packet counter in the dictionary, and when seeing the 10th packet, the controller will remove the flow key in the dictionary and adds the flow entry to the switch (see l2learn.py for syntax, but you also have to specify eth_type, ipv4_src, ipv4_dst, ip_proto, tcp_src and tcp_dst optional arguments for this task), as it does not need to mirror the packets to switch port 3 anymore. Flows for TCP packets arriving at port 1 must be added to the switch unconditionally.
  
  
  
  
  
  
## Information Retrieval System Evaluation and Text Classification Using Text Technologies
  
  build a module to evaluate IR systems using different retrieval scores. The inputs to your module are:

- system_results.csv file, containing the retrieved set of documents for 10 queries numbered from 1 to 10 for each of 6 different IR systems. The format of the files is as follow:

- qrels.csv file, which contains the list of relevant documents for each of the 10 queries. The format of the file is as follows:




Develop a module EVAL that calculates the following measures:
- P@10: precision at cutoff 10 (only top 10 retrieved documents in the list are considered for each query).
- R@50: recall at cutoff 50.
- r-precision
- AP: average precision
hint: for all previous scores, the value of relevance should be considered as 1. Being 1, 2, or 3 should not make a difference on the score.
- nDCG@10: normalized discount cumulative gain at cutoff 10.
- nDCG@20: normalized discount cumulative gain at cutoff 20.


TEXT ANALYSIS



Begin by downloading the training corpora, which contain verses from the Quran and the Bible (split into Old and New Testaments), here. For this coursework, we will consider the Quran, New Testament, and Old Testament each to be a separate corpus, and their verses as individual documents.


Preprocess the data as usual, including tokenization, stopword removal, etc. Note: you may reuse code from previous labs and courseworks to achieve this.
Compute the Mutual Information and Χ2 scores for all tokens (after preprocessing) for each of the three corpora. Generate a ranked list of the results, in the format token,score.

- What differences do you observe between the rankings produced by the two methods (MI and Χ2)?

- Include a table in your report showing the top 10 highest scoring words for each method for each corpus.
  
Run LDA on the entire set of verses from ALL corpora together. Set k=20 topics and inspect the results.
- For each corpus, compute the average score for each topic by summing the document-topic probability for each document in that corpus and dividing by the total number of documents in the corpus.
- Then, for each corpus, you should identify the topic that has the highest average score (3 topics in total). For each of those three topics, find the top 10 tokens with highest probability of belonging to that topic.
Add another section "Topic Analysis" to your report which includes:
- A table including the top 10 tokens and their probability scores for each of the 3 topics that you identified as being most associated with each corpus.


## Micro-Haskell Compiler
  
The objective of this practical is to illustrate the language processing pipeline in the
case of a small but interesting fragment of the Haskell programming language, which we
shall call Micro-Haskell (or MH). The practical is self-contained, and prior knowledge of
Haskell itself is not assumed; however, those with no previous experience of Haskell may
need to invest a little more time to understand what is required. The brief introduction
to Micro-Haskell to be given in Lecture 14 may also be helpful.
The practical illustrates all stages of the language processing pipeline for programming languages, taking us from a source file, written in MH, to execution of the program.
In our case, the pipeline has four stages: lexing, parsing, typechecking and evaluation.
Your task is to provide language-specific material to assist with the first three stages:
lexing, covered in Part A of the practical; parsing, covered in Part B; and typechecking,
covered in Part C. A simple evaluator is provided for you.
The code implementing all four stages will itself be written in Java. (So you are
warned at the outset that you’ll need to think in two languages at once!) Several
general-purpose components are provided; your task is to supply the parts specific to
MH, by applying your understanding of the course material. Once you have finished,
you’ll be able to hook up your implementations to the given evaluator to obtain a
complete working implementation of MH.  
  
  
  
  
  
## Natural Language Query System
  
  
In this assignment, you will use Python(Python2) and NLTK to construct a system
that reads simple facts and then answers questions about them
  
Example output
  
{% highlight python %}
$$ John is a duck.
OK
$$ Mary is a duck.
OK
$$ John is purple.
OK
$$ Mary flies.
OK
$$ John likes Mary.
OK
$$ Who is a duck?
John Mary
$$ Who likes a duck who flies?
John
$$ Which purple ducks fly?
None
{% endhighlight %}
  
  
  
In Part A, you will develop the machinery for processing statements. This will include
a simple data structure for storing the words encountered (a lexicon), and another for
storing the content of the statements (a fact base). You will also write some code to
2
extract a verb stem from its 3rd person singular form (e.g. flies → fly).
Parts B to D develop the machinery for questions. Part B is concerned with part-ofspeech tagging of questions, allowing for ambiguity and also taking account of singular
and plural forms for nouns and verbs. In Part C you are given a context free grammar
for the question language, along with a parser, courtesy of NLTK. Your task is to write
some Python code that does agreement checking on the resulting parse trees, in order
to recognize that e.g. Which ducks flies? is ungrammatical. Agreement checking is
used in the system to eliminate certain impossible parse trees. In Part D, you will
give a semantics for questions, in the form of a Python function that translates them
into lambda expressions. These lambda expressions are then processed by NLTK to
transform them into logical formulae; the answer to the question is then computed by a
back-end model checker which is provided for you.
  
  
  
  
## Blockchain Smart Contract Programming
  
> Solidity
  
Matching Pennies - Game Theory
Two players 
One even
One odd. 

Each player has a penny 
must secretly turn the penny to heads or tails

The players then reveal their choices simultaneously

If the pennies match (both heads or both tails)
Even keeps both pennies
wins one from Odd (+1 for Even, −1 for Odd). 
If the pennies do not match (one heads and one tails) 
Odd keeps both pennies
wins one from Even (−1 for Even, +1 for Odd).

Matching Numbers - Assignment Specific
Two players 
A
B

Each player has a choice 
Pick a number in {0,1}

The players then reveal their choices simultaneously

If the choices match (both 0 or both 1)
A wins 1 ETH
If the choices do not match (one 0 and one 1)
B wins 1 ETH.

Once the game is over, two new players should be able to use the contract to start a new game










Details
Implement smart contract
Deploy it on testnet
Contract, as far as possible, should be 
Secure
Gas Efficient
Fair
After deploying contract
Play other student’ contract
Evaluate their code and analyse its features in terms of security and fairness (lecture 04)
  
  
  
  
## Operating Systems Programming
  
> C++, Qemu, vncviewer
  
Using [InfOS](https://github.com/tspink/infos) created by Tom Spink, a small, readable swap out operating system written in C++. 
  
Implemented a priority task scheduler and the _tree_ command to the system.
  
  The task scheduler of an operating system is the component that decides which tasks get to run on the processor. When there is only a single physical processor in a system, that processor must be shared amongst all runnable tasks.
  
  
The multiple queue priority algorithm allows for greater flexibility during scheduling, but it is not perfect. Describe a common problem with the algorithm, and suggest a potential solution, making sure to explain how your suggestion addresses the problem. Your solution should not be any of the algorithms already discussed in lectures, but instead, the result of your own independent research. If your new solution has any drawbacks, be sure to acknowledge those. You should write your answer in a text file called cw1.txt, making sure you cite all sources appropriately.
  
  
  
  
The tree command has similar functionality, but operates recursively, using indentation to display the hierarchical structure of the directory contents. Upon terminating, tree then prints the total number of files and directories listed. An example can be seen below.
  
<img src="tree-example.png">
  
  
## Information Retreival Tool
  
  Implement a simple IR tool, that does the following:
Pre-processes text
- Tokenisation
- Stopword removal using this list of words. You MUST use this list.
- Porter stemming. You can use packages for this part, such as Snowball or NLTK.
Creates a positional inverted index
Your index can have whatever structure you like, and can be stored in any format you like, but you will need to output it to a text file using the format specfied below.
Uses your positional inverted index to perform:
- Boolean search
- Phrase search
- Proximity search
- Ranked IR based on TFIDF
  
  
  IR rankings then output into text files.
  
  
  
## This website!
  
This we bsite was created using GitHub pages and Jekyll themes, and put together using a combination of HTML and markdown.
  
  
  
  
  
  
