# Sparse Table

## Предисловие
Разряженные таблицы. Прекрасная структура данных, всего за nlog препроцесснга умеет за O(1) отвечать на запросы. Правда есть один минус: мы не можем сделать спарсы для операций, ответы которых меняются при добавлении уже имеющихся элементов.

К примеру, мы можем сделать операции min, max, gcd, ИЛИ, И и тд, так как при добавлении в это множество уже имеющееся число значение не поменяется. 

А вот, к примеру, сумма, умножение, XOR и тд, уже не работают, так как при добавлении в множество уже имеющегося числа, ответ меняется.

Вообще сама струтура данных много где используется, особенно там где массив статичный. К примеру, задача об lca в дереве решается с помощью спарсов за O(nlog) и O(1) на запрос (можно сделать за O(n), O(1) с помощью Фарака-Колтона-Бендера, там тоже юзаются спарсы).

А обрабатывать операции вида суммы XOR и тд, то есть, которое не может обрабатывать обычный sparse table, умеет его брат - [[DisjointST]] (Disjoint Sparse Table), примечательно, что асимптотики остаются все такие же, советую глянуть, но после обычных спарсов.

## Поставновка задачи
Есть статичный массив, даны запросы минимума на отрезке, на которые надо отвечать очень быстро (O(1)).

## Алгоритм
Заведем массив stable[logn][n]

stable[k][i] - минимум на отрезке [i, i + 2 ^ k)

Как его пересчитавать? Идея в том, что полуинтервал [i, i + 2 ^ k) это минимум из двух меньших полуинтервалов [i, i + 2 ^ (k - 1)) и [i + 2 ^ (k - 1), i + 2 ^ k).

Пусть мы пересчитали, как отвечать на запросы?

Найдем такой максимальный k, что 2 ^ k  <= r - l + 1. Тогда утверждается, что ответов будет min (stable[k][l], stable[k][r - 2 ^ k + 1]). 

![[спарсы.png]]

Как видно на картинке синяя область это то, что покрывает только [l, l + 2 ^ k), зеленое - [r - 2 ^ k +1, r] , а фиолетовое - их общая часть.

Вот почему в алгоритме можно брать только такие операции, которым все равно на одинаковые числа. 

Почему это работает?

Докажем то, что 1) мы не берем число вне отрезка 2) мы смотрим весь наш отрезок

1) Мы работаем внутри отрезка потому, что 2 ^ k <= len, поэтому ни l + 2 ^ k - 1, ни r - 2 ^ k + 1 не выходят за отрезок.
2) Пусть мы смотрим не весь наш отрезок, тогда между l + 2 ^ k - 1 и r - 2 ^ k + 1 есть какое-то ненулевое расстояние. (r - 2 ^ k + 1) - (l  + 2 ^ k - 1) - 1 = r - l + 1 - 2 ^ (k + 1) > 0; получаем противоречие: r - l + 1 >= 2 ^ (k + 1), то есть k - не максимальная степень.


## КОД

Немного маленьких проблем реализации? Я рассказал все, кроме того как мы находим k. Действительно, если искать k бинпоиском или обычным фором, асимптотика не будет O(1). Что же делать?

Заметим, что длины запросов до n + 1, тогда просто для каждой длины предподсчитаем k и все.

```
int deg[n + 2];
for (int i = 2; i <= n + 1; i++)
	deg[i] = deg[i / 2] + 1; 
int stable[k][n]; // лучше k брать как константу (в районе 18-20)

for (int i = 0; i < n; i++)
	stable[0][i] = a[i];

for (int i = 1; i < k; i++)
	for (int l = 0, r = (1 << (i - 1)); r < n; l++, r++)
		stable[i][j] = min(stable[i - 1][l], stable[i - 1][r]);

// 1 << x == 2 ^ x



void get(int l, int r) {
	int len = r - l + 1;
	if (len == 0) return a[l];
	int loc_k = deg[len];
	return min(stable[loc_k][l], stable[loc_k][r - (1 << loc_k) + 1]);
}



```

Готово! Места в коде эта штука не так много, зато считает за O(1). Правда про память также не скажешь: nlog все такие порой довольно таки много и невкусно, по сравнению с ДОшкой (O(n)), Фенвиком (O(n)) и тд. У каждого свои плюсы и минусы.