Download Link: https://assignmentchef.com/product/solved-cs201-assignment-4-david-huffman-compression
<br>
You’ve probably heard about David Huffman and his popular compression algorithm

The idea behind Huffman coding is based upon the frequency of a symbol in a sequence. The symbol that is the most frequent in that sequence gets a new code that is very small, the least frequent symbol will get a code that is very long, so that when we’ll translate the input we want to encode the most frequent symbols will take less space than they used to and the least frequent symbols will take more space but because they’re less frequent it won’t matter that much.

For this algorithm you need to have a basic understanding of <strong>binary tree</strong> data structure and the <strong>ordered queue</strong> (<strong>sorted on the bases of frequency</strong>) data structure. In the source code we’ll use the ordered queue.

Let’s say we have the string “beep boop beer!” which in his actual form, occupies 7 bits (suppose, ascii 0 to 127 ). That means that in total, it occupies 15*7 = 105 bits of memory. Through encoding, the string will occupy 40 bits approximately.

To better understand this example, we’ll going to apply it on an example. The string “beep boop beer!” is a very good example to illustrate this. In order to obtain the code for each element depending on its frequency we’ll need to build a binary tree such that each leaf of the tree will contain a symbol (a character from the string). The tree will be built from the leafs to the root, meaning that the elements of least frequency will be farther from the root than the elements that are more frequent.

To build the tree this way we’ll use a ordered queue, that the element with the least frequency is the most important. Meaning that the elements that are the least frequent will be the first ones we get from the queue. We need to do this so we can build the tree from the leaves to the root.

<strong>Example:</strong>

If we want to compress the string “beep boop beer!” Firstly we calculate the frequency of each character :

<table width="309">

 <tbody>

  <tr>

   <td width="150">Character</td>

   <td width="160">Frequency</td>

  </tr>

  <tr>

   <td width="150">‘b’</td>

   <td width="160">3</td>

  </tr>

  <tr>

   <td width="150">‘e’</td>

   <td width="160">4</td>

  </tr>

  <tr>

   <td width="150">‘p’</td>

   <td width="160">2</td>

  </tr>

  <tr>

   <td width="150">‘ ‘</td>

   <td width="160">2</td>

  </tr>

  <tr>

   <td width="150">‘o’</td>

   <td width="160">2</td>

  </tr>

  <tr>

   <td width="150">‘r’</td>

   <td width="160">1</td>

  </tr>

  <tr>

   <td width="150">‘!’</td>

   <td width="160">1</td>

  </tr>

 </tbody>

</table>

After calculating the frequencies, we’ll create binary tree nodes for each character, and we’ll introduce them in the ordered queue with the frequency:




<strong>Frequencies                 </strong>1                1                2                2                2                3                4

We now get the first two elements from the queue and create a link between them by creating a new binary tree node to have them both as successors (initialize root node with ‘<strong>*</strong>’ (character) ), so that the characters are siblings and we add their frequencies. After that we add the new node we created with the sum of the frequencies of its successors as it’s frequency in the queue.













We repeat the same steps and we get the following:

























Now after we link the last two elements, we’ll get the final tree:




Now, to obtain the code for each symbol we just need to traverse the trees until we get to that leaf node (symbol (character)). Now we associate 0 (zero) to left edges and 1 (one) to right edges, after each step we take to the left we concatenate a 0 to the code or 1 if we go right.







To decode a string of bits we just need to traverse the tree for each bit, if the bit is 0 we take a left step and if the bit is 1 we take a right step until we hit a leaf (which is the symbol we are looking for). For example, if we have the string “101 11 101 11″ . Using above given tree, decoding it we’ll get the string “pepe”.

A practical aspect of implementing this algorithm is considering building a Huffman table as soon as we have the tree. The table is basically a structure that contains each symbol with its code because it will make encoding somewhat more efficient. It’s hard to look for a symbol by traversing a tree and at the same time calculating its code because we don’t know where exactly in the tree that symbol is located. <strong>As a principle, we use a Huffman table for encoding and a Huffman tree for decoding.</strong>

<table width="309">

 <tbody>

  <tr>

   <td width="93">Character</td>

   <td width="217">Code</td>

  </tr>

  <tr>

   <td width="93">‘b’</td>

   <td width="217">00</td>

  </tr>

  <tr>

   <td width="93">‘o’</td>

   <td width="217">010</td>

  </tr>

  <tr>

   <td width="93">‘ ’</td>

   <td width="217">011</td>

  </tr>

  <tr>

   <td width="93">‘r‘</td>

   <td width="217">1000</td>

  </tr>

  <tr>

   <td width="93">‘!’</td>

   <td width="217">1001</td>

  </tr>

  <tr>

   <td width="93">‘p’</td>

   <td width="217">101</td>

  </tr>

  <tr>

   <td width="93">‘e’</td>

   <td width="217">11</td>

  </tr>

 </tbody>

</table>










Original String: beep boop beer!

105 bits: <u>110 0010</u> (b) <u>110 0101</u> (e) <u>110 0101</u> (e) <u>111 0000</u> (p) <u>010 0000</u> (space) <u>110 0010</u> (b) <u>110 1111</u>

(o) <u>110 1111</u> (o) <u>111 0000</u> (p) <u>010 0000</u> (space) <u>110 0010</u> (b) <u>110 0101</u> (e) <u>110 0101</u> (e) <u>111 0010</u> (r) <u>010</u>

<u>0001 (!)</u>

Encoded 40 bits: <u>00</u> (b) <u>11</u> (e) <u>11</u> (e) <u>101</u> (p) <u>011</u> (space) <u>00</u> (b) <u>010</u> (o) <u>010</u> (o) <u>101</u> (p) <u>011</u> (space) <u>00</u> (b) <u>11</u> (e) <u>11</u> (e) <u>1000</u> (r) <u>1001</u> (!)

<strong>Build Compressed file: </strong>

Use Original String &amp; Huffman Table to construct <strong>encoded.txt   </strong>

Create 7 bits character using Huffman Table. When 7 bits are completed, and code is incomplete, remaining bits will move to next character.

Since the character encodings have different lengths, often the length of a Huffman-encoded file does not come out to an exact multiple of 7 bits. Files are stored as sequences of character (7 bits), so in cases like this the remaining digits of the last bit are filled with 0s.




<strong>2 extra bits to complete char (7 bits) </strong>




<table width="325">

 <tbody>

  <tr>

   <td width="85">character Number</td>

   <td width="108">Binary</td>

   <td width="132">ASCII value / char</td>

  </tr>

  <tr>

   <td width="85">1st</td>

   <td width="108"><strong>00</strong> <strong>11</strong> <strong>11</strong> <strong>1</strong></td>

   <td width="132">62 / ‘ &gt; ’</td>

  </tr>

  <tr>

   <td width="85">2<sup>nd</sup></td>

   <td width="108"><strong>01 011 00</strong></td>

   <td width="132">44 / ‘ ’ ’</td>

  </tr>

  <tr>

   <td width="85">3rd</td>

   <td width="108"><strong>010 010 1 </strong></td>

   <td width="132">37 / ‘ % ’</td>

  </tr>

  <tr>

   <td width="85">4<sup>th</sup></td>

   <td width="108"><strong>01 011 00 </strong></td>

   <td width="132">44 / ‘ ’ ’</td>

  </tr>

  <tr>

   <td width="85">5th</td>

   <td width="108"><strong>11 11  100 </strong></td>

   <td width="132">124 / ‘ | ’</td>

  </tr>

  <tr>

   <td width="85">6<sup>th</sup></td>

   <td width="108"><strong>0 1001 00 </strong></td>

   <td width="132">36 / ‘ $ ’</td>

  </tr>

 </tbody>

</table>

Write these characters (&gt;’%’|$) to encoded file.

<strong>Uncompressing the file: </strong>

Read the <strong>character</strong> from encoded.txt and convert in 7 bits binary and search original character from

<strong>Huffman tree</strong>, write that character on to reconstructed.txt

<strong>What to do in the Assignment? </strong>

Input: You will be provided a text file namely original.txt.

Tasks:

<ul>

 <li>Read the data from “original.txt” file</li>

 <li>Calculate the original size of the file in bits.</li>

 <li>Find out how many unique characters are there in the file.</li>

 <li>Find the frequency of each character. (print characters and their frequencies)</li>

 <li>Build Huffman tree using Ordered Queues (Note: tree should be link list Structure &amp; Queue should be Array based Structure). Print the tree inorderly (including ‘*’).</li>

 <li>Construct a Huffman Table which will contain a character and its corresponding encoding. Print this table.</li>

 <li>Store encoding of each character in a separate file called <strong>txt </strong>and print the size of encoded.txt file in bits.</li>

 <li>Next using the “encoded.txt” reconstructs the original file and call it “<strong>txt</strong>”.</li>

 <li>Check (using programming) whether “<strong>txt</strong>” and “<strong>reconstructed.txt</strong>” are exactly same or not. Print complete report which includes original.txt size in bits, encoded.txt size in bits, reconstructed.txt size in bits, and print those characters whose are different including line &amp; column numbers. (which will be added only in assignments)</li>

</ul>