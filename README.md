# Contextual Data Transforms

<p align="justify"> 
Modern compression tools and techniques are not based solely on the use of a data compression algorithm, actually the use of this is part of the final stage of an entire compression pipeline, but before reaching the last one, there is a stage called contextual transformations that are responsible for reorganizing the symbols of the dataset so they are more sensitive to statistical compression algorithms such as Huffman, in other words they are artificial generators of redundancy, two of the main algorithms that will be explained in this repository are the <strong>BWT</strong> and the <strong>MTF.</strong>
</p> 

<p align="center">
  <img src="https://github.com/Wittline/Huffman-decoding/blob/master/docs/images/ct.png" />
</p>

<p align="justify"> 
It should be noted that the Huffman algorithm is widely used in many known compression tools or codecs, the image below shows the compression pipelines of some of these tools and we can see that the huffman coding is very common in much of them
</p>

<p align="center">
  <img src="https://github.com/Wittline/Huffman-decoding/blob/master/docs/images/codecs.png" />
</p>

<p align="justify">
On the other hand, it is relevant to mention that Hadoop is one of the most famous tools to control and manage large amounts of data and is composed of the most robust codecs for compressing its formats, The blocks in bzip2 can be independently decompressed, bzip2 files can be decompressed in parallel, making it a good format for use in big data solutions with distribuited computing frameworks like Hadoop, Apache Spark, and Hive
</p>

<p align="center">
  <img width="90%" src="https://github.com/Wittline/Huffman-decoding/blob/master/docs/images/hadoop_codecs.png" />
</p>


## Burrows Wheeler transform (BWT)

<details closed>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/bwt.html"> Burrows Wheeler transform (BWT) </a>
  </summary>
</details>

<p align="justify">
Before processing with the MTF algorithm, the sequence of symbols of the original data set must go through the BWT algorithm first, this in order to improve the rate compression using the statistical encoding algorithm, below, an example of how its works using the following dataset: ABADBEAB
</p>

- The first input sequence is placed inside the first row of a matrix of size N * N, where N is the size of the sequence, the other rows have the same sequence, but each one cyclically rotated once to the left.

<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/bwt1.png" />
</p>

- The rows must be ordered lexicographically, the last highlighted column represents the BWT transformed string, in which repetitions of nearby symbols or clusters of symbols can be observed, the highlighted row represents the sequence of symbols from the original dataset.

<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/bwt2.png" />
</p>

<p align="justify">
The output of the BWT transformation algorithm for this example is expressed as follows: BWT = [I, S] where S represents the last column EBBAADAB and the value I = 1 represents the index of the row where the original sequence appears, both variables are needed to decode a sequence of symbols that was encoded with the BWT algorithm.
</p>

<p align="justify">
  The reverse process can retrieve the original sequence of the data set using the output variables of the previous process, I and S, the BWT decoding works as follows, it is iterated N times, where N is equal to the size of the sequence, in each iteration three actions occur that involve the following series of steps:
</p>

- The sequence S is placed in a first column and the sequence S ordered lexicographically in a second column.

- In the next iteration the first column is the concatenation of the first and second column of the previous step and the second column is the lexicographically ordered concatenation of the previous step and so on until iteration N.

- The index I will tell us which is the correct sequence that represents the original data set in iteration N.


<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/bwt3.png" />
</p>


## Move to Front (MTF)

<details closed>    
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/mtf.html"> Move to Front (MTF) </a> </summary>    
</details>

<p align="justify">
The goal of this transformation technique is to improve the compression rate for a statistical compression algorithm, this method achieves better results when there are clusters of repeated symbols in the sequence being analyzed and normally this last feature is offered by the BWT, in chunks of the sequence of symbols of the original data set there are always biases in the probabilities of the symbols, this means that locally there are repetitions of symbols and this is used by the MTF, its operation is described below using the output dataset of the algorithm BWT (EBBAADAB)
</p>

- We start with an ordered alphabet list, in this case the example is text-based, if we had images or other data where there is a larger alphabet it would not be a bad idea to use a byte-ordered list of all ASCII symbols.

<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/mtf1.png" />
</p>

- Each element in the ordered list must be easily identified by its index, this means that the first element in the list can be identified by index 0 and so on.

<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/mtf2.png" />
</p>

- At the moment of coding, if a symbol occurs the first time we must send the index corresponding to that symbol in the list, and the symbol is moved to the front of the list, the movements of the found index are stored for the output.

<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/mtf3.png" />
</p>

<p align="justify">
In this case, the output of the algorithm returns 4202412, a list of integers in small ranges, there is a probability that the union of the MTF-encoded chunks will have more repetitions and therefore less entropy, the compression with huffman benefits from this process, in In this example the entropy of the final sequence changed very little but above the BWT sequence, The decoder can recover the original sequence by reversing the process, given 4202412, it starts with an ordered list from A to Z and for each symbol that the decoder read, output the value at that index in the ordered list, then move that symbol to the front of the ordered list.
</p>

<details closed>    
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/mtf.html"> Check this code: BWT + MTF </a> </summary>    
</details>

## Run Length Encoding (RLE)
  
<details closed>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/rle.html"> Run Length Encoding (RLE) </a> </summary>
</details>

<p align="justify">
RLE takes advantage of the succession of repeated symbols, also called clustered of symbols, a sequence of the same symbol is replaced by its number of repetitions and the symbol that is repeated, we call this type of replacement "RUN", see the following example:
</p>

<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/rle1.png" />
</p>

## Delta encoding

<details closed>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/de.html"> Delta encoding  </a> </summary>
</details>  


## XOR encoding

<details closed>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/xor.html"> XOR encoding  </a> </summary>
</details>

## Recommendations

Please <strong>do not use an Cellular automaton as a transformation technique</strong>, it is expensive, slow and does not offer good results, besides It is a technique that goes against the above explained.

# Contributing and Feedback
Help me to improve, you can insult me, criticize me, eulogy me or just copy and paste my homework

# Authors
- Created by <a href="https://www.linkedin.com/in/ramsescoraspe"><strong>Ramses Alexander Coraspe Valdez</strong></a>
- Created Oct, 2020

# License
This project is licensed under the terms of the MIT license.
