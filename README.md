# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
- Худорожков Георгий Олегович
- РИ211102
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)
## Цель работы
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.
## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.
- Скриншот anaconda.
- ![image](https://user-images.githubusercontent.com/114441283/201139167-e20ec7f6-1ee7-499c-a1e1-d2d6afec6665.png)
- Скриншот скрипт файла
- ![image](https://user-images.githubusercontent.com/114441283/201144469-9c8ff427-361c-4e7e-b9bb-53f029ea3df3.png)
- Скрипт
```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```
- Скриншот юнити
- ![image](https://user-images.githubusercontent.com/114441283/201144879-fa5438a1-da10-4867-bbb8-e85f07a90e8b.png)
-  Анимация.
- ![лаба](https://user-images.githubusercontent.com/114441283/201144131-f013a2a1-7bab-4ec6-a167-086f9d25b016.gif)
## Задание 2
### Подробно опишите каждую строку файла конфигурациинейронной сети, доступного в папке с файлами проекта по ссылке. Самостоятельнонайдите информацию о компонентах Decision Requester, Behavior Parameters, добавленных на сфере.
  ```py
  behaviors:
  rollerball: - имя агента
    trainer_type: ppo - устанавливаем режим обучения (proximal policy optimization).
    hyperparameters: - задаются гиперпараметры.
      batch_size: 10 - количество опытов на каждой итерации для обновления экстремумов функции.
      buffer_size: 100 - количество опыта, которое нужно набрать перед обновлением модели.
      learning_rate: 3.0e-4 - устанавливает шаг обучения (начальная скорость).
      beta: 5.0e-4 - отвечает за случайность действия, повышая разнообразие и иследованность пространства обучения.
      epsilon: 0.2 - порог расхождений между старой и новой политиками при обновлении.
      lambd: 0.99 - определяет авторитетность оценок значений во времени. чем выше значение, тем более авторитетен набор предыдущих оценок.
      num_epoch: 3 - количество проходов через буфер опыта, при выполнении оптимизации.
      learning_rate_schedule: linear - определяет, как скорость обучения изменяется с течением времени, линейно уменьшает скорость.
    network_settings: - определяет сетевые настройки.
      normalize: false - отключается нормализация входных данных.
      hidden_units: 128 - количество нейронов в скрытых слоях сети.
      num_layers: 2 - количество скрытых слоев для размещения нейронов.
    reward_signals: - задает сигналы о вознаграждении.
      extrinsic:
        gamma: 0.99 - коэффициент скидки для будущих вознаграждений.
        strength: 1.0 - шаг для learning_rate.
    max_steps: 500000 - общее количество шагов, которые должны быть выполнены в среде до завершения обучения.
    time_horizon: 64  - количество циклов ml агента, хранящихся в буфере до ввода в модель.
    summary_freq: 10000 - количество опыта, который необходимо собрать перед созданием и отображением статистики.
```



## Выводы

В ходе выполненной лабораторной работы были получены знания о работе ML-агента и его обучении.

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
