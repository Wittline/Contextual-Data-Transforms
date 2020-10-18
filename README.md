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

Before processing with the MTF algorithm, the sequence of symbols of the original data set must go through the BWT algorithm first, this in order to improve the rate compression using the statistical encoding algorithm, below, an example of how its works using the following dataset: ABADBEAB


- The first input sequence is placed inside the first row of a matrix of size N * N, where N is the size of the sequence, the other rows have the same sequence, but each one cyclically rotated once to the left.

<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/bwt1.png" />
</p>

- The rows must be ordered lexicographically, the last highlighted column represents the BWT transformed string, in which repetitions of nearby symbols or clusters of symbols can be observed, the highlighted row represents the sequence of symbols from the original dataset.

<p align="center">
  <img width="90%" src="https://wittline.github.io/Contextual-Data-Transforms/img/bwt2.png" />
</p>


La salida del algoritmo de transformación BWT para este ejemplo se expresa de la manera siguiente: \mathbit{BWT}\ =\ \left[\mathbit{I},\ \mathbit{S}\right]\ donde S representa la última columna EBBAADAB y el valor I = 1 representa el índice de la fila donde aparece la secuencia original, ambas variables son necesarias para decodificar una secuencia de símbolos que fue codificada con el algoritmo BWT.


<details open>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/bwt.html"> Burrows Wheeler transform (BWT) </a>
  </summary>
</details>



<details open>    
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/mtf.html"> Move to Front (MTF) </a> </summary>    
</details>

<details open>    
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/mtf.html"> BWT + MTF </a> </summary>    
</details>
  
<details open>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/rle.html"> Run-Length Encoding (RLE) </a> </summary>
</details>

<details open>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/de.html"> Delta encoding  </a> </summary>
</details>  

<details open>
<summary> <a href="https://wittline.github.io/Contextual-Data-Transforms/code/xor.html"> XOR encoding  </a> </summary>
</details> 



# Contributing and Feedback
Help me to improve, you can insult me, criticize me, eulogy me or just copy and paste my homework

# Authors
- Created by <a href="https://www.linkedin.com/in/ramsescoraspe"><strong>Ramses Alexander Coraspe Valdez</strong></a>
- Created Oct, 2020

# License
This project is licensed under the terms of the MIT license.
