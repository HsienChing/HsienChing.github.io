---
title: "關於壓力單位並且練習自行換算 <br> About pressure unit and practice converting on our own"
date: 2025-02-24 17:00:00 +0800
excerpt: "關於壓力單位，我們國中時，大概就是背 \"1個大氣壓約等於76公分水銀柱高\"。但壓力單位其實有很多種。這裡備忘一下幾個常用的壓力單位。並且練習一下自行換算方式。"
categories:
  - Physics
  - 物理
tags:
  - Unit
  - 單位
---

在台灣，我們國中時，大概就是背 "1個大氣壓約等於76公分水銀柱高"

但壓力單位其實有很多種。

這裡備忘一下幾個常用的壓力單位。

也不用列太多，會記不住，而且有可能會亂掉。


# bar

Unit system: Metric system  
Symbol: bar

1 bar =  
100 kPa (SI unit)  
100,000 Pa = 10<sup>5</sup> Pa (SI unit)  

1 bar ≈  
0.98692327 atm  
14.503774 psi  
10206.4 mmH<sub>2</sub>O  
29.529983 inHg  
750.06158 mmHg  
750.06168 Torr

Wikipedia: Bar (unit)  
<https://en.wikipedia.org/wiki/Bar_(unit)>


# psi, pound per square inch

Unit system: Imperial units, US customary units  
Symbol:	psi, lbf/in<sup>2</sup>

1 psi ≈ 6.89 kPa

1 psi ≈  
6.894757 kPa ≈ 6.89 kPa (SI unit)  
0.006894757 Pa ≈ 6.89 × 10<sup>-3</sup> Pa (SI unit)  
0.06894757 bar ≈ 6.89 × 10<sup>-2</sup> bar 

Wikipedia: Pound per square inch  
<https://en.wikipedia.org/wiki/Pound_per_square_inch>


# Pa, pascal

Unit system: SI  
Symbol: Pa

定義

1 Pa = 1 N/m<sup>2</sup> = 1 (kg⋅m/s<sup>2</sup>)/m<sup>2</sup> = 1 kg/(m⋅s<sup>2</sup>) = 1 J/m<sup>3</sup>

Wikipedia: Pascal (unit)  
<https://en.wikipedia.org/wiki/Pascal_(unit)>


# 線上壓力單位換算機

網路上也有很多壓力單位計算機，隨便Google一下就有。

下面列出幾個當參考

unitconverters.net - Pressure Converter
<https://www.unitconverters.net/pressure-converter.html>


Panasonic - Conversion of pressure unit
<https://industrial.panasonic.com/ww/presureunit>


# 自行練習壓力單位換算

獻慶在看某工程文件時，看到

> 9.8 kPa = 1 metre of head  
> 43 psi = 100 feet of head

如果硬要翻譯成中文，那就會變成這樣  
9.8 kPa 等於 1公尺高的水壓  
43 psi 等於 100英尺高的水壓

要背下來也行啦! 只是我的記憶力，那就是一整個爛。

那麼該如何去感受這件事情?

就來做個基本換算，增加單位的實用感。

## 關於 "9.8 kPa = 1 metre of head"

公制單位，比較好換算

1公尺高的水，水壓有多大?

回答這個問題，那就建立一個簡單模型，把面積也設定下去。問題變成，在1m<sup>2</sup>面積下，1公尺高的水，水壓多大?

1立方公尺的水，體積為 1 m<sup>3</sup>

設定水的密度為 1 kg/l = 1000 kg/m<sup>3</sup> (這裡說 "設定" 是因為，你去查一下就知道不是1，而是很接近1的數字，設定1會比較好計算)

1立方公尺的水，重量為 1000 kg  
( 1 m<sup>3</sup> × 1000 kg/m<sup>3</sup> = 1000 kg )

接下來要把 kg 換成 N

在地球表面時， 1 kg 約等於 g<sub>0</sub> N，其中 g<sub>0</sub> 為標準重力加速度 (standard acceleration of gravity)，數值為 g<sub>0</sub> = 9.80665 m/s<sup>2</sup>。

1立方公尺的水，在地球表面時，為 9806.65 N  
( 1000 kg × 9.80665 N/kg = 9806.65 N )

9806.5 N 在 1 m<sup>2</sup> 面積上產生的壓力為 9806.65 N/m<sup>2</sup>  
( 9806.5 N / 1 m<sup>2</sup> = 9806.65 N/m<sup>2</sup> )

而之前有講到 1 Pa = 1 N/m<sup>2</sup>

9806.65 N/m<sup>2</sup> = 9806.65 Pa = 9.80665 kPa ≈ 9.8 kPa

這樣就換算到 "9.8 kPa 等於 1公尺高的水壓"

Wikipedia: Standard gravity  
<https://en.wikipedia.org/wiki/Standard_gravity>


## 關於 "43 psi = 100 feet of head"

這個還是偷懶一下，直接用查表對照來進行換算。

查表得到 1 psi = 703.704 mmH<sub>2</sub>O  
查表得到 1 ft = 30.48 cm = 304.8 mm

接下來，簡單換算一下  
43 psi  
= 703.704 mmH<sub>2</sub>O × 43  
= 30259.272 mmH<sub>2</sub>O  
= (30259.272/304.8) ftH<sub>2</sub>O  
= 99.276 ftH<sub>2</sub>O

確實 "43 psi 將近 100英尺高的水壓"


<!--
FB: 

Twitter: 

-->
