# Stegsolve
## Описание ##
Stegsolve - open source решение для анализа структуры файлов,изучения устройства стереограммы, а также самостоятельного рассмотрения каждой разрядной матрицы.
## Установка ###
* Для Unix систем  
wget http://www.caesum.com/handbook/Stegsolve.jar -O stegsolve.jar  
chmod +x stegsolve.jar  
mkdir bin  
mv stegsolve.jar bin/  

* Для Windows систем  
Скачиваем Stegsolve с
http://www.caesum.com/handbook/Stegsolve.jar  
Для его работы также необходимо установить Java Runtime Environment  
http://goo.gl/LKvNof


## Примеры использования ##
### XOR ###
1. Даны два изображения  
<img src='img/1.jpg'> <img src='img/2.jpg'>  

2. Открываем первое изображение, идем в Меню -> Analyse -> Image Combiner и выбираем второе изображение.  
3. В режиме XOR видим флаг AZADI TOWER  
<img src='img/4.jpg'>.

### Анализ данных ###
1. Дано изображение  
<img src='img/3.jpg'>
2. Воспользуемся инструментом File format, находящимся во в кладке меню Analyse  
3. После того как сама картинка в файле кончается, за ней следует текст с флагом  
<img src='img/5.jpg'>.

### Каналы изображения ###
1. Дано изображение  
<img src='img/6.png'>
2. При переключении каналов замечаем, что на одном из режимов присутствует часть слова "Flag"  
<img src='img/7.jpg'>
3. Проверяя оставшиеся каналы, находим режим, при котором возможно прочитать запись целиком  
<img src='img/8.jpg'>

### Извлечение данных ###
1. Дано изображение  
<img src='img/9.png'>
2. Производим анализ каналов изображения. Замечаем, что в режимах Red/Blue/Green Plane 0 исходное изображение полностью отсутствует  
<img src='img/10.jpg'>
3. Меню -> Analyse -> Data Extract  
Выбрав любой из приведенных выше режимов, нажимаем Preview. В окне Extract Preview видим флаг  
<img src='img/11.jpg'>
