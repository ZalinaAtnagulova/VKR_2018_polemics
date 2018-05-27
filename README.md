# ВКР Методы автоматической и полуавтоматической оценки полемичности обсуждений на русскоязычных интернет-ресурсах

Выделение наиболее полемичных тем на материале заголовков с сайта новостного агентства РИА Новости

## Логика программы
Программа состоит из 5 логических блоков:
1) *скачивание заголовков с сайта РИА Новости* 
2) *обучение модели Doc2vec на тренировочном корпусе заголовков*
3) *кластеризация заголовков для выделения полемичных тем*
4) *скачивание комметариев к статьям в кластерах на полемичные темы*
5) *кластеризация комметариев для выделения мнений, которых придерживаются люди, обсуждающие тему*

## 1. Скачивание заголовков с сайта РИА Новости
За этот блок отвечают файлы: *Ria.ipynb* и *R-sport.ipynb*. Они скачивают новостные заголовки из разделов сайта «политика», «общество», «экономика», «происшествия», «в мире», а также по рубрике «Пхенчхан-2018» раздела «спорт» за период 1 января 2017 года – 10 февраля 2018 года, из раздела Пхенчхан-2018 были взяты статьи за период 12 июля 2017 года – 28 февраля 2018 года в отдельный файл *all_heads_links_nodups.txt* для каждого раздела в формате: заголовок статьи, ссылка на статью, разделитель '###'.
Код в файле *Remove repeating headlines.ipynb* удалит дубликаты из корпуса и разобьет его на тренировочный, тестовый и короткий тестовый файлы. Короткиц тестовый файл использовался, что сократить время кластеризации в п. 2

## 2. Обучение модели Doc2vec на тренировочном корпусе заголовков
За этот блок отвечает файл *doc2vec beautiful no stop lemma.ipynb*. Модель обучается на тренировочном корпусе, файл *train_headlines.txt*, содержащем 160 000 заголовков из всех разделов, который она получает на вход вместе со списком стоп-слов из файла *stopwords.txt*. Обученная модель сохраняется в файл *noStopLemma_PV-DBOW_wrd-vec_1it_2win_6mincount_alpha25-25_sz80.model*

## 3. Кластеризация заголовков для выделения полемичных тем
За этот блок отвечает файл *KMeansClusterer.ipynb*. Программа загружает модель из предыдущего этапа *noStopLemma_PV-DBOW_wrd-vec_1it_2win_6mincount_alpha25-25_sz80.model*, а также получает на вход файл *test_headlines_short.txt* с тестовыми заголовками, среди которых она будет выделять кластеры и файл со стоп-словами *stopwords.txt*. Всего она выделяет 35 кластеров, дла каждого из готорого выведет 30% всех заголовков в кластере, которе ближе всего центру кластера. Из них нужно будет отобрать те, что действительно могут быть о полемичной теме, и сохранить в файл *polemic_headlines_by_themes.txt*, в формате: id_кластера \r\n все ближашие к центру кластера заголовки по одному на отроку \r\n\r\n

## 4. Скачивание комметариев к статьям в кластерах на полемичные темы
За этот блок отвечает файл *Get comments.ipynb*. Код получает на вход файл *polemic_headlines_by_themes.txt* и файл *all_heads_links_nodups.txt* в котором он по заголовкам находит ссылку на статью и скачивает все комментарии к ней и необходимую метаинформацию: id статьи, дату последнего комментария на странице, дату и id комментария, к которому есть ответы. Результат скчивания комментариев сохраняется в список с id кластеров полемичных тем, выделенных в п.3, и для каждой выделенной темы список с заголовком статьи, ссылкой на нее, датой первого комментария, всеми комментариями и ответами на них и списком всех пользователей, участвовавших в обсуждениях по теме статьи. Итоговый список с комментариями к статьям в каждом из выделенны в п.3 кластеров сохраняется в файл *polemic_comments.pkl*

## 5. Кластеризация комметариев для выделения мнений, которых придерживаются люди, обсуждающие тему
За этот блок отвечает файл *sklearn K-means.ipynb*. Программа получает на вход сохраненный в файл список *polemic_comments.pkl*, подгружает модель Doc2vec из п.2 и файл со стоп-словами *stopwords.txt*, и кластеризует комментарии к каждой из выделенных в п.3 полемичных тем. Рекомендуется передавать для кластеризации только те темы, в которыз больше 100 заголовков и на каждый из нихв среднем приводится не меньше 10 комментариев, сводные данные программа выводит.

### Контакты
По всем вопросам относительно кода писать [мне](zalina2804@mail.ru).
Буду рада любому фидбэку.
