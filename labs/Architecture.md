# Архитектура
1. [Проектирование архитектуры](#type) <br>
  1.1 [Тип приложения](#type) <br>
  1.2 [Стратегия развертывания](#strategy) <br>
  1.3 [Используемые технологии](#technology) <br>
  1.4 [Реализация сквозной функциональности](#throught) <br>
  1.5 [Показатели качества](#quality) <br>
  1.6 [Диаграмма ToBe](#diagram) <br>
2. [Анализ архитектуры](#arc_analysis) <br>
  2.1 [Анализ](#analysis) <br>
  2.2 [Диаграмма AsIs](#asIS) <br>
  2.3 [Диаграмма классов](#class) <br>
3. [Сравнение As Is и To Be, пути улучшения архитектуры](#compare) <br>  

<a name='type'></a>
## 1. Проектирование архитектуры 
### 1.1 Тип приложения
  Данное приложения будет создано на основе микросервисной архитектуры. Оно будет состоять из четырех модулей: <br>
- Shop - отвечает за операции над данными пользователей (логины, пароли и т.д.), а также за операции над заказами. 
- Сatalog - операции над предложениями.
- Vseti-UI - микросервис, отвечающий за корректное отображение данных и взаимодействие с пользователем приложения.
- Processor - модуль, взаимодействующий с пользователем и связывающий между собой три предыдущих.

Согласно микросервисной архитектуре для shop и catalog созданы отдельные базы данных.
Особенностью данной архитектуры является ее гибкость к изменениям, что позволяет, например, вносить какие-либо изменения в модуль, отвечающий за пользовательскую информацию, но все другие модули будут работать как и прежде. Минусом данной архитектуры является ее сложность. В процессе написания программы возникла трудность для связывания сущностей в таблицах, которые находятся в разных базах данных.

<a name='strategy'></a>
### 1.2 Стратегия развертывания
  Для развертывания данного приложения будет применяться набор облачных сервисов, предоставляемых Amazon AWS. Для автоматизации развертывания будет применяться Docker. Каждый модуль будет помещен в отдельный снимок - исполняемый пакет, который будет содержать код, все необходимые зависимости и системные переменные. После того, как снимок будет запущен, то он становится отдельной средой выполнения - контейнером. Преимущество данного подхода заключается в том, что приложение выполняется не на отдельной виртуальной машине, а в контейнере, который менее ресурсозатратный и запускается на любой ОС.
  
<a name='technology'></a>
### 1.3 Используемые технологии
  Для написания серверной части выбор пал на Spring Framework, поскольку он наиболее распространен и предоставляет очень широкий и удобный инструментарий, позволяющий в несколько строк кода и пару аннотаций создать с нуля полноценный REST веб-сервис. Клиентская часть будет написана при помощи javascript фреймворка Angular.
  
<a name="throught"></a>
### 1.4 Реализация сквозной функциональности
  - Сетевое взаимодействие: использовать асинхронные взаимодействия; использовать HTTP протокол.
  - Управление конфигурацией: продумать, какие параметры должны быть конфигурируемыми извне.
  - Управление исключениями: обеспечить стабильность состояния приложения после сбоя.
  
<a name="quality"></a>
### 1.5 Показатели качества
  - Концептуальная целостность.
  - Удобство и простота обслуживания.
  - Возможность повторного использования.
  - Тестируемость.
  - Взаимодействие с пользователем.
  
<a name="diagram"></a>
### 1.6 Диаграмма ToBe
С диаграммой ToBe Architecture можно ознакомиться по следующей [ссылке](https://github.com/AndrewNaumenko/vseti/tree/master/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D1%8B/DeploymentDiagramToBeArchitecture.jpg)

<a name="arc_analysis"></a>
## 2. Анализ архитектуры

<a name='analysis'></a>
### 2.1 Анализ

  Архитектура каждого back-end модуля состоит из одинакового набора пакетов:
  - controller - Содержит внутри себя контроллеры, отвечающие на запросы как со стороны пользователя, так и со стороны других приложений. Сюда так же входят error контроллеры, задача которых заключается в обработке ошибок, пробрасываемых классами пакета service.
  - service - Внутри классов данного пакета содержится вся бизнес-логика приложения. CRUD операции над предложениями, заказами и пользовательскими аккаунтами находятся именно здесь.
  - repository - В данном пакете находятся репозитории - интерфейсы, взаимодействующие непосредственно с базой данных.
  - entity - Содержит классы, отображающие сущности и отношения между ними, с которыми работает данное приложение (пользователь, заказ, предложение).
  - dto - Содержат классы, которые представляют сущности в удобном для клиентской части приложения формате.
  - configuration - Классы, выполняющие настройку фреймворка.
  - mapper - Классы, занимающиеся пребразованием сущностей в dto.
  - exception - Наши собственные исключения.
  
  Не очень сложные приложения, построенные на базе Spring Framework, как правило, состоят из данных пакетов. Также для них характерна иерархическая структура: ControllerClass содержит как минимум один ServiceClass и MapperClass. ServiceClass как правило один или несколько RepositoryClass и MapperClass. 

<a name='asIs'></a>  
### 2.2 Диаграмма AsIs

С диаграммой AsIs Architecture можно ознакомиться по следующей [ссылке](https://github.com/AndrewNaumenko/vseti/tree/master/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D1%8B/DeploymentDiagramAsIsArchitecture.jpg)

<a name='class'></a>
### 2.3 Диаграмма классов

С диаграммой классов можно ознакомиться по следующей [ссылке](https://github.com/AndrewNaumenko/vseti/tree/master/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D1%8B/Catalog_class_diagramm.png)

<a name="compare"></a>
### 3. Сравнение As Is и To Be, пути улучшения архитектуры
  В архитектуре As Is, в отличие от архитектуры To Be, отсутствует микросервис Processor, т.к. на данном этапе он еще не был реализован. Также отсутствует пакет exception со своими исключениями. 
  Пути улучшения архитектуры:
- реализация микросервиса Processor в соответствии с архитектурой ToBe.
- реализация обработки исключений.
