# Взлом Rail fence cipher

Шифр Rail fence относится к перестановочным шифрам и взламывается методом перебора. Для реализации можно использовать инструмент RailFencePython, модифицировав тестовый файл testRailFence.py

## Пример

Имеется зашифрованная строка:  
> AaY--rpyfneJBeaaX0n-,ZZcs-uXeeSVJ-sh2tioaZ}slrg,-ciE-anfGt.-eCIyss-TzprttFliora{GcouhQIadctm0ltt-FYluuezTyorZ-

```python
 
	from railFence import decryptRailFence  
	for i in xrange(1,255):  
		print decryptRailFence("AaY--rpyfneJBeaaX0n-,ZZcs-uXeeSVJ-sh2tioaZ}slrg,-ciE-anfGt.-eCIyss-TzprttFliora{GcouhQIadctm0ltt-FYluuezTyorZ-", i, 0);   
```

После запуска, просматривая вывод, можно найти строку:

>  A-fence-is-a-structure-that-encloses-an-area,-SharifCTF{QmFzZTY0IGlzIGEgZ2VuZXJpYyB0ZXJt},-typically-outdoors.

----
взято с сайта http://itsecwiki.org/