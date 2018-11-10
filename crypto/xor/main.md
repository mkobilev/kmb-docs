# XOR, XORtool

## XOR
(Логи́ческое сложе́ние, исключа́ющее «ИЛИ», строгая дизъюнкция, XOR, поразрядное дополнение, побитовый комплемент, жегалкинское сложение).

- Логическая операция для булевых манипуляций с битами.

![Рис. 1](http://habrastorage.org/storage2/9af/1de/e09/9af1dee09d4d36ff0b15bdb4aae19e3b.png)

##### Свойста <font face="Arial"> XOR: </font>

> a XOR 0 = a <br>
 a XOR a = 0<br>
a XOR b = b XOR a<br>
 (a XOR b) XOR b = a

Нагладные действия операции<font face="Arial"> XOR: </font>( на языке <font face="Arial">Python</font>):
```python
	x = 5
	y = 7

	x = x^y     # x == 2
	y = x^y     # y == 5
	x = x^y     # x == 7
```
Реверс текстовой строки на языке <font face="Arial"> Java: </font>
```java
    	  public static final String reverseWithXOR(String string) {
	        char[] array = string.toCharArray();
	        int length = array.length;
	        int half = (int) Math.floor(array.length / 2);
	        for (int i = 0; i < half; i++) {
	            array[i] ^= array[length - i - 1];
	            array[length - i - 1] ^= array[i];
	            array[i] ^= array[length - i - 1];
	        }
	        return String.valueOf(array);
	    }		  
```
### Шифрование используя <font face="Arial"> XOR </font>

    (a XOR key) XOR key = a
### <font face="Arial">XOR и CTF</font>
#### <font face="Arial">(Crypto) hack.lu CTF 2011 Simplexor (200)  </font>

> To get a better security we deceided to encrypt our most secret document with the secure xor-algorithm. Unfortunately we lost the key. Now we are sad. Can you help us recovering the key?
>
> Для большей безопасности мы зашифровали наш документ с помощью <font face="Arial"> XOR</font>-алгоритма. Ключ был потерян. Сможете ли вы восстановить ключ?
> (Файл прилагается)

Writeup:
Т.к. файл был зашифрован с помощью XOR алгоритма, нам необходимо использовать <font face="Arial">XORtool или Cryptool.</font>

Во-первых, декодируем <font face="Arial">Base64.</font>

> $ base64 -d simplexor.txt >ciphertext.bin

Осуществляем подбор длины ключа.
Для этого воспользуемся скриптом на <font face="Arial"> Python. </font>

> git clone https://github.com/hellman/xortool.git

Данный скрипт умеет анализировать файл на количество вхождений символов, подбирать ключ, и

сохранять результаты перебора.
```
    $ xortool ciphertext.bin
```
Возможная длина ключа: <center>2:   4.9 %<br>
   4:   7.3 %<br>
   6:   4.8 %<br>
   8:   9.5 %<br>
  10:   4.8 %<br>
  12:   7.1 %<br>
  14:   4.9 %<br>
  16:   14.1 %<br>
  18:   4.8 %<br>
  20:   7.1 %<br>
  22:   4.9 %<br>
  24:   9.2 %<br>
  26:   4.8 %<br>
  28:   7.0 %<br>
  30:   4.8 %<br></center>
16 знаков - наиболее вероятная длина нашего ключа.
Проверяем это, получаем:

> $ xortool ciphertext.bin -c 20
Probable key lengths:
...
1 possible key(s) of length 16:
WklF6e5TEc5XmEG8

----------

> $ xxd xortool_out/0_WklF6e5TEc5XmEG8                       | head
0000000: 2e72 4b08 367f 3e03 1646 4700 6054 7f32     .rK.6.>..FG.`T.2
0000010: 2e38 512f 6320 7435 2020 2020 2020 2020    .8Q/c t5        
0000020: 2d04 2020 200f 5623 0829 2d10 2f65 450d     -.   .V#.)-./eE.

Проблема заключается в том, что наш ключ выглядит длиннее.
По умолчанию скрипт работает со значениями, меньшими 32\. Заставим его работать, скажем, с 257.

    $ xortool ciphertext.bin -m 257 -c 20
	...
	Key-length can be 4*n
	1 possible key(s) of length 64:
	WvhnPry60NRl41weWY7IueaAEc5XmEG8ZOlF6JCWmj8hbvmYkkwFox5Tz1HLvdKl

Как мы видим, мы на правильном пути. Проверим выходные данные.

    $ head xortool_out/0_*
	.oO Phrack 49 Oo.

    Volume Seven, Issue Forty-Nine

    File 14 of 16

    BugTraq, r00t, and Underground.Org
    bring you

    XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
 Отлично! Очевидно, что ключ найден.
 Flag: <font size="2"> liWvhnPry60NRl41weWY7IueaAEc5XmEG8ZOlF6JCWmj8hbvmYkkwFox5Tz1HLvdKl</font>
