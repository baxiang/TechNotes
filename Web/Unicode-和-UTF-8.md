### ASCII
In the older days of computing, ASCII code was used to represent characters. The English language has only 26 alphabets and a few other special characters and symbols.
在早期的计算机时代中，ASCII码用于表示26个英语字母以及一些特殊的字符和符号。
The table below provides the ASCII characters and their corresponding Decimal and Hex values.
下表展示了ASCII字符对应的十进制值和十六进制值。
![ascii codes](http://upload-images.jianshu.io/upload_images/143845-d3f25e7c69272c8f.gif?imageMogr2/auto-orient/strip)

As you can infer from the above table, the ASCII values can be represented from 0 to 127 in the decimal number system. Lets look at the binary representation of 0 and 127 in 8 bit bytes.
通过上表我们得知，通过0到127可以代表ASCII值任意值，让我们看一下如何通过的8位的二进制来表示0到127。
0 is represented as

![0 in binary](http://upload-images.jianshu.io/upload_images/143845-b8d8074f97fcea18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

127 is represented as

![127 in binary](http://upload-images.jianshu.io/upload_images/143845-5ed6d53f2f6c51b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

It can be inferred from the above binary representation that decimal values 0 to 127 can be represented using 7 bits leaving the 8th bit free.
实际上我们只是用了7个字节就表示除了0到127 第八位是空余的
This is where things started getting messy.

People came up with different ways of using the remaining eight bit which represented decimal values from 128 to 255 and collisions started to happen. For instance the decimal value 182 was used by the Vietnamese to represent the Vietnamese alphabet ờ whereas the same value 182 was used by the Indians to represent the Hindi alphabet *घ*. So if an email written by an Indian contains the alphabet *घ* and if it is read by a person in Vietnam it would appear as *ờ*. Cleary not the intended way to appear.

This is where [Unicode](http://www.unicode.org/) character set came to save the day.
这就是Unicode产生的原因
### Unicode和码点

Unicode character set mapped each character in the world to a unique number. This ensured that there are no collisions between alphabets of different languages. These numbers are platform independent.
Unicode字符集将世界上的每个字符和一个惟一的数字相对应。以此解决不同语言的字母之间的冲突。
**These unique numbers are called as code points in the unicode terminology.**

Lets see how they are referred.

The latin character *ṍ* is referred using the code point

```
U+1E4D  

```

**U+** denotes unicode and **1E4D** is the hexadecimal value assigned to the character ṍ

The English alphabet **A** is represented as **U+0041**

Please visit [http://www.unicode.org/charts/](http://www.unicode.org/charts/) to know the code points for all languages and alphabets of the world
**U +**表示unicode，**1E4D**是分配给字符hex的十六进制值
英文字母**A**表示为**U + 0041**
请访问[http://www.unicode.org/charts/](http://www.unicode.org/charts/)，了解世界上所有语言和字母的代码点

### utf - 8编码

Now that we know what is unicode and how each alphabet in the world is assigned to a unique code point, we need a way to represent these code points in the computer's memory. This is where character encodings come into the picture. One such encoding scheme is UTF-8.
既然我们已知道unicode，以及世界上每个字母拥有一个唯一的的码点的，我们需要一种方法来在计算机内存中的表示这些码点。这就是字符编码的用武之地。其中一种编码方案就是UTF-8。

UTF-8 encoding is a variable sized encoding scheme to represent unicode code points in memory. Variable sized encoding means the code points are represented using 1, 2, 3 or 4 bytes depending on their size
UTF-8编码是一种字节大小可变的编码方案，用于表示内存中的unicode编码点。可变字节长度编码意味着码点根据大小使用1、2、3或4个字节表示。

##### UTF-8 1 byte encoding

A **1 byte encoding** is identified by the presence of 0 in the first bit.

![UTF-8 1 byte encoding](http://upload-images.jianshu.io/upload_images/143845-579d92884dcf8606.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

The English alphabet **A** has unicode code point **U+0041**. It's binary representation is **1000001**.

A is represented in UTF-8 encoding as
**01000001**

**The red 0 bit indicates that 1 byte encoding is used and the remaining bits represent the code point**

##### UTF-8 2 byte encoding

The latin alphabet **ñ** with code point **U+00F1** has binary value **11110001**. This value is larger than the maximum value that can be represented using 1 byte encoding format and hence this alphabet will be represented using UTF-8 2 byte encoding.
拉丁字母ñ与代码点U + 00F1具有二进制值11110001。该值大于可以使用1字节编码格式表示的最大值，因此该字母表将使用UTF-8 2字节编码表示。

2 byte encoding is identified by the presence of the bit sequence **110** in the first bit and **10** in the second bit.
通过第一位中的位序列110和第二位中的10的存在来识别2字节编码。

![image](http://upload-images.jianshu.io/upload_images/143845-a0c3189f9f370c2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

The binary value of the unicode code point **U+00F1** is **1111 0001**. Filling these bits in the 2 byte encoding format, we get the UTF-8 2 byte encoding representation of **ñ** shown below. The filling is done starting with the least significant bit of the code point being mapped to the least significant bit of the second byte.

**11000011 10110001**

The binary digits in blue **11110001** represent the code point U+00F1's binary value and the ones in red are the 2 byte encoding identifiers. The black coloured zeros are used to fill up the empty bits in the byte.

##### UTF-8 3 byte encoding

The latin character **ṍ** with code point **U+1E4D** is be represented using 3 byte encoding as it is larger than the maximum value that can be represented using 2 byte encoding.

A 3 byte encoding is identified by the presence of the bit sequence **1110 in the first byte and 10 in the second and third bytes.**

![image](http://upload-images.jianshu.io/upload_images/143845-8012533ae1b0c6d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

The binary value for the hex code point **0x1E4D** is **1111001001101**. Filling these bits in the above encoding format gives us the UTF-8 3 byte encoding representation of **ṍ** show below. The filling is done starting with the least significant bit of the code point mapped to the least significant of the third byte.
**11100001 10111001 10001101**
The red bits indicate 3 byte encoding, the black ones are filler bits and the blues represent the code point.

#### UTF-8 4 byte encoding

The Emoji 😭 has unicode code point **U+1F62D**. This is bigger than the maximum value that can be represented using 3 byte encoding and hence will be represented using 4 byte encoding.

4 byte encoding is identified by the presence of **11110** in the first byte and **10** in the subsequent second, third and fourth bytes.

![image](http://upload-images.jianshu.io/upload_images/143845-bea45b6a8d5718c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

The binary representation of **U+1F62D** is **11111011000101101**. Filling these bits in the above encoding format gives us the UTF-8 4 byte encoding of 😭. The least significant bit of the code point is mapped to the least significant bit of the fourth byte and so on.

**11110000 10011111 10011000 10101101**

The red bits identify the 4 byte encoding format, the blue ones are the actual code point and the black ones are the filler bits.

### UTF-16 Encoding

UTF-16 encoding is a variable byte encoding scheme which uses either 2 bytes or 4 bytes to represent unicode code points. Most of the characters for all modern languages are represented using 2 bytes.

The latin alphabet **ñ** with code point **U+00F1** and with binary value **11110001** is represented in UTF-16 encoding as
**00000000 11110001**
The above representation is in Big Endian Byte Order mode(Most significant bit first).
UTF-16编码是一种可变字节编码方案，它使用2个字节或4个字节来表示unicode代码点。所有现代语言的大多数字符都使用2个字节表示。

拉丁字母ñ，代码点为U + 00F1，二进制值为11110001，以UTF-16编码表示为
### UTF-32 Encoding

UTF-32 encoding is a fixed byte encoding scheme and it uses 4 bytes to represent all code points.

The English alphabet A has unicode code point U+0041\. It's binary representation is **1000001**.
UTF-32编码是固定字节编码方案，它使用4个字节来表示所有代码点。
英文字母A具有unicode代码点U + 0041。它的二进制表示是1000001。
Its represented in UTF-32 encoding as shown below,

**00000000 00000000 00000000 01000001**

The blue bits are the binary representation of the code point. The above assumes Big Endian Byte order mode.
Thats if for character sets and encoding.
