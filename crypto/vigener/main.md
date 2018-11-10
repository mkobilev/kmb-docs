# Шифр Виженера

 ![](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1a/Vigenere.jpg/220px-Vigenere.jpg)

**Шифр Виженера** — метод полиалфавитного шифрования буквенного текста с использованием ключевого слова.

# Описание алгоритма #

----------


В шифре Цезаря каждая буква алфавита сдвигается на несколько позиций; например в шифре Цезаря при сдвиге +3, A стало бы D, B стало бы E и так далее. Шифр Виженера состоит из последовательности нескольких шифров Цезаря с различными значениями сдвига. Для зашифровывания может использоваться таблица алфавитов, называемая tabula recta или квадрат (таблица) Виженера. Применительно к латинскому алфавиту таблица Виженера составляется из строк по 26 символов, причём каждая следующая строка сдвигается на несколько позиций. Таким образом, в таблице получается 26 различных шифров Цезаря. На каждом этапе шифрования используются различные алфавиты, выбираемые в зависимости от символа ключевого слова.

 ![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/25/Vigenère_square.svg/300px-Vigenère_square.svg.png)

**Квадрат Виженера**

 Например, предположим, что исходный текст имеет вид:

    ATTACKATDAWN

Человек, посылающий сообщение, записывает ключевое слово («LEMON») циклически до тех пор, пока его длина не будет соответствовать длине исходного текста:

    LEMONLEMONLE

Первый символ исходного текста A зашифрован последовательностью L, которая является первым символом ключа. Первый символ L шифрованного текста находится на пересечении строки L и столбца A в таблице Виженера. Точно так же для второго символа исходного текста используется второй символ ключа; то есть второй символ шифрованного текста X получается на пересечении строки E и столбца T. Остальная часть исходного текста шифруется подобным способом.

    Исходный текст:   ATTACKATDAWN

    Ключ: LEMONLEMONLE

    Зашифрованный текст: LXFOPVEFRNHR

# Расшифрование #

----------


 Криптоанализ шифра может быть построен в два этапа:

 1.	Поиск длины ключа. Можно анализировать распределение частот в зашифрованном тексте с различным прореживанием. То есть брать текст, включающий каждую 2-ю букву зашифрованного текста, потом каждую 3-ю и т. д. Как только распределение частот букв будет сильно отличаться от равномерного (например, по энтропии), то можно говорить о найденной длине ключа.
 2.	Криптоанализ. Совокупность l шифров Цезаря (где l — найденная длина ключа), которые по отдельности легко взламываются.

# Практический пример #

----------


**Задание:**

Необходимо расшифровать данный текст:

> LoatuvftYejeerzAgibeejwzriyazfrkknxefvoxvhanvmsxlizyjzhnxmvhnjwyhnonafjgmiunfrbjxnzrrgfkgearfywv.Bnotfrqgwesiprqzbvotvvgomcumozbklszuqzsypizhslbjtmkngrzggdgpccwkwsiireqk,tsceycoyvuztveu-kwgktrtvthlugvvgggdonafjgmibengdxhaihrj.HnxUtiivfybte’scfgomiunvehnxngtvfbgeutiivfybterneyoggypefjoweyprigatsovrvjowetcrkcomsgcuzsbxmkngj,ovhsotvmsofamenergiaysvfblhrkxpvzrxnie:FWsjNwgsnnoxwejtuv5hnilgcrzbzaeGnalorBnjecvbjxnzNnkwugarUazjkksotlIotditgf.JTkwUkqhzdybtygerrattksjzhnxsyeakwgesqiycgzhgovrkvkfaiozgszbtovrrrbtnzatvknxnotpfakltugrkhogggjbs.HnxktojcsjzegcdlwxxdgtFWsjNetaocsymhkmgfpuedrysrqkmhkdrdotwsgnqtvgelkntvguytne21fkqkgtarlrgcxlrafkcihnzrvsizxtutuvrkoerocdstmoltuvzuvarcbdaagizy.

**Решение:**

Для быстрого и качественного решения используем программу **Cryptools:**
![](http://s58.radikal.ru/i160/1509/b8/02bddb945d22.png)
 Вставляем наш текст в созданное окно:

![](http://s43.radikal.ru/i101/1509/02/23d2dc2fd70f.png)

Выбираем вкладку **Analysis > Symmetric encryption (classic) > Ciphertext-only >  Vigenere**

![](http://s009.radikal.ru/i308/1509/8e/936164db232a.jpg)

Далее программа находит длину ключа :

 ![](http://s017.radikal.ru/i422/1509/77/da2ccf37e4ec.png)

Жмём > **Continue**
Далее находит сам ключ:
 ![](http://i058.radikal.ru/1509/91/240e8ca68347.png)

Жмём> **Decrypt** , и получаем расшифрованный текст:
![](http://s020.radikal.ru/i702/1509/ae/a3cc27d65935.png)

**Расшифрованный текст:**    `SoutdernFederalUnivensityisamodernreoearchuniversitysithemphasisoninjovationsandentrapreneurship.Initoacademicactiviteesitcombinesstuzieswithfundamenpalandappliedsciance,aswellascutteng-edgetechnologeesandinnovativewpproaches.TheUnirersity’spositionenthenationalunirersityrankingspnovidespersuasivaevidencetoitsacdievements,apositeveimageandapasseonforexcellence:OFedUwasawardedtde5thplaceintheAnjualIndependentNwtionalUniversituRankings.SFedUeqqipsitsgraduatessithessentialskihlstogivethemacoipetitiveadvantacewhenitcomestogattingajob.TheknosledgeacquiredatOFedUenablesthempoboldlyfacethedamandsandchallencesofthe21stcenturuaswellastocontrebutetothedevelolmentofthelocalckmmunity`.

**Скачать программу можно здесь:**
[https://www.cryptool.org/en/ct1-downloads](https://www.cryptool.org/en/ct1-downloads)

# Быстрая реализация на Python #

----------

**Шифрование**

```python
m = "ATTACKATDAWN"  # исходное сообщение
k = "LEMON"  # ключ

k *= len(m) // len(k) + 1  # подгоняем ключ
c = ''.join([chr((ord(j) + ord(k[i])) % 26 + ord('A')) for i, j in enumerate(m)])  # шифруем
print(c) # LXFOPVEFRNHR
```

**Расшифровывание**
```python
c = "LXFOPVEFRNHR"  # шифрованое сообщение
k = "LEMON"  # ключ

k *= len(c) // len(k) + 1  # подгоняем ключ
m = ''.join([chr((ord(j) - ord(k[i])) % 26 + ord('A')) for i, j in enumerate(c)])  # расшифровываем
print(m) # ATTACKATDAWN
```
