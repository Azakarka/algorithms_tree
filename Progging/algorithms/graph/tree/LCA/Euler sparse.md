Сделаем Эйлеров путь - проход по всем вершинам один раз и кидаем их в список
1 - 2
1 - 3
1 2 1 3 1

Найдем какой-то вход наших вершин и посмотрим на индексы между ними. Можно доказать, что не важно какие мы возьмем их lca будет лежать между ними, тогда сделаем спарсы на индекс с минимальной высотой, что и будет ответом. 

**0(nlog) + O(1)**
