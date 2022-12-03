# LR-18[LR-18.zip](https://github.com/Maxsim2418/LR-18/files/10146052/LR-18.zip)

Борнеман М.А

ЭВТ-70

Игровой движок:Unity

Лабораторная работа №18

Тема: Разработка игрового проекта змейка.

Цель: приобрести навыки в разработке игрового проекта змейка

Ход работы:

1.	Выполнение работы

1.	Были созданы спрайты еды и самой змейки.

![image](https://user-images.githubusercontent.com/119674602/205433279-fe09961b-cfb2-4807-a5e0-8b6200a4a110.png)

Рисунок 18.1 спрайты змейки и еды

2.	Был создан скрипт для передвижения, змейки.

Листинг 18.1 Snakes.cs

using System.Collections.Generic;
using UnityEngine;

[RequireComponent(typeof(BoxCollider2D))]
public class Snake : MonoBehaviour
{
    private List<Transform> segments = new List<Transform>();
    public Transform segmentPrefab;
    public Vector2 direction = Vector2.right;
    private Vector2 input;
    public int initialSize = 4;

    private void Start()
    {
        ResetState();
    }

    private void Update()
    {

        if (direction.x != 0f)
        {
            if (Input.GetKeyDown(KeyCode.W) || Input.GetKeyDown(KeyCode.UpArrow))
            {
                input = Vector2.up;
            }
            else if (Input.GetKeyDown(KeyCode.S) || Input.GetKeyDown(KeyCode.DownArrow))
            {
                input = Vector2.down;
            }
        }

        else if (direction.y != 0f)
        {
            if (Input.GetKeyDown(KeyCode.D) || Input.GetKeyDown(KeyCode.RightArrow))
            {
                input = Vector2.right;
            }
            else if (Input.GetKeyDown(KeyCode.A) || Input.GetKeyDown(KeyCode.LeftArrow))
            {
                input = Vector2.left;
            }
        }
    }

    private void FixedUpdate()
    {

        if (input != Vector2.zero)
        {
            direction = input;
        }


        for (int i = segments.Count - 1; i > 0; i--)
        {
            segments[i].position = segments[i - 1].position;
        }


        float x = Mathf.Round(transform.position.x) + direction.x;
        float y = Mathf.Round(transform.position.y) + direction.y;

        transform.position = new Vector2(x, y);
    }

    public void Grow()
    {
        Transform segment = Instantiate(segmentPrefab);
        segment.position = segments[segments.Count - 1].position;
        segments.Add(segment);
    }

    public void ResetState()
    {
        direction = Vector2.right;
        transform.position = Vector3.zero;


        for (int i = 1; i < segments.Count; i++)
        {
            Destroy(segments[i].gameObject);
        }


        segments.Clear();
        segments.Add(transform);


        for (int i = 0; i < initialSize - 1; i++)
        {
            Grow();
        }
    }

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.gameObject.CompareTag("Food"))
        {
            Grow();
        }
        else if (other.gameObject.CompareTag("Obstacle"))
        {
            ResetState();
        }
    }

}

3.	Был создан скрипт для собирания еды змейкой.

Листинг 18.2 Food.cs

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Food : MonoBehaviour
{
    public BoxCollider2D gridArea;

    private void Start()
    {
        RandomizePosition();
    }

    private void RandomizePosition()
    {
        Bounds bounds = this.gridArea.bounds;

        float x = Random.Range(bounds.min.x, bounds.max.x);
        float y = Random.Range(bounds.min.y, bounds.max.y);

        this.transform.position = new Vector3(Mathf.Round(x), Mathf.Round(y), 0.0f);
    }


    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.tag == "Player") { 
            RandomizePosition();
        }
    }
}

4.	Были созданы сегменты для увеличения змейки.

![image](https://user-images.githubusercontent.com/119674602/205433285-fc77cb3b-b0b0-41ff-873f-1fe8da7cee32.png)

Рисунок 18.2 сегменты змейки

5.	Были созданы стены, GameTag  “Obstacle” и привязан “Obstacle” к стенам змейке.

![image](https://user-images.githubusercontent.com/119674602/205433298-d5795825-58d7-4827-9296-b82bc0f5274b.png)

Рисунок 18.3 кадр из игры

2.	Вывод

В ходе проделанной работы был разработан игровой проект змейка.
