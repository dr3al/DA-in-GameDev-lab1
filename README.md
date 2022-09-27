# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Клевцов Андрей Константинович
- 2048873 
* Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ |------------| ------ |
| Задание 1 | *          | 60 |
| Задание 2 | *          | 20 |
| Задание 3 | *          | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

## Задание 1
### Установить Python и написать простую программу
Ход работы:
- Установил Python
- Написал простую программу

```py
print("Hello world!")
```
![image](https://user-images.githubusercontent.com/47189738/192506350-447b01e3-5182-4d09-a052-74038c7299a4.png)

## Задание 2
### Установить Unity и написать простой скрипт
- Установил программное обеспечение
- Написал простой скрипт и прикрепил его к объекту

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class script : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        print("Hello_world!");
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```
![image](https://user-images.githubusercontent.com/47189738/192606204-81a869b0-2b44-4ca3-8a50-88ff70a6df75.png)
## Задание 3
### Пошагово выполнить каждый пункт раздела "ход работы" с описанием и примерами реализации задач
Ход работы
1. Произвести подготовку данных для работы с алгоритмом линейной регрессии. 10 видов данных были установлены случайным образом, и данные находились в линейной зависимости. Данные преобразуются в формат массива, чтобы их можно было вычислить напрямую при использовании умножения и сложения.

```py
import numpy as np
import matplotlib.pyplot as plt

%matplotlib inline

x = [3,21,22,34,54,34,55,67,89,99]
x = np.array(x)
y = [2,22,24,65,79,82,55,130,150,199]
y = np.array(y)

plt.scatter(x,y)
```
![image](https://user-images.githubusercontent.com/47189738/192610050-8e40f9ad-9214-4663-ad72-b1e31f048d11.png)

2. Определите связанные функции. Функция модели: определяет модель линейной регрессии wx+b. Функция потерь: функция потерь среднеквадратичной ошибки. Функция оптимизации: метод градиентного спуска для нахождения частных производных w и b.

```py
def model(a, b, x):
  return a*x + b

def loss_function(a, b, x, y):
  num = len(x)
  prediction=model(a,b,x)
  return (0.5/num) * (np.square(prediction-y)).sum()
  
def optimize(a,b,x,y):
  num = len(x)
  prediction = model(a,b,x)
  da = (1.0/num) * ((prediction -y)*x).sum()
  db = (1.0/num) * ((prediction -y).sum())
  a = a - Lr*da
  b = b - Lr*db
  return a, b
  
def iterate(a,b,x,y,times):
  for i in range(times):
    a,b = optimize(a,b,x,y)
  return a,b
```

3. Начать итерацию
- Шаг 1. Инициализация и модель итеративной оптимизации.

```py
a = np.random.rand(1)
print(a)
b = np.random.rand(1)
print(b)
Lr = 0.000001
a,b = iterate(a,b,x,y,1)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)
```
![image](https://user-images.githubusercontent.com/47189738/192611577-e8bf9de1-92db-4d65-acd5-0e3a38d7a67c.png)

- Шаг 2. На второй итерации отображаются значения параметров, значения.

```py
a,b = iterate(a,b,x,y,2)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)
```
![image](https://user-images.githubusercontent.com/47189738/192611855-d79b22a3-79a3-4af7-8ce0-0bcb2f819b2b.png)

- Шаг 3. Третья итерация показывает значения параметров, значения потерь и визуализацию после итерации.

```py
a,b = iterate(a,b,x,y,3)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)
```
![image](https://user-images.githubusercontent.com/47189738/192612715-4e614b8e-dc9d-49ba-b353-0c021c81c2f7.png)

- Шаг 4. На четвертой итерации отображаются значения параметров, значения потерь и эффекты визуализации.

```py
a,b = iterate(a,b,x,y,4)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)
```
![image](https://user-images.githubusercontent.com/47189738/192612928-49e4372e-1ea7-42fc-8797-3b6f50c9a812.png)

- Шаг 5. Пятая итерация показывает значение параметра, значение потерь и эффект визуализации после итерации.

```py
a,b = iterate(a,b,x,y,5)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)
```
![image](https://user-images.githubusercontent.com/47189738/192614014-daf8dfcd-1e4a-49c7-bcf2-8df2d962e9c3.png)

- Шаг 6. 10000-я итерация, показывающая значения параметров, потери и визуализацию после итерации.

```py
a,b = iterate(a,b,x,y,10000)
prediction=model(a,b,x)
loss = loss_function(a, b, x, y)
print(a,b,loss)
plt.scatter(x,y)
plt.plot(x,prediction)
```
![image](https://user-images.githubusercontent.com/47189738/192614361-dafd6852-766d-49d8-bb07-dfb66cce553a.png)


## Выводы

- Произвел подготовку данных для работы с алгоритмом линейной регрессии.
- Определите связанные функции.
- Посмотрел изменения после нескольких итераций.

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
