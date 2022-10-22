# lab-3
Отчет по лабораторной работе #3 выполнила:


Дементьева Юлия Дмитриевна


РИ-211002

Отметка о выполнении заданий:

Задание 1 *	60


Задание 2	#	20


Задание 3	#	20


знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:


к.т.н., доцент Денисов Д.В.

к.э.н., доцент Панов М.А.

ст. преп., Фадеев В.О.
N|Solid

Build Status

Структура отчета

Данные о работе: 
название работы,фио, группа, 
выполненные задания.

Цель работы.

Задание 1.
Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).

Выводы.


Цель работы:
познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

Задание 1.
Реализовать систему машинного обучения в связке Python – Unity с помощью MLAgents.

Ход работы:

1. Создала новый пустой 3D проект на Unity.

2. Скачала папку с ML агентом.

3. В созданный проект добавила ML Agent.

4. Запустила Anaconda Prompt для возможности запуска команд через консоль, создала виртуальную среду и ввела следующие команды для создания и активации нового ML-агента, а также для скачивания необходимых библиотек:

conda create -n MLAGENT python=3.6.13

conda activate MLAGENT

pip install mlagents==0.28.0

pip install torch~=1.7.1 -f https://download.pytorch.org/whl/torch_stable.html

Перешела в папку с unity-проектом cd /d H:\ВУЗ\3 семестр\Дата сайнс в примерах и задачах\lab-3\MLAtest-lab-3

5. Создала на сцене плоскость, куб и сферу.

![2022-10-23 (2)](https://user-images.githubusercontent.com/114353535/197358543-5764bc27-f498-49e3-977f-f7c4213a84b2.png)


6. Создала простой C# скрипт-файл, подключила его к сфере.

Скрипт:

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


7. Объекту «сфера» добавила компоненты Rigidbody, Decision Requester, Behavior Parameters, настроила их.


8. В корень проекта добавила файл конфигурации нейронной сети rollerball_config.yaml.

9. Запустила работу ml-агента: mlagents-learn rollerball_config.yaml --run-id=RollerBall --force

10. Вернулась в проект Unity, запустила сцену, проверила работу ML-Agent’a.

11. Сделала 3/9/27 копии/копий модели «Плоскость-Сфера-Куб», запустила симуляцию сцены и наблюдала за результатом обучения модели.

![2022-10-23 (6)](https://user-images.githubusercontent.com/114353535/197358856-70e71094-1e35-4851-8345-34c137ca3b54.png)

![2022-10-23 (13)](https://user-images.githubusercontent.com/114353535/197359157-989fd6be-4ded-442e-ac7a-fcb2863618e8.png)


![2022-10-23 (7)](https://user-images.githubusercontent.com/114353535/197358900-3ea0ec5e-ccef-469f-918e-3bdce42edc66.png)

![2022-10-23 (8)](https://user-images.githubusercontent.com/114353535/197358940-b1375573-b3eb-4eb6-b3e8-55d9d0db876a.png)


12. После завершения обучения проверила работу модели. 

![2022-10-23 (24)](https://user-images.githubusercontent.com/114353535/197359232-ea822f11-ec9c-4c86-b20f-111ba09b86ed.png)

![2022-10-23 (25)](https://user-images.githubusercontent.com/114353535/197359260-cf31153f-03bb-4e8c-9f7e-da68227bb26b.png)

![2022-10-23 (26)](https://user-images.githubusercontent.com/114353535/197359313-ff8f7adb-12b7-492c-aa23-e9ce5819ed80.png)

![2022-10-23 (27)](https://user-images.githubusercontent.com/114353535/197359335-ab2f1ce5-f05d-46c7-9b6d-3228ba84b7b3.png)

![2022-10-23 (28)](https://user-images.githubusercontent.com/114353535/197359367-0f39b7e0-31f9-46b9-b332-f33d42f52f16.png)

![2022-10-23 (29)](https://user-images.githubusercontent.com/114353535/197359391-b8e6f82e-acc2-4de6-8818-345810219d88.png)

![2022-10-23 (30)](https://user-images.githubusercontent.com/114353535/197359415-6f094e82-8d13-40d4-b856-a63f602b1a89.png)


Выводы:
В результате проделанной работы я научилась пользоваться ml-агентом, узнала, что такое машинное обучение и опробовала его в деле.

Powered by
BigDigital Team: Denisov | Fadeev | Panov
