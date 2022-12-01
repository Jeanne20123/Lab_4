# Перцептрон
Отчет по лабораторной работе #4 выполнил(а):
- Борщевская Жанна Александровна
- РИ-210911
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;
## Цель работы
Познакомиться и закрепить знания об перцептроне
## Ход работы
Создайте пустой проект на Unity.
Добавьте пустой объект Empty Game Object с именем Perceptron.
Подключите Perceptron.cs к пустому Game Object с именем Perceptron.

## Задание 1
В проекте юнити реализовать перцептрон, который умеет производить вычисления:
а) OR
![а](https://user-images.githubusercontent.com/114568072/205083878-3738ab40-8fbe-46fc-b103-91c2ab5cebb6.jpg)
Работает корректно уже после 4 итераций обучения.
b) AND
![b](https://user-images.githubusercontent.com/114568072/205084517-139f9d5b-b515-4d6a-9672-f90ef651bb8d.jpg)
Работает корректно после 5 итераций обучения.
c) NAND 
![c](https://user-images.githubusercontent.com/114568072/205084801-28a6a336-c95a-420b-a5af-0a561fedd026.jpg)
Работает корректно после 5 итераций обучения.
d) XOR
![d](https://user-images.githubusercontent.com/114568072/205085043-a6ab3563-f505-400b-ac08-2d0724e3e7a1.jpg)
Не удалось обучить систему производить вычисления XOR.

Perceptron.cs
```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}
	
		double CalcOutput(double i1, double i2)
		{
			double[] inp = new double[] {i1, i2};
			double dp = DotProductBias(weights,inp);
			if(dp > 0) return(1);
			return (0);
		}
	
	void Train(int epochs)
		{
			InitialiseWeights();

			for(int e = 0; e < epochs; e++)
			{
				totalError = 0;
				for(int t = 0; t < ts.Length; t++)
				{
					UpdateWeights(t);
					Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
				}
				Debug.Log("TOTAL ERROR: " + totalError);
			}
		}

		void Start () {
			Train(10);
			
			Debug.Log("Test 0 0: " + CalcOutput(0,0));
			Debug.Log("Test 0 1: " + CalcOutput(0,1));
			Debug.Log("Test 1 0: " + CalcOutput(1,0));
			Debug.Log("Test 1 1: " + CalcOutput(1,1));		
			
		}

	void Update () {
		
	}
}
```
## Выводы
В данной лабораторной работе я обучила перцептрон, который научился производить простейшие вычисления.
