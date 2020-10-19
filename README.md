# Contextual Data Transforms

<p align="justify"> 
Modern compression tools and techniques are not based solely on the use of a data compression algorithm, actually the use of this is part of the final stage of an entire compression pipeline, but before reaching the last one, there is a stage called contextual transformations that are responsible for reorganizing the symbols of the dataset so they are more sensitive to statistical compression algorithms such as Huffman, in other words they are artificial generators of redundancy, two of the main algorithms that will be explained in this repository are the <strong>BWT</strong> and <strong>MTF.</strong>
</p> 

<p align="center">
    <img src="https://wittline.github.io/Contextual-Data-Transforms/img/ct.png" />
</p>

<p align="justify"> 
It should be noted that the Huffman algorithm is widely used in many known compression tools or codecs, the image below shows the compression pipelines of some of these tools and we can see that the huffman coding is very common in much of them
</p>

<p align="center"> 
    <img src="https://wittline.github.io/Contextual-Data-Transforms/img/codecs_.png" />
</p>

<p align="justify">
On the other hand, it is relevant to mention that Hadoop is one of the most famous tools to control and manage large amounts of data and is composed of the most robust codecs to compress its formats, most of these codecs are the tools mentioned below, for example in bzip2 the blocks can be decompressed independently, or they can be decompressed in parallel, making it a good tool to use in data-intensive solutions with distributed computing frameworks such as Hadoop, Apache Spark, and Hive. There is a considerable amount of studies on this, where the studies are based on improving the communication of these codecs with the main components of Hadoop. Internally these codecs use the contextual transformations mentioned in this document and others that were not mentioned, shows the codecs commonly used by Hadoop components, as mentioned before two of them use Huffman in their compression pipeline.
</p>

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/codecs.png" />
</p>


## Burrows Wheeler transform (BWT)

<details closed>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/bwt.html"> <strong>Check this code:</strong> Burrows Wheeler transform (BWT) </a>
  </summary>
</details>

<p align="justify">
Before processing with the MTF algorithm, the sequence of symbols of the original dataset must go through the BWT algorithm first, this in order to improve the rate compression using the statistical encoding algorithm, below, an example of how its works using the following dataset: ABADBEAB
</p>

- The first input sequence is placed inside the first row of a matrix of size N * N, where N is the size of the sequence, the other rows have the same sequence, but each one cyclically rotated once to the left.

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/bwt1.png" />
</p>

- The rows must be ordered lexicographically, the last highlighted column represents the BWT transformed string, in which repetitions of nearby symbols or clusters of symbols can be observed, the highlighted row represents the sequence of symbols from the original dataset.

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/bwt2.png" />
</p>

<p align="justify">
The output of the BWT transformation algorithm for this example is expressed as follows: BWT = [I, S] where S represents the last column EBBAADAB and the value I = 1 represents the index of the row where the original sequence appears, both variables are needed to decode a sequence of symbols that was encoded with the BWT algorithm.
</p>

<p align="justify">
  The reverse process can retrieve the original sequence of the dataset using the output variables of the previous process, I and S, the BWT decoding works as follows, it is iterated N times, where N is equal to the size of the sequence, in each iteration three actions occur that involve the following series of steps:
</p>

- The sequence S is placed in a first column and the sequence S ordered lexicographically in a second column.

- In the next iteration the first column is the concatenation of the first and second column of the previous step and the second column is the lexicographically ordered concatenation of the previous step and so on until iteration N.

- The index I will tell us which is the correct sequence that represents the original dataset in iteration N.


<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/bwt3.png" />
</p>


## Move to Front (MTF)

<details closed>    
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/mtf.html"> <strong>Check this code:</strong> Move to Front (MTF) </a> </summary>    
</details>

<p align="justify">
The goal of this transformation technique is to improve the compression rate for a statistical compression algorithm, this method achieves better results when there are clusters of repeated symbols in the sequence being analyzed and normally this last feature is offered by the BWT, in chunks of the sequence of symbols of the original dataset there are always biases in the probabilities of the symbols, this means that locally there are repetitions of symbols and this is used by the MTF, its operation is described below using the output dataset of the algorithm BWT (EBBAADAB)
</p>

- We start with an ordered alphabet list, in this case the example is text-based, if we had images or other data where there is a larger alphabet it would not be a bad idea to use a byte-ordered list of all ASCII symbols.

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/mtf1.png" />
</p>

- Each element in the ordered list must be easily identified by its index, this means that the first element in the list can be identified by index 0 and so on.

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/mtf2.png" />
</p>

- At the moment of coding, if a symbol occurs the first time we must send the index corresponding to that symbol in the list, and the symbol is moved to the front of the list, the movements of the found index are stored for the output.

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/mtf3.png" />
</p>

<p align="justify">
In this case, the output of the algorithm returns 4202412, a list of integers in small ranges, there is a probability that the union of the MTF-encoded chunks will have more repetitions and therefore less entropy, the compression with huffman benefits from this process, in In this example the entropy of the final sequence changed very little but above the BWT sequence, The decoder can recover the original sequence by reversing the process, given 4202412, it starts with an ordered list from A to Z and for each symbol that the decoder read, output the value at that index in the ordered list, then move that symbol to the front of the ordered list.
</p>

<details closed>    
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/mtf.html"> <strong>Check this code:</strong> BWT + MTF </a> </summary>    
</details>

## Run Length Encoding (RLE)
  
<details closed>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/rle.html"> <strong>Check this code:</strong> Run Length Encoding (RLE) </a> </summary>
</details>

<p align="justify">
RLE takes advantage of the succession of repeated symbols, also called clustered of symbols, a sequence of the same symbol is replaced by its number of repetitions and the symbol that is repeated, we call this type of replacement "RUN", see the following example:
</p>

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/rle1.png" />
</p>

<p align="justify">In the example above we can clearly see that the algorithm in addition to offering a transformation offers compression, this algorithm also has its disadvantages when the repetitions of symbols are very short, it is advisable to read about the different implementations associated with this kind of contextual transformation. check the below repository too:
</p>

[@powturbo/Turbo-Run-Length-Encoding](https://github.com/powturbo/Turbo-Run-Length-Encoding )


## Delta encoding

<details closed>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/de.html"> <strong>Check this code:</strong> Delta encoding  </a> </summary>
</details>  

<p align="justify">
Sometimes there are local correlations between the data transmitted, for example in time-series, images or audio. It is absolutely necessary take advantage of the adjacency of each symbol, and sometimes could be very suitable, two adjacent values could describe one of them as the difference of the other value, subtract the current value from the previous one and store the difference in the same stream that is transmitted. This technique is called Delta Encoding and it is one of the most important algorithms regarding the compression and transmission of data means.
</p>

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/de1.png" />
</p>

## XOR Delta encoding

<details closed>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/xor.html"> <strong>Check this code:</strong> XOR encoding  </a> </summary>
</details>

<p align="justify">
XOR Delta encoding can deal with problems related to negative values, in addition to having the property of lowering the range of values, which in terms of entropy suits us.
</p>

<p align="center">
  <img src="https://wittline.github.io/Contextual-Data-Transforms/img/xor1.png" />
</p>


## Conclusions and recommendations
<p align="justify">  
    
- Please <strong>do not use an Cellular automaton as a transformation technique</strong>, it is expensive, slow and does not offer good results, besides It is a technique that goes against the above explained.

</p>
<p align="justify">
    
- When the differences between subsequent values is small Delta encoding could be suitable, in other cases negative values could be generated and this is bad.
    
</p>

<p align="justify">
    
- Xor delta encoding will not always lower the entropy of the symbol sequence, but it can help you to keep positive values in case the Delta encoding technique has generated negative values.
    
</p>

<p align="justify">
    
- If your data set does not has large repetitions of symbols, RLE won’t be suitable MTF could be a good option.

</p>

<p align="justify">
    
- MTF lowers the range of the values, thus offering a chain of symbols with less entropy, but the performance of this algorithm depends on a previous transformation that can create clusters of repeated symbols, the BWT can be a good option to generate these clusters.

</p> 

<p align="justify">
    
- In cases where the nature of the dataset contains symbols with uniform distributions, for example images, the use of the BWT and MTF in the compression pipeline could help to achieve acceptable compression rates.
    
</p>  

<p align="justify">
    
- BWT does not compress the data, you only will have a different permutation of the symbols that has consecutive repetitions and therefore a lower entropy.

</p> 



# Contributing and Feedback
Help me to improve this repository or just copy and paste my work

# Authors
- Created by <a href="https://www.linkedin.com/in/ramsescoraspe"><strong>Ramses Alexander Coraspe Valdez</strong></a>
- Created Oct, 2020

# License
This project is licensed under the terms of the MIT license.
