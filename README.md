# Jet Container Security Framework (JCSF)

### Введение

Контейнеры давно и прочно вошли в жизнь IT - все больше приложений работают в них, все больше компаний переходят с классических монолитных приложений на микросервисные. Тема безопасности контейнеров, сред контейнеризации (например, Docker) или контейнерной оркестрации (например, k8s), образов контейнеров тоже далеко не нова: существует множество лучших практик, рекомендаций производителей и просто устоявшихся подходов. Нам в нашей работе не хватало чего-то одного, агрегирующего и систематизирующего все эти знания. Поэтому мы решили создать Jet Container Security Framework, являющийся квинтэссенцией наиболее популярных рекомендаций, практик и нашего опыта.

### Важный дисклеймер

В публичный доступ выложены не все наши наработки по JCSF. Мы считаем, что основная часть фреймворка JCSF должна быть публичным достоянием: общее описание абстракций, список контролей и их расположение на уровнях зрелости **навсегда останутся общедоступными.**

Однако, есть и “закрытая” часть, которую мы реализуем в наших проектах по аудиту с использованием JCSF. В ней есть 

- опросные листы для более удобного сбора информации от них “оффлайн”;
- супердетальные примеры того, “как проверить, что та или иная практика ДЕЙСТВИТЕЛЬНО выполняется”, а также примеры того, как необходимо реализовывать КАЖДУЮ практику;
- динамическая “иллюминация” ячеек с практиками, степенью их выполнения;
- расширенный и кастомизируемый отчет по аудиту на основе JCSF.

Если вас интересует аудит по нашему фреймворку JCSF - обращайтесь к нам (можно писать на dso@jet.su)

### R&D и дальнейшие планы:

- Разметить практики, применимые только к docker-swarm, docker compose, OpenShift.
- Описать примеры реализации каждой практики

### Описание JCSF

При обеспечении безопасности контейнеров на протяжении всего их жизненного цикла часто возникает вопрос **«А что действительно нужно делать вначале и что можно сделать чуть позже?».** Ведь если сделать “всю безопасность” сразу, то ничего работать не будет вообще :)

Поэтому мы предлагаем смотреть на безопасность контейнерной инфраструктуры и жизненный цикл контейнеров через 5 абстракций (ДОМЕНОВ):

- Безопасность узлов (nodes);
- Безопасность оркестратора (orchestrator);
- Безопасность образов (images);
- Безопасность манифестов (manifests);
- Безопасность контейнеров в рантайм (container).

![image.png](https://github.com/Jet-Security-Team/Jet-Container-Security-Framework/blob/main/images/5abstracts1.png)

В каждом домене нужно выполнять определенный набор действий (контролей). Некоторые из контролей (например, управление событиями безопасности или управление секретами) могут выполняться в нескольких доменах (но разными способами и\или инструментами). 

В свою очередь, каждый из контролей состоит из набора практик (задач), которые необходимо выполнять на том или ином этапе зрелости компании (и процессов защиты контейнерной инфраструктуры). 

![image.png](https://github.com/Jet-Security-Team/Jet-Container-Security-Framework/blob/main/images/controls1.png)

Именно такую структуру (домены - контроли - практики) с наложенными уровнями зрелости (Beginners - L1, Intermediate - L2, Advanced - L3, Experts - L4) на каждую практику и предлагает JCSF.

### Графические результаты

**Для визуализации получаемых в результате аудита (или самооценки компании) контейнерной инфраструктуры мы добавили лист “Результаты”, где на лепестковой диаграмме отображаются “успехи” по каждому домену.** 

На каждом листе, содержащем в названии “sec” сгруппированы практики по уровням зрелости (по аналогии с большинством общеизвестных фреймворков):

- **Уровень 0: Uninitiated**

> На этом уровне компания не имеет никаких формализованных процессов или инструментов контейнерной безопасности. Практики могут использоваться случайным образом по инициативе отдельных работников.
> 
- **Уровень 1: Beginners**

> На этом уровне зрелости практик в компании начинают появляются инструменты, используемые при защите контейнеров с минимальной зоной покрытия и без автоматизации. Появляются базовые процессы.
> 
- **Уровень 2: Intermediate**

> Процессы на этом уровне становятся повторяемыми и управляемыми, происходит расширение зоны покрытия используемых инструментов, внедрение автоматизации. Компания начинает применять методики для планирования, выполнения и отслеживания активностей. Однако эти методики могут быть не всегда последовательными или полностью документированными. Инструменты по прежнему не покрывают весь процесс защиты контейнеров.
> 
- **Уровень 3: Advanced**

> На данном уровне фактически инструменты контейнерной безопасности обеспечивают максимальное покрытие и автоматизацию. Все процессы являются последовательными и полностью документированными
> 
- **Уровень 4: Experts**

> Когда все процессы и инструменты развиты максимально, нет предела совершенству. Можно по прежнему что-то улучшить и повысить зрелость.
> 

## Материалы, используемые при создании

Для создания фреймворка были проанализированы и использованы следующие материалы:

- Международные лучшие практики:
- Практики от Center for Internet Security (CIS):
    - [CIS Software Supply Chain Security Guide](https://www.cisecurity.org/insights/white-papers/cis-software-supply-chain-security-guide);
    - [CIS GitHub Benchmark](https://www.cisecurity.org/insights/blog/cis-benchmarks-february-2023-update);
    - [NIST Application Container Security Guide](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-190.pdf);
    - [ФСТЭК России №118](https://fstec.ru/dokumenty/vse-dokumenty/prikazy/prikaz-fstek-rossii-ot-16-iyunya-2023-g-n-118);
    - [NSA Kubernetes hardening guideline](https://www.nsa.gov/Press-Room/News-Highlights/Article/Article/2716980/nsa-cisa-release-kubernetes-hardening-guidance/).
- Best practices:
    - Aqua Cloud Native Security Maturity Model
- Наш опыт, а также опыт наших заказчиков.
![image](https://github.com/Jet-Security-Team/Jet-Container-Security-Framework/blob/main/images/398951626-fd47b785-1144-469d-b2b7-6d1aae0290b4.png)
