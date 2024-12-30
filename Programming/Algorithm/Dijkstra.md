# Dijkstra

## Dijkstra의 개요
음의 가중치가 없는 그래프에서 한 정점에서 모든 정점으로 가는 최소비용을 구하는 알고리즘. O(V²)의 시간복잡도를 가진다. 우선순위 큐를 사용한다면 O(E log V)가 될 수 있다.

## Dijkstra의 구현
1. 출발점으로부터 최단거리를 저장할 배열을 선언하고, 출발점을 제외하고 최대값으로 초기화한다. 출발점을 현재 노드로 설정한다.
2. 현재 노드에서 이동 가능한 노드를 찾아 현재 노드를 거쳐서 가는 비용과 이전에 저장한 비용을 비교해 더 작은 비용을 저장한다.
3. 현재 노드를 방문 처리하고, 미방문 상태면서 가장 비용이 작은 노드를 현재 노드로 설정한다.
4. 2~3을 반복한다.

```
void Dijkstra()
{
    dist[start] = 0;

    for(int i=0;i<n;i++)
    {
        int min = GetSmallestNode();
        visited[min] = true;
        for(int j=0;j<n;j++)
        {
            if(!visited[j])
            {
                if(dist[min] + weight[min][j] < dist[j])
                {
                    dist[j] = dist[min] + weight[min][j];
                }
            }
        }
    }
}
```