Есть много разных способов lca, я скажу которые знаю и эффективны в принципе.


1) [[Bin up]]. Самый стандартный, работает за O(nlog) и O(log).


2) [[Euler sparse]]. Спарсы на дереве лол. O(nlog) и O(1).


3) [[Tarjan]] сложный алгоритм Тарьяна на оффлайн запросы. Общая сложность O(n *alpha*), где *alpha* - обратная Аккермана. 

4) [[Euler SegTree]] O(n) и O(log)




# Задачи 


## Генеалогия
Запрос состоит из множества точек, нужно посчитать кол-во вершин, у которых в поддереве имеется одна из вершин.

Можно свести задачу к отрезкам и точкам, где отрезки это tin и tout вершины, а точками будут tin вершин в запросе.


## максимальное ребро на пути
Будем кроме массива up хранить еще и массив maxx, который будет хранить вес макс ребра между вершиной v и up[i][v].
Заполняется все так:
``` 

for(int i = 0; i < logs; i++){
	for(int j= 0; j < n; j++){
		up[i][j] = up[i - 1][up[i - 1][j]];
		maxx[i][j] = max(maxx[i - 1][j], maxx[i - 1][up[i - 1][j]]);
	}
}
```