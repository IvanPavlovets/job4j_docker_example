## Создание Образа, через другой образ + Dockerfile.
Пример создания docker image на основании окружения самого проекта + Dockerfile.
Cам проект представляет собой один единсвеный класс Main.
Окружение этого приложения: java + maven.

## Создаем проект

Проект создается в Idea как обычно - File > New > Project
Тип проекта: Java
Build system:  Maven
Далее добавляем .gitignore и README.


## Maven Shade Plugin

добавляем в pom.xml Maven Shade Plugin,<br>
который собирает скомпилированный код в jar - архив (runnable-jar).
```xml
<finalName>main</finalName>
```
указывает на имя собранного jar.
```xml
<mainClass>ru.job4j.Main</mainClass>
```
mainClass - указывает на точку входа в программу.


## Создаем Dockerfile

В корне проекта создаем файл Dockerfile.

Его содержимое:
```
FROM maven:3.6.3-openjdk-17
RUN mkdir job4j_docker_example
WORKDIR job4j_docker_example
COPY . .
RUN mvn install
CMD ["java", "-jar", "target/main.jar"]
```
_Примечание. Все команды будут выполнятся в среде Linux(Ubuntu),
когда мы склонируем проект в среду Linux._

Описание:<br>

**FROM** указывает образ, который будет использоваться для построения нашего образа.<br>
Их может быть несколько. Мы явно указываем образ Maven и образ JDK.

**RUN** выполняет команду в терминале.<br>
В первую очередь нам нужно создать директорию под проект

**WORKDIR** устанавливает рабочую директорию.<br>
Это значит что все команды терминала будут запускаться из нее

**COPY** производит копирование файлов из хост машины в образ.<br>
Так как мы установили рабочую директорию ранее, то все файлы скопируются в нее

**RUN** mvn package install позволяет нам запустить команду упаковки нашего проекта в jar<br>

**CMD** указывает что мы будем запускать, когда мы будем запускать контейнер.<br>
В данном случае это target/main.jar


## Cоздаем образ на основании Dockerfile

В терминал Linux и установите git client
```
sudo apt-get update
sudo apt-get install git
git --version
```
Клонируем свой проект
```
git clone https://github.com/IvanPavlovets/job4j_docker_example.git
```
Переходим в папку своего проекта
```
cd job4j_docker_example
```
Запускаем команду сборки
```
docker build -t job4j_docker_example .
```
_Примечание: точка в конце команды указывает на каталог расположения Dockerfile_

Проверим, что образ создан
```
docker images
```
![Image of addPost](https://github.com/IvanPavlovets/job4j_docker_example/blob/master/images/images.png)<br>

Запустим образ

```
docker run a6cabd2778ac
```
![Image of addPost](https://github.com/IvanPavlovets/job4j_docker_example/blob/master/images/run.png)<br>