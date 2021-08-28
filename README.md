# Maven Learning
Apache Maven - система збирання та управління java-проектами.

## Інсталяція Apache Maven

Для скачування актуальної версії Apache Maven заходимо на [офіційний сайт](https://maven.apache.org/download.cgi "https://maven.apache.org/download.cgi"). У розділі "Files" вибираємо "Binary zip archive" - скачуємо та розпаковуємо архів у довільний каталог. Після цього необхідно додати шлях до директорії Maven у системні змінні "M2_HOME" та "PATH".

Для більшості Linux-систем, які використовують Bash в якості стандартного терміналу, просто додайте наступні рядки до файлу `/home/user_name/.bashrc`
```
# Maven
export M2_HOME=/path_to_maven
export PATH=$M2_HOME/bin:$PATH
```
Після збереження змін (можливо знадобиться перезавантаження) можна перевірити версію Maven за допомогою команди `mvn -v`. Відобразиться інформація про версію Maven та його домашню директорію, а також про версію Java, системну локаль, кодування та ОС.

## Створення нового Maven-проекту

Для створення нового Maven-проекту відкриваємо консоль і вводимо команду `mvn archetype:generate`, після чого необхідно заповнити наступні параметри:
|Параметр|Короткий опис|
|---|---|
|Номер архітипу|Виберіть порядковий номер архітипу який вам підходить, або натисніть `Enter` для вибору стандартного архітипу `maven-archetype-quickstart`.|
|Версія архітипу|Виберіть номер версії архітипу, або натисніть `Enter` для вибору останьої версії.|
|'groupId'|Це ID групи проекту. Найчастіше, це унікальна організація або проект. Наприклад, `org.apache.maven.plugins`.|
|'artifactId'|Це ідентифікатор самого проекту. Найчастіше - його ім'я. Наприклад, `maven-assembly-plugin`. artifactId також допомагає знайти проект в репозиторіі.|
|'version'|Версія проекту. Визначає конкретну версію продукту.|
|'package'|Ім'я стандартного пакету програми. Package зазвичай являє собою обернене доменне ім'я сайту програми. Наприклад, `org.apache.maven`.|
|Підтвердження введених параметрів|Введіть `Y` або натисніть `Enter` для підтвердження введених параметрів та побудови проекту.|

## Структура Maven-проекту

Maven дотримується суворої структури проекту - вихідний код програми, конфігураційні файли, графічні та інші ресурси повинні знаходитися у строго визначених директоріях. Це є вимогою POM-моделі (Project Object Model), яка дозволяє стандартизувати проекти. Основні директорії Maven-проекту описані у таблиці нижче:
|Директорія|Вміст директорії|
|---|---|
|`src/main/java`|Вихідний код програм/бібліотек|
|`src/main/resources`|Ресурси програм/бібліотек|
|`src/main/filters`|Ресурсні фільтри програм/бібліотек|
|`src/main/config`|Конфігураційні файли|
|`src/main/scripts`|Скрипти, необхідні для системних адміністраторів та розробників|
|`src/main/db`|Файли баз даних, наприклад SQL-скрипти|
|`src/main/webapp`|Ресурси web-програм, наприклад .jsp-файли, таблиці стилів, зображення і т.д.|
|`src/test/java`|Вихідний код тестів|
|`src/test/resources`|Ресурси тестів|
|`src/test/filters`|Ресурсні фільтри тестів|
|`src/it`|Інтеграційні тести (в основному для плагінів)|
|`src/assembly`|Дескриптори збирання проекту|
|`src/site`|Файли, які необхідні для генерування сайту проекту|
|`target`|Тимчасові файли та зібрані артефакти|

## Maven POM

POM (Project Object Model) є базовим модулем Maven. Це спеціальний XML-файл, який завжди зберігається в базовій директорії проекту і називається pom.xml. Структура найпростішого POM-файлу подана нижче:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    
    <!-- Інформація про проект -->
    <groupId>com.rutar</groupId>
    <artifactId>Simple_Maven_App</artifactId>
    <packaging>jar</packaging>

</project>
```

POM-файл містить інформацію про проект та різноманітні деталі конфігурації, які Maven використовує для створення проекту. Також POM-файл містить завдання і плагіни. Під час виконання завдань, Maven шукає файл pom.xml в базовій директорії проекту. Він читає його і отримує необхідну інформацію, після чого виконує завдання.

До основних конфігурацій POM-файлу можна віднести наступні:
+ назва проекту
+ версія проекту
+ залежності проекту
+ плагіни
+ завдання
+ профілі збирання
+ інфомація про розробника
+ список репозиторіїв

Усі POM-файли є спадкоємцями батьківського POM. Цей POM-файл називається Super POM - саме у ньому містяться значення, успадковані за замовчуванням.

