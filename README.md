# deposit-calculator

Калькулятор банковского вклада (простые проценты и капитализация).
Лабораторная работа № 2 «Инструменты и средства разработки сценариев в системах непрерывной интеграции».

Один и тот же исходный код собирается тремя инструментами: Apache Ant, Apache Maven и Gradle.

## Требования

- JDK 17 или новее (код компилируется с `release 17`)
- Apache Ant 1.10.x (`ant-junitlauncher.jar` входит в дистрибутив)
- Apache Maven 3.9.x
- Для Gradle: JDK 17–24. Gradle 8.14 не запускается на JDK 25, поэтому в работе
  использован JDK 21 (входит в поставку IntelliJ IDEA)

## Ant

JAR-файлы JUnit 5 для Ant лежат в `lib/`.

```
ant -p          # список targets
ant clean       # удалить build/
ant compile     # build/classes
ant test        # запуск JUnit 5 через junitlauncher
ant report      # XML + HTML-отчёт: build/reports/html/index.html
ant jar         # build/jar/DepositCalculator.jar
ant run         # запуск приложения
ant clean all   # чистая полная сборка
```

## Maven

```
mvn clean verify               # полный цикл: compile → test → package → verify
mvn dependency:tree            # дерево зависимостей
mvn surefire-report:report     # HTML-отчёт: target/reports/surefire.html
```

Результаты тестов в формате XML/TXT: `target/surefire-reports/`.

## Gradle (расширение)

В проект добавлен Gradle Wrapper 8.14: `gradlew`, `gradlew.bat` и каталог
`gradle/wrapper/`. Глобальный Gradle не нужен — Wrapper сам использует
зафиксированную версию из `gradle/wrapper/gradle-wrapper.properties`.

```
gradlew.bat test    # тесты, HTML-отчёт: build/reports/tests/test/index.html
gradlew.bat build   # компиляция, тесты, JAR: build/libs/deposit-calculator-1.0.0-SNAPSHOT.jar
gradlew.bat run     # запуск приложения
```

Если Gradle не находит JDK 17, укажите `JAVA_HOME` на JDK 17–24. Компиляция
выполняется с `options.release = 17`, поэтому байткод совпадает с Ant- и
Maven-сборкой.

## Checkstyle (дополнительно)

В POM добавлен профиль `quality` со статическим анализом Checkstyle
(конфигурация — `config/checkstyle/checkstyle.xml`). На основной lifecycle
профиль не влияет, запуск — отдельной командой:

```
mvn -Pquality checkstyle:check       # 0 нарушений → BUILD SUCCESS
mvn -Pquality checkstyle:checkstyle  # HTML-отчёт: target/reports/checkstyle.html
```

## CI

Workflow GitHub Actions: `.github/workflows/maven.yml` — запускает `mvn -B clean verify`
и сохраняет `target/surefire-reports/` как артефакт.
