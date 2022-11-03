# Классификация позитивных и негативных отзывов на товары
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/valentinatihova/DS_projects/blob/main/ml_for_text_classification/ml_for_text_classification.ipynb)
## Данные
На вход поданы комментарии клиентов на товары интернет-магазина «Викишоп». Дан набор данных с разметкой о токсичности правок. Столбец *text* в нём содержит текст комментария, а *toxic* — целевой признак.

## Задача
Необходимо обучить модель классифицировать комментарии на позитивные и негативные, то есть получить инструмент, который будет искать токсичные комментарии и отправлять их на модерацию.

## Используемые библиотеки
nltk, stopwords, corpus, TfidfVectorizer, word2vec, Embeddings, BERT \ RuBERT, PyTorch, tqdm
