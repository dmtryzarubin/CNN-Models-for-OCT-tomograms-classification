# Нейронные сетей для анализа томограмм сетчатки глаза
***
В данном репозитории представлены модели свёрточных нейронных сетей, созданные для классификации томограмм сетчатки глаз. Всего было создано 6 моделей CNN (Convolutional Neaural Networks). Одна из них - с Inception модулями.

Итак, этот репозиторий состоит из 6 файлов ".ipynb". Каждый из них содержит: обучение, оценку и визуализацию каждой модели: ее архитектуры, нейронов и тепловых карт.
Для разработки использовалась библиотека Tensorflow версии "2.0.0", а также фреймворк Keras.
Некоторые функции были взяты из книги "Глубокое обучение на Python" - Ф.Шолле.

## § Описание
На долю патологии сетчатки приходится около 1% всех заболеваний глаза, хотя эта проблема является одной из самых актуальных в офтальмологии. Среди болезней сетчатки выделяют: 
-	сосудистые заболевания (диабетическая ретинопатия, непроходимость вен сетчатки, окклюзия центральной артерии сетчатки, изменение сетчатки при ар-териальной гипертензии);
-	патологические изменения сетчатки (кровоизлияние, отек, макулярные разрывы сетчатки, новообразования сетчатки);
-	травмы сетчатки (проникающие рубцы, инородные тела);
-	расслоение сетчатки;
-	дистрофические заболевания (возрастная макулодистрофия (дистрофия) сетчатки, центральная и периферическая дистрофия сетчатки при близорукости, пигментная дегенерация сетчатки);
-	новообразования.
Наиболее распространенными патологиями (рисунок 1) сетчатки глаза по данным ВОЗ являются:
-	хориоидальная неоваскуляризация (CNV);
-	диабетический макулярный отек (DME);
-	возрастная макулярная дегенерация (AMD);
-	друзы (DRUSEN).

### Особенности томограмм для патологий (CNV, DME, Drusen) и здоровой сетчатки 
![alt text](https://i.imgur.com/fSTeZMd.png "Особенности томограмм для патологий (CNV, DME, Drusen) и здоровой сетчатки")

### Признаки патологий
+ общая деформация сетчатки:
..* вогнутая;
..* выгнутая; 
+ исчезновение центральной ямки; 
+ новообразования;
+ асимметрия контура центральной ямки; 
+ выраженное утолщение среза сетчатки;
+ и т.д.

### Проблема: 
Большое количество признаков значительно усложняет создание строго алгоритмизированного анализа

### Решение:
Решение: глубокие нейронные сети самостоятельно выделяют признаки в процессе обучения

### Выделение признаков нейронной сетью
![alt text](https://i.imgur.com/fSTeZMd.png "Особенности томограмм для патологий (CNV, DME, Drusen) и здоровой сетчатки")


## § Что было сделано
Стоит отметить, что для анализа медицинских изображений важно, чтобы при классификации, модель не отнесла пациента с патологией к группе здоровых.
Для оценки моделей описанных далее были использованы следующие метрики: 
- доля верных ответов;
- точность;
- полнота;
- F1-мера.

Для обучения и оценки использовался следующий набор данных – [«Labeled Optical Coherence Tomography (OCT) images for classification»](https://www.kaggle.com/paultimothymooney/kermany2018). Набор размеченных томограмм сетчатки содержит 83495 изображений для обучения и 968 изображений для оценки модели. Изображения разделены на 4 каталога: CNV, DME, DRUSEN и NORMAL. Данные представлены изображениями разных размеров (512×496, 512×768, 1536×496) в формате ".jpeg", которые затем были сжаты до размеров 227×227. Несмотря на сжатие почти в 2 раза, изображения обладают достаточной информативностью для классификации.
После сжатия набор был разделен в пропорции 80:20 на тренировочную и валидационную выборку, таким образом, получаем 66788 снимков для обучения и 16696 снимков для валидации. На данном наборе было обучено 7 моделей, описанных ниже, для всех моделей использовался оптимизатор “rmsprop”. Для оценки потерь – категориальная перекрестная энтропия. Для обучения моделей ис-пользовалась библиотека «Tensorflow» и фреймворк «Keras».

## § Результаты и их сравнение
Первоначально была разработана CNN с 3-мя сверточными слоями, так как такого количества слоев достаточно для выделения признаков, согласно [источнику](https://www.nature.com/articles/nature14539), кроме того изображения в наборе достаточно однородные, хотя и присутствуют выбросы в виде зашумленных изображений или изображений с белыми областями.

### Диаграмма 1
![alt text](https://github.com/dmtryzarubin/CNN-Models-for-OCT-tomograms-classification/blob/master/imgs/%D0%94%D0%BE%D0%BB%D1%8F%20%D0%B2%D0%B5%D1%80%D0%BD%D1%8B%D1%85%20%D0%BE%D1%82%D0%B2%D0%B5%D1%82%D0%BE%D0%B2.png "Диаграмма 2") 

Из диаграммы 1 видно, что относительно более глубоких моделей, линейная CNN с 3 сверточными слоями показывает на порядок более низкую точность.  Затем, последовала разработка более глубоких моделей, например, диаграмма 1 показывает, что еще один свёрточный слой и увеличение количества фильтров позволяет кардинально улучшить долю правильных ответов. Блок-схемы с количеством слоёв, размерами фильтров, и другими параметрами моделей находятся в «.ipynb» файлах , вместе с матрицами неточностей, визуализированными картами активации, и визуализированные тепловые карты.

В диаграмме 1 видно, что модель с 5 свёрточными слоями дает еще более точные прогнозы в отношении диагноза, однако увеличение количества слоев с 5 до 6 не дает увеличения доли правильных ответов, что может говорить о том, что такая архитектура является слишком сложной для данной задачи, также это может сигнализировать о недообучении. Далее, к 6 линейным свёрточным слоям были добавлены дополнительные слои свертки с фильтром 1×1, таким образом получив модель с 10 сверточными слоями. Это дополнение дало увеличение точности на 1 процент, также из матрицы неточностей, видно, что данная модель выполнила основную задачу лучше остальных, так как ни один больной пациент не был классифицирован как здоровый. Далее были предприняты попытки разработать модель с Inception-модулями, так как [Xception](https://arxiv.org/abs/1610.02357), является лидером соревнования «Imagenet», но использовать столь глубокую сеть для классификаций однородных томограмм было бы нецелесообразно с точки зрения затраченного времени на обучение. Следовательно, нужно было пойти на компромисс, либо уменьшить количество параметров – еще больше сжать изображения, либо разработать менее глубокую модель с такими модулями. Как можно видеть из диаграммы 1 данная модель показывает себя хуже, чем линейная модель с 11 сверточными слоями. В ipython ноутбуках есть тепловые карты активации для каждого класса изображений, на которых можно увидеть, какие области изображений повлияли на принятие сетью решения, то есть с их помощью можно понять какие признаки важны для того или иного класса. При помощи карт активаций, можно увидеть какие признаки выделялись в каждом слое. Для сравнения с более глубокими архитектурами была обучена модель [«MobileNetV2»](https://arxiv.org/abs/1801.04381), которая отличается сравнительно небольшим количеством параметров и высокой точностью. Более детально изучив тепловые карты, можно сделать вывод, что модель с 3 слоями при классификации изображения класса CNV была «не уверена» в предсказании, так как на изображении отсутствуют желтые или красные области. Касательно других моделей – можно сделать вывод, что модели выделяли правильные области для классификации, например для класса DRUSEN в основном выделялась область с волнообразной деформации линии пигментного эпителия, также, для классификации здоровых сетчаток, алгоритмы «смотрят» на деформацию макулы. 

При обучении использовались дополнительные функции “keras. callbacks” для предотвращения переобучения, поэтому количество эпох для обучения каждой модели разное. Все модели прекращали обучения если в течение трёх эпох обучения значение точности на валидации не изменялось более чем на ± 0,1, далее, если значение функции потерь не изменялось в течение двух эпох, скорость обучения уменьшалась в 10 раз. Также из-за различий в сложности модели требуют разной вычислительной мощности, следовательно, нужно было корректировать параметр “batch size”, который равен количеству изображений передаваемых в нейросеть за раз.

## § Итоги
Таким образом были спроектированы алгоритмы анализа томограмм, исполь-зующие нейронные сети различной сложности: 3-слойные, 4-слойные, 5-слойные, 6-слойные, 10-слойные, а также модель с Inception модулями;
Также было показано, что наиболее точной является 10-слойная свёрточная нейронная сеть. Точность составляет 98.3% что сопоставимо с точностью «MobileNetV2» 98,7%, но разработанная модель требует значительно меньшего времени, а следовательно и ресурсов, на обучение.

### Диаграмма 2
![alt text](https://github.com/dmtryzarubin/CNN-Models-for-OCT-tomograms-classification/blob/master/imgs/%D0%92%D1%80%D0%B5%D0%BC%D1%8F%20%D0%BE%D0%B1%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D1%8F.png "Диаграмма 2")
