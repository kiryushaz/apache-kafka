# ЛР 5. Apache Kafka. Producer-Consumer
### Задача 

Запустить Kafka.

Создать Topic.

Написать два приложения на любом удобном языке программирования. Первое пишет 1000 сообщений - дата и время создания сообщения в Topic Kafka. Второе читает сообщение из Topic и выводит поличество прочитанных сообщений на экран.

### Отчет

Для выполнения лабораторной работы использовалась среда Windows Subsystem for Linux.

Создаем папку проекта в любом удобном месте, например:
```
mkdir ~/kafka-python-getting-started && cd ~/kafka-python-getting-started
```

Создаем виртуальное окружение Python в папке проекта, затем запускаем его:
```
virtualenv env

source env/bin/activate
```

Создаем файл конфигурации `getting_started.ini`:
```
[default]
bootstrap.servers=localhost:9092

[consumer]
group.id=python_example_group_1

# 'auto.offset.reset=earliest' to start reading from the beginning of
# the topic if no committed offsets exist.
auto.offset.reset=earliest
```

Устанавливаем библиотеку `confluent-kafka`:
```
pip install confluent-kafka
```

Создаем приложения в `producer.py` и `consumer.py`. Готовый код можно посмотреть в этом репозитории.

Скачиваем последнюю версию Confluent CLI отсюда: https://github.com/confluentinc/cli/releases/latest

Распаковываем архив.

Перемещаем `confluent.exe` в папку с проектом.

Запускаем контейнер с kafka на 9092 порту:
```
./confluent.exe local kafka start --plaintext-ports 9092
```

Создаем Topic с названием highload:
```
./confluent.exe local kafka topic create highload
```

Запускаем producer.py:
```
./producer.py getting_started.ini
```

Результат:
```
Produced event to topic highload: key = Message №0   value = 2023-11-22T16:40:24.478070
Produced event to topic highload: key = Message №1   value = 2023-11-22T16:40:24.528371
Produced event to topic highload: key = Message №2   value = 2023-11-22T16:40:24.578553
Produced event to topic highload: key = Message №3   value = 2023-11-22T16:40:24.628866
Produced event to topic highload: key = Message №4   value = 2023-11-22T16:40:24.679183
Produced event to topic highload: key = Message №5   value = 2023-11-22T16:40:24.729495
Produced event to topic highload: key = Message №6   value = 2023-11-22T16:40:24.779753
Produced event to topic highload: key = Message №7   value = 2023-11-22T16:40:24.829986
Produced event to topic highload: key = Message №8   value = 2023-11-22T16:40:24.880177
Produced event to topic highload: key = Message №9   value = 2023-11-22T16:40:24.930427
...
Produced event from topic highload: key = Message №990 value = 2023-11-22T16:41:14.287980
Produced event from topic highload: key = Message №991 value = 2023-11-22T16:41:14.338356
Produced event from topic highload: key = Message №992 value = 2023-11-22T16:41:14.388700
Produced event from topic highload: key = Message №993 value = 2023-11-22T16:41:14.438970
Produced event from topic highload: key = Message №994 value = 2023-11-22T16:41:14.489323
Produced event from topic highload: key = Message №995 value = 2023-11-22T16:41:14.539728
Produced event from topic highload: key = Message №996 value = 2023-11-22T16:41:14.590093
Produced event from topic highload: key = Message №997 value = 2023-11-22T16:41:14.640443
Produced event from topic highload: key = Message №998 value = 2023-11-22T16:41:14.690739
Produced event from topic highload: key = Message №999 value = 2023-11-22T16:41:14.741034
```

Запускаем consumer.py:
```
./consumer.py getting_started.ini
```

Результат:
```
Consumed event to topic highload: key = Message №0   value = 2023-11-22T16:40:24.478070
Consumed event to topic highload: key = Message №1   value = 2023-11-22T16:40:24.528371
Consumed event to topic highload: key = Message №2   value = 2023-11-22T16:40:24.578553
Consumed event to topic highload: key = Message №3   value = 2023-11-22T16:40:24.628866
Consumed event to topic highload: key = Message №4   value = 2023-11-22T16:40:24.679183
Consumed event to topic highload: key = Message №5   value = 2023-11-22T16:40:24.729495
Consumed event to topic highload: key = Message №6   value = 2023-11-22T16:40:24.779753
Consumed event to topic highload: key = Message №7   value = 2023-11-22T16:40:24.829986
Consumed event to topic highload: key = Message №8   value = 2023-11-22T16:40:24.880177
Consumed event to topic highload: key = Message №9   value = 2023-11-22T16:40:24.930427
...
Consumed event from topic highload: key = Message №990 value = 2023-11-22T16:41:14.287980
Consumed event from topic highload: key = Message №991 value = 2023-11-22T16:41:14.338356
Consumed event from topic highload: key = Message №992 value = 2023-11-22T16:41:14.388700
Consumed event from topic highload: key = Message №993 value = 2023-11-22T16:41:14.438970
Consumed event from topic highload: key = Message №994 value = 2023-11-22T16:41:14.489323
Consumed event from topic highload: key = Message №995 value = 2023-11-22T16:41:14.539728
Consumed event from topic highload: key = Message №996 value = 2023-11-22T16:41:14.590093
Consumed event from topic highload: key = Message №997 value = 2023-11-22T16:41:14.640443
Consumed event from topic highload: key = Message №998 value = 2023-11-22T16:41:14.690739
Consumed event from topic highload: key = Message №999 value = 2023-11-22T16:41:14.741034
Waiting...
Waiting...
^C1000 messages read.
```

Останавливаем контейнер:
```
./confluent.exe local kafka stop
```
