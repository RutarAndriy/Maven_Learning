# Maven Learning
Apache Maven — система збирання та управління java-проектами.

## Зміст  

+ [Інсталяція Apache Maven](#інсталяція-apache-maven)
+ [Створення нового Maven-проекту](#створення-нового-maven-проекту)
+ [Структура Maven-проекту](#структура-maven-проекту)
+ [Maven POM](#maven-pom)
  + [Основні теги POM-файлу](#основні-теги-pom-файлу)
  + [Використання змінних у POM-файлі](#використання-змінних-у-pom-файлі)
+ [Залежності Maven](#залежності-maven)
  + [Транзитивні залежності](#транзитивні-залежності)
  + [Виключення залежностей](#виключення-залежностей)
  + [Області видимості залежностей](#області-видимості-залежностей)
  + [Ручне встановлення залежностей](#ручне-встановлення-залежностей)
+ [Репозиторії Maven](#репозиторії-maven)
  + [Локальні репозиторії](#локальні-репозиторії)
  + [Центральні репозиторії](#центральні-репозиторії)
  + [Віддалені репозиторії](#віддалені-репозиторії)
  + [Порядок пошуку залежностей Maven](#порядок-пошуку-залежностей-maven)
+ [Профілі збирання Maven](#профілі-збирання-maven)
+ [Плагіни Maven](#плагіни-maven)
+ [Етапи збирання Maven-проекту](#етапи-збирання-maven-проекту)
+ [Основні команди Maven](#основні-команди-maven)

## Інсталяція Apache Maven

Для скачування актуальної версії Apache Maven заходимо на [офіційний сайт](https://maven.apache.org/download.cgi "https://maven.apache.org/download.cgi"). У розділі "Files" вибираємо "Binary zip archive" — скачуємо та розпаковуємо архів у довільний каталог. Після цього необхідно додати шлях до директорії Maven у системні змінні "M2_HOME" та "PATH".

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
|'artifactId'|Це ідентифікатор самого проекту. Найчастіше в якості ідентифікатора виступає ім'я проекту. Наприклад, `maven-assembly-plugin`. Він також допомагає знайти проект в репозиторії.|
|'version'|Версія проекту. Визначає конкретну версію продукту.|
|'package'|Ім'я стандартного пакету програми, яке зазвичай являє собою обернене доменне ім'я її сайту. Наприклад, `org.apache.maven`.|
|Підтвердження введених параметрів|Введіть `Y` або натисніть `Enter` для підтвердження введених параметрів та побудови проекту.|

## Структура Maven-проекту

Maven дотримується суворої структури проекту — вихідний код програми, конфігураційні файли, графічні та інші ресурси повинні знаходитися у строго визначених директоріях. Це є вимогою POM-моделі (Project Object Model), яка дозволяє стандартизувати проекти. Основні директорії Maven-проекту описані у таблиці нижче:
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

    <!-- Версія pom-моделі -->
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

Усі POM-файли є спадкоємцями батьківського POM. Цей POM-файл називається SuperPOM — саме у ньому містяться значення, успадковані за замовчуванням.

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
|`...` > `<packaging>`|Тип артефакту (jar, pom, maven-plugin і т.д.)|
|`...` > `<dependencies>`|Кореневий тег, містить список залежностей|
|`...` > `...` > `<dependency>`|Конкретно визначена залежність проекту|
|`...` > `<build>`|Кореневий тег, містить параметри збирання проекту|
|`...` > `...` > `<finalName>`|Кінцеве ім'я зібраного артефакту|
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

`SNAPSHOT` — це спеціальна версія, яка показує поточну робочу копію. При кожній збірці Maven перевіряє наявність нової snapshot-версії на віддаленому репозиторії.

У випадку зі звичайною версією, якщо Maven одного разу завантажив `1.0`, то він більше не буде намагатися завантажити нову версію `1.0` зі сховищ. Для того, щоб завантажити оновлений продукт версія має бути оновлена до `1.1`.

У випадку зі `snapshot`, Maven автоматично буде підтягувати крайній snapshot (наприклад, `1.0-SNAPSHOT`) кожен раз, коли буде виконуватися збірка проекту.

### Використання змінних у POM-файлі

У POM-файлі є можливість використовувати змінні, які необхідні для зручного групування налаштувань користувача. Наприклад, перевизначення змінної `<maven.compiler.target>` дозволяє задати версію java-компілятора для плагіна `compiler`, що є хорошою альтернативою задання налаштувань через сукупність тегів `<build>` > `<plugins>` > `<plugin>` > `<configuration>` > `<target>`.

Змінні задаються у тезі `<properties>` в якості вкладених тегів, наприклад:

```xml
<properties>

    <!-- Версія бібліотеки junit -->
    <junit.junit>4.12</junit.junit>

    <!-- Версія java-компілятора -->
    <maven.compiler.target>16</maven.compiler.target>

    <!-- Користувацька змінна custom_var -->
    <custom_var>25.04.1995</custom_var>

</properties>
```

Для того, щоб отримати значення змінної, використовують конструкцію вигляду `${var_name}`, наприклад:

```xml
<dependency>

    <!-- Підключаємо бібліотеку junit -->
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>

    <!-- Підставляємо значення змінної <junit.junit> -->
    <version>${junit.junit}</version>

</dependency>

```

## Залежності Maven

Рідко коли який-небудь проект обходиться без додаткових бібліотек, і як правило, такі бібліотеки необхідно включити в збірку, якщо це не проект [OSGi](https://en.wikipedia.org/wiki/OSGi) або [WEB](https://en.wikipedia.org/wiki/Web_application). Для вирішення даного завдання в Maven-проекті необхідно використовувати залежності (dependencies), що прописуються у pom-файлі. Для кожного використовуваного в проекті артефакту необхідно задати такі параметри:

+ параметри GAV (`groupId`, `artifactId`, `version`) і, в окремих випадках, `classifier`
+ області видимості залежностей: `compile`, `provided`, `runtime`, `test`, `system` або `import`
+ місцерозташування залежності (для `system` області видимості залежності)

Оголошення залежностей відбувається в середині тегу `<dependencies>`. Кількість залежностей не обмежена. Приклад підключення залежності подано нижче:

```xml
<dependencies>
    <dependency>

        <!-- GAV координати залежності -->
        <groupId>${package.name}</groupId>
        <artifactId>${jar.name}</artifactId>
        <version>${jar.version}</version>

        <!-- Область видимості залежності -->
        <scope>system</scope>

        <!-- Шлях до залежноті (тільки для system scope) -->
        <systemPath>${jar.path}</systemPath>

    </dependency>
<dependencies>
```

Інколи виникає необхідність визначати такий параметр як класифікатор — `classifier`, оскільки в іншому випадку бібліотека не буде знайдена в центральному репозиторії:

```xml
<dependencies>
    <dependency>

        <!-- Підключння бібліотеки json-lib -->
        <groupId>net.sf.json-lib</groupId>
        <artifactId>json-lib</artifactId>
        <version>2.4</version>
        <classifier>jdk15</classifier>

    </dependency>
<dependencies>
```

Класифікатор `classifier` використовується в тих випадках, коли розподіл артефакту за версіями є недостатнім. Наприклад, певна бібліотека (артефакт) може бути використана тільки з певною JDK (VM), або розроблена під windows або linux. Визначати цим бібліотекам різні версії — ідеологічно не правильно. Але використовуючи різні класифікатори можна вирішити дану проблему. Значення `classifier` додається в кінець назви артефакту, після його версії, перед розширенням. Наприклад: `json-lib-2.4-jdk15.jar`

### Транзитивні залежності

Підключені залежності в Maven можуть мати власні залежності — вони називаються транзитивними. Транзитивні залежності прописані у pom-файлі підключених залежностей і завантажуються автоматично разом із ними. Внаслідок цього можуть виникнути конфлікти, оскільки різні залежності можуть використовувати спільні транзитивні залежності різних версій. Розглянемо наступний випадок:

```xml
<dependencies>

    <!-- Бібліотека junit -->
    <!-- Використовує транзитивну залежність hamcrest-core версії 1.3 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>

    <!-- Бібліотека hamcrest-core, версія 2.2 -->
    <dependency>
        <groupId>org.hamcrest</groupId>
        <artifactId>hamcrest-core</artifactId>
        <version>2.2</version>
    </dependency>

</dependencies>
```

У даному проекті використовується дві залежності: `junit` та `hamcrest-core`. Вони є залежностями першого порядку. Бібліотека `junit` має свою транзитивну залежність `hamcrest-core` версії `1.3`. Вона є залежністю другого порядку. Схематично це можна зобразити так:

+ `junit-4.12` > `hamcrest-core-1.3`
+ `hamcrest-core-2.2`

Як бачимо, виник конфлікт залежностей, оскільки у проекті використовується залежність образу двох версій: `1.3` та `2.2`. У цьому випадку Maven залишить у проекті ту залежність, яка має вищий порядок, тобто `hamcrest-core-2.2`. Інша версія бібліотеки буде виключена із дерева залежностей.

### Виключення залежностей

Бувають випадки, коли стандартного методу регулювання конфліктних залежностей недостатньо, або ж він спричиняє проблеми. Зазвичай так трапляється тоді, коли артефакту потрібна конкретна версія транзитивної залежності, без якої він не може працювати і яка була виключена стандартним методом. В такому випадку може допомогти ручне виключення залежностей. Розглянемо наступний праклад:

```xml
<dependencies>

    <!-- Бібліотека lib_A -->
    <!-- Використовує транзитивні залежності lib-C-1.0 та lib-Z-2.0 -->
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>lib-A</artifactId>
        <version>1.0</version>
    </dependency>

    <!-- Бібліотека lib-B -->
    <!-- Використовує транзитивну залежність lib-Z-1.8 -->
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>lib-B</artifactId>
        <version>1.0</version>
    </dependency>

</dependencies>
```

Є дві залежності `lib-A` та `lib-B`, кожна з яких має свої транзитивні залежності. Схематично це можна зобразити так:

+ `lib-A-1.0` > `lib-C-1.0` > `lib-Z-2.0`
+ `lib-B-1.0` > `lib-Z-1.8`

Для роботи артефакту `lib-C` необхідна транзитивна залежність `lib-Z` версії `2.0`, але вона буде виключена з дерева залежностей, оскільки версія `1.8` має вищий порядок. Для вирішення цієї проблеми, необхідно вручну виключити транзитивну залежність версії `1.8`. Для цього необхідно змінити pom-файл, додавши список виключених бібліотек у тег `<exclusions>` наступним чином:

```xml
<dependencies>

    <!-- Бібліотека lib_A -->
    <!-- Використовує транзитивні залежності lib-C-1.0 та lib-Z-2.0 -->
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>lib-A</artifactId>
        <version>1.0</version>
    </dependency>

    <!-- Бібліотека lib-B -->
    <!-- Використовує транзитивну залежність lib-Z-1.8 -->
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>lib-B</artifactId>
        <version>1.0</version>
        <exclusions>

            <!-- Виключаємо транзитивну залежність lib-Z-1.8 -->
            <exclusion>
                <groupId>com.example</groupId>
                <artifactId>lib-Z</artifactId>
            </exclusion>

        </exclusions>
    </dependency>
</dependencies>
```

Після вищеописаних маніпуляцій із pom-файлом, залежність `lib-Z-1.8` буде виключена із дерева залежностей, тому залежність `lib-Z-2.0` буде використовуватися у проекті в якості основної.

### Області видимості залежностей

При детальному вивченні pom-файлу можна помітити, що для опису деяких залежностей (наприклад бібліотек тестів `Junit` та `TestNG`) використовується параметр `<scope>`, він же — область видимості. Даний параметр дозволяє вказати Maven'у коли і для чого нам потрібна дана залежність. Всього існує шість областей видимості:

|Значення|Короткий опис|
|:-:|---|
|`compile`|Залежність необхідна для збирання, тестування та виконання програми. Активна за замовчуванням.|
|`provided`|Аналог `compile`, але в результуючий пакет залежність добавлена не буде, оскільки вона вже є на сервері.|
|`runtime`|Протилежність `provided`, залежність необхідна для виконання та тестування, але не для збирання проекту.|
|`test`|Залежність необхідна під час тестування.|
|`system`|Аналог `compile`, але необхідно вказувати абсолютний шлях залежності.|
|`import`|Використовується для імпорту залежностей (типу pom) з інших артефактів, а також для управління залежностями у складних пакетах, які складаються з декількох артефактів.|

Дуже важливим є правильне задання області видимості для кожної конкретної залежності, оскільки, в разі помилки, у найкращому випадку розмір результуючого пакету буде необгрунтовано збільшено за рахунок непотрібних залежностей, а у найгіршому — програма не буде коректно працювати, оскільки не зможе знайти необхідні їй залежності.

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

Також допускається об'явлення локальних залежностей у pom-файлі, з використанням області видимості `system`. У цьому випадку в тезі `<systemPath>` необхідно вказати шлях до використовуваної бібліотеки, абсолютний або відносний. Розглянемо наступний приклад:

```xml
<properties>
  
    <!-- Абсолютний шлях до бібліотеки -->
    <absolute.jar.path>/home/user/lib/CustomLib.jar</absolute.jar.path>
  
    <!-- Відносний шлях до бібліотеки -->
    <relative.jar.path>${basedir}/lib/CustomLib.jar</relative.jar.path>

</properties>

<dependency>
  
    <!-- Задаємо довільні параметри артефакту -->
    <groupId>com.mylib</groupId>
    <artifactId>CustomLib</artifactId>
    <version>1.0</version>
  
    <!-- Вибираємо область видимості system -->
    <scope>system</scope>
  
    <!-- Вказуємо абсолютний або відносний шлях до бібліотеки -->
    <systemPath>${absolute.jar.path} ... ${relative.jar.path}</systemPath>

</dependency>
```

Хоча такий варіант об'явлення локальних залежностей є допустимим, на практиці, зазвичай, надають перевагу першому варіанту, оскільки він дає можливість використовувати локальні залежності усім проектам, а не лише конкретному.

## Репозиторії Maven

Репозиторій — спеціальний сервер, на якому зберігається і з якого можна завантажити програмне забезпечення. Якщо говорити про нього у контексті Maven, то репозиторій — це сервер (локальний або віддалений), на якому зберігаються Maven-артефакти: бібліотеки, плагіни та інші залежності.

Існує три основні типи репозиторіїв Maven:

|Тип|Короткий опис|
|:-:|---|
|`local`|Локальні репозиторії — зберігаються на комп'ютері розробника|
|`central`|Центральні репозиторії — зберігаються в інтернеті, підтримуються спільнотою Maven|
|`remote`|Віддалені репозиторії — зберігаються в інтернеті, підтримуються власниками репозиторіїв|

### Локальні репозиторії

Локальний репозиторій — це директорія, яка зберігається на нашому комп'ютері. Вона створюється в момент першого виконання будь-якої команди Maven.

Локальний репозиторій Maven зберігає всі залежності проекту (бібліотеки, плагіни і т.д.). Коли ми виконуємо збірку проекту за допомогою Maven, всі залежності (тобто їхні JAR-файли) автоматично завантажуються в локальний репозиторій. Це допомагає нам уникнути повторного скачування залежностей при кожній збірці проекту.

### Центральні репозиторії

Центральний репозиторій Maven — це віддалене сховище, яке забезпечується та підтримується співтовариством Maven. Воно містить величезну кількість часто використовуваних бібліотек.

Якщо Maven не може знайти залежності в локальному репозиторії, то автоматично починається пошук необхідних файлів в центральному репозиторії за цією адресою: 
https://repo1.maven.org/maven2/

### Віддалені репозиторії

Іноді, Maven не може знайти необхідні залежності в центральному репозиторії. В цьому випадку, процес збірки переривається і в консоль виводиться повідомлення про помилку.

Для того, щоб запобігти подібній ситуації, в Maven передбачений механізм віддалених сховищ, які представляють собою репозиторії, що визначені самими розробниками. В них можуть зберігатися всі необхідні залежності.

Для того, щоб налаштувати віддалений репозиторій, необхідно прописати його атрибути у POM-файлі. Для цього використовується теги `<repositories>` та `<repository>`:

```xml
<repositories>
  
    <!-- Підключаємо сторонній репозиторій proselyte.lib1 -->
    <repository>
       <id>proselyte.lib1</id>
       <url>http://download.proselyte.net/maven2/lib1</url>
    </repository>
  
    <!-- Підключаємо власний репозиторій -->
    <repository>
       <id>custom-repository</id>
       <url>http://com.user.net/repository/my_libs</url>
    </repository>

</repositories>
```

### Порядок пошуку залежностей Maven

Коли ми виконуємо збірку проекту в Maven, автоматично починається пошук необхідних залежностей в наступному порядку:

1. Пошук залежностей в локальному репозиторії. Якщо залежності не виявлено, відбувається перехід до кроку 2.
2. Пошук залежностей в центральному репозиторії. Якщо вони не виявлені, але визначені віддалені репозиторії, то відбувається перехід до кроку 4.
3. Якщо віддалені репозиторії не визначені, то процес збірки припиняється і виводиться повідомлення про помилку.
4. Пошук залежностей у віддалених репозиторіх, якщо вони знайдені, то відбувається їх завантаження в локальний репозиторій, якщо ні — виводиться повідомлення про помилку.

## Профілі збирання Maven

## Плагіни Maven

## Етапи збирання Maven-проекту

## Основні команди Maven


