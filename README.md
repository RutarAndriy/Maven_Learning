# Maven Learning
Apache Maven - система збирання та управління java-проектами.

## Зміст  

+ [Інсталяція Apache Maven]()
+ [Створення нового Maven-проекту]()
+ [Структура Maven-проекту]()
+ [Maven POM]()
  + [Основні теги POM-файлу]()
  + [Використання змінних у POM-файлі]()
+ [Залежності Maven]()
  + [Транзитивні залежності]()
  + [Виключення залежностей]()
  + [Області видимості залежностей]()
  + [Ручне встановлення залежностей]()
+ [Репозиторії Maven]()
+ [Профілі збирання Maven]()
+ [Плагіни Maven]()
+ [Етапи збирання Maven-проекту]()
+ [Основні команди Maven]()

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

Усі POM-файли є спадкоємцями батьківського POM. Цей POM-файл називається SuperPOM - саме у ньому містяться значення, успадковані за замовчуванням.

### Основні теги POM-файлу

POM-файл використовує xml-теги для конфігурації пректу. Головним тегом проекту є `<project>`. Він містить внутрішні теги, кожен з яких відповідає за налаштування конкретного параметру проекту.

Теги бувають обов'язковими та необов'язковими. Обов'язкові теги визначають унікальні властивості, які не можуть бути успадкованими, наприклад тег `<artifactId>` задає унікальне ім'я проекту. Необов'язкові теги наслідуються від SuperPOM, якщо вони не визначені користувачем явно.

Опис основних тегів POM-файлу подано у наступній таблиці:

|Теги|Короткий опис|
|---|---|
|`<project>`|Обов'язковий кореневий тег проекту|
|`...` > `<modelVersion>`|Версія POM-моделі, обов'язковий тег|
|`...` > `<groupId>`|ID групи проекту, обов'язковий тег|
|`...` > `<artifactId>`|Назва проекту, обов'язковий тег|
|`...` > `<version>`|Версія проекту, обов'язковий тег|
|`...` > `<packaging>`|Тип контейнера проекту (jar, pom, maven-plugin і т.д.)|
|`...` > `<dependencies>`|Кореневий тег, містить список залежностей|
|`...` > `...` > `<dependency>`|Конкретно визначена залежність проекту|
|`...` > `<build>`|Кореневий тег, містить параметри збирання проекту|
|`...` > `...` > `<finalName>`|Кінцеве ім'я зібраного контейнера проекту|
|`...` > `...` > `<plugins>`|Кореневий тег, містить список задіяних плагінів|
|`...` > `<properties>`|Кореневий тег, містить список змінних проекту|
|`...` > `...` > `<project.build.sourceEncoding>`|Кодування проекту, наприклад `UTF-8`|
|`...` > `...` > `<maven.compiler.source>`|Версія вихідного коду проекту|
|`...` > `...` > `<maven.compiler.target>`|Версія java-компілятора|

Версія проекту `<version>` задається у вигляді `2.15.3-beta2`, де:

|Елемент|Короткий опис|
|--:|---|
|`2`|Мажорна версія, змінюється після глобальних змін у проекті|
|`15`|Мінорна версія, змінюється після виправлення значних помилок або при додаванні нового функціоналу|
|`3`|Допоміжна версія, змінюється після виправлення незначних помилок та багів|
|`beta2`|Стан версії, вказує на ступінь завершеності проекту|

|Стан версії|Короткий опис|
|:-:|---|
|`SNAPSHOT`|Проект знаходиться на етапі активної розробки і може містити багато помилок та багів|
|`alpha`|Етап внутрішнього тестування, можливе додавання нових функціональних можливостей|
|`beta`|Етап публічного тестування та кінцевого налагодження. Виправляються незначні баги|
|`rc`|Проект пройшов усі комплексні тести і готується до стабільного релізу|
|`stable`|Стабільна версія проекту|
|`final`|Фінальна версія проекту, яка більше не буде оновлюватися|

SNAPSHOT - це спеціальна версія, яка показує поточну робочу копію. При кожній збірці Maven перевіряє наявність нової snapshot-версії на віддаленому репозиторії.

У випадку зі звичайною версією, якщо Maven одного разу завантажив `1.0`, то він більше не буде намагатися завантажити нову версію `1.0` зі сховищ. Для того, щоб завантажити оновлений продукт версія має бути оновлена до `1.1`.

У випадку зі snapshot, Maven автоматично буде підтягувати крайній snapshot (наприклад, `1.0-SNAPSHOT`) кожен раз, коли буде виконуватися збірка проекту.

### Використання змінних у POM-файлі

У POM-файлі є можливість використовувати змінні, які необхідні для зручного групування налаштувань користувача. Наприклад, перевизначення змінної `<maven.compiler.target>` дозволяє задати версію java-компілятора для плагіна `compiler`, що є хорошою альтернативою задання налаштувань через сукупність тегів `<build>` > `<plugins>` > `<plugin>` > `<configuration>` > `<target>`.

Змінні задаються у тезі `<properties>` в якості вкладених тегів, наприклад:

```xml
<properties>
    <!-- Версія бібліотеки junit-->
    <junit.junit>4.12</junit.junit>
    <!-- Версія java-компілятора --->
    <maven.compiler.target>16</maven.compiler.target>
    <!-- Користувацька змінна custom_var-->
    <custom_var>25.04.1995</custom_var>
</properties>
```

Для того, щоб отримати значення змінної, використовують конструкцію вигляду `${var_name}`, наприклад:

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <!-- Підставляємо значення змінної <junit.junit> -->
    <version>${junit.junit}</version>
</dependency>

```

## Залежності Maven

### Області видимості залежностей

При детальному вивченні pom-файлу можна помітити, що для опису деяких залежностей (наприклад бібліотек тестів `Junit` та `TestNG`) використовується параметр `<scope>`, він же - область видимості. Даний параметр дозволяє вказати Maven'у коли і для чого нам потрібна дана залежність. Всього існує шість областей видимості:

|Значення|Короткий опис|
|:-:|---|
|`compile`|Залежність необхідна для збирання, тестування та виконання програми. Активна за замовчуванням.|
|`provided`|Аналог `compile`, але в результуючий пакет залежність добавлена не буде, оскільки вона вже є на сервері.|
|`runtime`|Протилежність `provided`, залежність необхідна для виконання та тестування, але не для збирання проекту.|
|`test`|Залежність необхідна під час тестування.|
|`system`|Аналог `compile`, але необхідно вказувати абсолютний шлях залежності.|
|`import`|Використовується для імпорту залежностей (типу pom) з інших артефактів, а також для управління залежностями у складних пакетах, які складаються з декількох артефактів.|

Дуже важливим є правильне задання області видимості для кожної конкретної залежності, оскільки, в разі помилки, у найкращому випадку розмір результуючого пакету буде необгрунтовано збільшено за рахунок непотрібних залежностей, а у найгіршому - програма не буде коректно працювати, оскільки не зможе знайти необхідні їй залежності.

### Ручне встановлення залежностей

Іноді трапляється таке, що необхідної бібліотеки немає у репозиторіях, але користувач має готовий скомпільований jar-архів. У такому випадку, для того щоб добавити його в проект у якості залежності використовується ручне встановлення. Для цього необхідно виконати наступну команду: 

```sh
mvn install:install-file -Dfile=... 
                         -DgroupId=... 
                         -DartifactId=...
                         -Dversion=...
                         -Dpackaging=... 
```

Усі параметри є обов'язковими для заповнення. Після успішного виконаня команди, jar-архів буде скопійовано у локальний maven-репозиторій (`.m2/repository`), для нього згенерується pom-файл із основними налаштуваннями і він стане доступний для використання у якості залежності.

