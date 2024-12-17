# Отчет по расчетной работе по дисциплине ПиОИвИС
# Цель 
Ознакомиться с основами теории графов, cпособами представления графов, базовыми алгоритмами для работы с разными видами графов.
# Задача
Найти граф каркасов для неориентированного графа. Граф задается списком смежности, данные которой задает пользователь . Вывод работы программы выводится в консоль.
# Список ключевых понятий
-**Граф** - это топологичекая модель, которая состоит из множества вершин и множества соединяющих их рёбер. При этом значение имеет только сам факт, какая вершина с какой соединена.
-**Вершина** - точка в графе, отдельный объект.
-**Ребро** - неупорядоченная пара двух вершин, которые связаны друг с другом.
-**Список смежности** — один из способов представления графа в виде коллекции списков вершин. Каждой вершине графа соответствует список, состоящий из «соседей» этой вершины.
-**Ориентированный граф** - граф, в котором рёбра имеют направления.
-**Неориентированный граф** - граф, в котором рёбра не имеют направления.
-**Компонента связности** - множество таких вершин графа, что между любыми двумя вершинами существует маршрут.
-**Связный граф** - граф, в котором существует путь между любыми двумия вершинами.
-**Цикл** - цепь, в котором последняя вершина совпадает с первой.
-**Каркасом**, или **остовным деревом** для этого графа называется связный подграф этого графа, содержащий все вершины графа и не имеющий циклов
![](http://www.hpcc.unn.ru/image.php?id=1009)
# Текстовый пример
Ввод количества вершин и ребер
![](https://github.com/user-attachments/assets/b152b4cb-9ca0-4e5a-b9fd-7fb2742604be)
Ввод вершин, соединяющих ребро
![](https://github.com/user-attachments/assets/cff8c2e5-64f4-4f29-b00f-28e58bec142c)
Вывод возможных остовых деревьев
![](https://github.com/user-attachments/assets/f3ff2112-7a36-45d5-a534-887853e02d52)
# Пример кода
~~~c++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Поиск в глубину (DFS)
void dfs(int v, const vector<vector<int>>& graph, vector<bool>& visited) {
    visited[v] = true;
    for (int to : graph[v]) {
        if (!visited[to]) {
            dfs(to, graph, visited);
        }
    }
}

// Проверка на связность
bool isConnected(int vertexCount, const vector<pair<int, int>>& edges) {
    vector<vector<int>> tempGraph(vertexCount);
    for (const auto& edge : edges) {
        tempGraph[edge.first].push_back(edge.second);
        tempGraph[edge.second].push_back(edge.first);
    }

    vector<bool> visited(vertexCount, false);
    dfs(0, tempGraph, visited);
    return all_of(visited.begin(), visited.end(), [](bool v) { return v; });
}

// Генерация всех остовных деревьев
void generateMST(int edgeIndex, int vertexCount, const vector<pair<int, int>>& edges, vector<pair<int, int>>& currentMST, vector<vector<pair<int, int>>>& allMSTs) {
    // Условие для остовного дерева: должно быть vertexCount - 1 рёбер
    if (currentMST.size() == vertexCount - 1) {
        if (isConnected(vertexCount, currentMST)) {
            allMSTs.push_back(currentMST);
        }
        return;
    }

    for (int i = edgeIndex; i < edges.size(); i++) {
        currentMST.push_back(edges[i]);
        generateMST(i + 1, vertexCount, edges, currentMST, allMSTs);
        currentMST.pop_back(); // Убираем последнее добавленное ребро
    }
}

int main() {
    int vertexCount, edgeCount;
    setlocale(LC_ALL, "ru");
    cout << "Введите количество вершин: ";
    cin >> vertexCount;
    cout << "Введите количество рёбер: ";
    cin >> edgeCount;

    vector<pair<int, int>> edges; // Список рёбер

    for (int i = 0; i < edgeCount; i++) {
        int a, b;
        cout << "Введите вершины для ребра " << (i + 1)<<": ";
        cin >> a >> b;
        a--; // Уменьшаем на 1 для 0-индексации
        b--;
        edges.push_back({ a, b });
    }

    vector<vector<pair<int, int>>> allMSTs; // Все остовные деревья
    vector<pair<int, int>> currentMST;

    // Генерируем все остовные деревья
    generateMST(0, vertexCount, edges, currentMST, allMSTs);

    // Вывод всех остовных деревьев
    cout << "Все возможные остовные деревья:\n";
    for (const auto& mst : allMSTs) {
        cout << "{ ";
        for (const auto& edge : mst) {
            cout << "(" << edge.first + 1 << ", " << edge.second + 1 << ") ";
        }
        cout << "}\n";
    }

    return 0;
}
~~~
# Вывод
В результате выполнения этой работы я познакомилась с такой структурой данных как графы, научилась работать с алгоритмом поиска в глубину и решила поставленную задачу.














