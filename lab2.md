# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
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
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity
## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity.
Ход работы:
- Реализовал запись данных из скрипта на Python в Google Sheets

```py
import gspread
import numpy as np
gc = gspread.service_account(filename='unity-364106-24951b8b63a1.json')
sh = gc.open("UnityTable")
price = np.random.randint(2000, 10000, 11)
mon = list(range(1,11))
i = 0
for i in range(1,12):
    tempInf = ((price[i-1]-price[i-2])/price[i-2])*100
    tempInf = str(tempInf)
    tempInf = tempInf.replace('.', ',')
    sh.sheet1.update(('A' + str(i)), str(i))
    sh.sheet1.update(('B' + str(i)), str(price[i-1]))
    sh.sheet1.update(('C' + str(i)), str(tempInf))
    print(tempInf)
```
- Результат работы скрипта
![image](https://user-images.githubusercontent.com/47189738/193554025-ee158bd1-2f7a-45e9-9e49-57bd41ad017d.png)

- Появившиеся данные в таблице
![image](https://user-images.githubusercontent.com/47189738/193554337-6418914f-ff1a-4fc4-a17c-32c6b7351626.png)

- Реализовал получение данных из таблицы на Unity

```csharp
IEnumerator GoogleSheets()
    {
        UnityWebRequest currentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1b3TTGpu8J1ilywo-XcQjDfggJIDo1ETaJrrlKKnaa7s/values/Лист1?key=token");
        yield return currentResp.SendWebRequest();
        string rawResp = currentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var ItemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(ItemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }
```
- Реализовал воспроизведение аудио в зависимости от полученных из таблицы данных

```csharp
void Update()
    {
        if (i-1 != dataSet.Count & dataSet.ContainsKey("Mon_" + i.ToString()) & statusStart==false)
        {
            if (dataSet["Mon_" + i.ToString()] <= 10)
            {
                StartCoroutine(PlaySelectAudioGood());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
            }

            else if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100)
            {
                StartCoroutine(PlaySelectAudioNormal());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
            }

            else if (dataSet["Mon_" + i.ToString()] >= 100)
            {
                StartCoroutine(PlaySelectAudioBad());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
            }
        }
    }
    
IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(2);
        statusStart = false;
        i++;
    }

    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
```
- Результат работы
![image](https://user-images.githubusercontent.com/47189738/193557520-87953072-12b6-43bb-842d-578b0b538029.png)

## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1
- Переписал функцию iterate() из лабораторной работы №1, чтобы она каждую n-ую итерацию выводила в таблицу значение loss и разницу с предыдущим значением.

```py
def iterate(a, b, x, y, times):
    prevLoss, loss = 0, 0
    for i in range(times):
        a, b = optimize(a, b, x, y)
        if i==0:
            loss = loss_function(a, b, x, y)
            sh.sheet1.update(('A' + str(i // 10 + 1)), str(i+1))
            sh.sheet1.update(('B' + str(i // 10 + 1)), str(loss))
        elif times <= 300 and i%10==0:
            prevLoss, loss = loss, loss_function(a, b, x, y)
            sh.sheet1.update(('A' + str(i // 10 + 1)), str(i))
            sh.sheet1.update(('B' + str(i // 10 + 1)), str(loss))
            sh.sheet1.update(('C' + str(i // 10 + 1)), str(prevLoss - loss))
        elif times > 300 and times <= 3000 and i%100==0:
            prevLoss, loss = loss, loss_function(a, b, x, y)
            sh.sheet1.update(('A' + str(i//100 + 1)), str(i))
            sh.sheet1.update(('B' + str(i//100 + 1)), str(loss))
            sh.sheet1.update(('C' + str(i//100 + 1)), str(prevLoss-loss))
        elif times > 3000 and i%1000==0:
            prevLoss, loss = loss, loss_function(a, b, x, y)
            sh.sheet1.update(('A' + str(i // 1000 + 1)), str(i))
            sh.sheet1.update(('B' + str(i // 1000 + 1)), str(loss))
            sh.sheet1.update(('C' + str(i // 1000 + 1)), str(prevLoss - loss))
    return a, b
```
![image](https://user-images.githubusercontent.com/47189738/194012131-17b93b96-c5fa-42fe-9ca7-c261959e63ae.png)

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2

```csharp
void Update()
    {
        if (i-1 != dataSet.Count & dataSet.ContainsKey("Mon_" + i.ToString()) & statusStart==false)
        {
            if (dataSet["Mon_" + i.ToString()] <= 105)
            {
                StartCoroutine(PlaySelectAudioGood());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
            }

            else if (dataSet["Mon_" + i.ToString()] > 105 & dataSet["Mon_" + i.ToString()] < 180)
            {
                StartCoroutine(PlaySelectAudioNormal());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
            }

            else if (dataSet["Mon_" + i.ToString()] >= 180)
            {
                StartCoroutine(PlaySelectAudioBad());
                Debug.Log(dataSet["Mon_" + i.ToString()]);
            }
        }
    }
```
- Немного изменил границы значений для каждого звука

## Выводы

- Реализовал связку Python - Google Sheets - Unity 
- Звуковое ранжирование данных в Unity

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
