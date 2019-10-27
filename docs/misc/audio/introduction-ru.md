[EN](./introduction.md) | [ZH](./introduction-zh.md) | [RU](./introduction-ru.md)
Темы CTF, связанные с аудио, в основном используют стеганографические методы разрешения. Они в основном делятся на: стеганографию в MP3, LSB, waveform-стеганографию, спектровую, и т.д.


## Общие понятия и средства


Информация, которую можно найти через `binwalk` и `strings`, не детализирована.


## MP3-стеганография


### Принцип


MP3-стеганография в основном осуществляется инструментом [Mp3Stego] (http://www.petitcolas.net/steganography/mp3stego/). Основные принципы и инструкции:


> MP3Stego спрячет информацию в MP3-файлах во время процесса сжатия. Данные сначала будут сжаты, зашифрованы, а затем запрятаны в битовый поток MP3.



```shell

encode -E hidden_text.txt -P pass svega.wav svega_stego.mp3

decode -X -P pass svega_stego.mp3

```



### Пример


> ISCC-2016: Music Never Sleep



После первичного изучения в `strings` ничего не нашлось, как и при прослушивании аудио. Значит, данные были спрятаны стеганографически.


![](./figure/1.jpg)



После получения пароля для расшифровки надо было использовать `Mp3Stego`.


```shell

decode.exe -X ISCC2016.mp3 -P bfsiscc2016

```



Получаем файл `iscc2016.mp3.txt`:
```

Flag is SkYzWEk0M1JOWlNHWTJTRktKUkdJTVpXRzVSV0U2REdHTVpHT1pZPQ== ???

```



Флаг открывается после Base64 &amp;&amp; Base32.


## Waveform


### Принцип


В общем случае, в направлении формы сигнала, после наблюдения аномалии, соответствующее программное обеспечение (Audacity, Adobe Audition и т.д.) используется для наблюдения за законом формы сигнала, и форма сигнала дополнительно преобразуется в строку 0, 1 и т.д., тем самым мы извлекаем и получаем окончательный флаг.


### Example


> ISCC-2017: Misc-04



In fact, the hidden information in this question is in the first part of the audio. If you don&#39;t listen carefully, you may mistake it for steganography.


![](./figure/3.png)



The high is 1 low and 0 is converted to get the `01` string.


```

110011011011001100001110011111110111010111011000010101110101010110011011101011101110110111011110011111101

```



Конвертируем в ASCII, расшифровываем пароль-морзянку и получаем флаг.


!!! note

Some of the more complicated ones may first perform a series of processing on the audio, such as filtering. For example [JarvisOJ - Voice of God Writeup] (https://www.40huo.cn/blog/jarvisoj-misc-writeup.html)


## Spectrum


### Principle


Спектральная стеганография прячет информацию в спектре. Такое аудио обычно отличается по звучанию, звук более бормочущий или жёсткий. 


### Example


> Su-ctf-quals-2014:hear_with_your_eyes



![](./figure/4.png)



## LSB audio steganography


### Principle


Да, этот метод работает не только с картинками, но и с аудио. Программа [Silenteye] (http://silenteye.v1kings.io/) может быть использована следующим образом:


> SilentEye - кросс-платформенное приложение, делающее стеганографию несложной. В данном случае сообщения закладываются в изображения или звук. Вам предлагается симпатичный интерфейс и простая интеграция новых стеганографических алгоритмов и криптографии с помощью системы плагинов.


### Example


&gt; 2015 Guangdong Strong Net Cup - Little Apple


Just use `slienteye`.


![](./figure/2.jpg)



## Extension


- [LSB in Audio] (https://ethackal.github.io/2015/10/05/derbycon-ctf-wav-steganography/)
- [Stealth Summary] (http://bobao.360.cn/learning/detail/243.html)
