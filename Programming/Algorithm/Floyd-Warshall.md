# Floyd-Warshall

## Floyd-Warshall 개요
그래프 상에서 모든 노드 간 최소비용을 구하는 알고리즘. O(V³)의 시간복잡도를 가진다.

## Floyd-Warshall 구현
graph = 행의 인덱스에서 열의 인덱스로 갈 때의 비용   
i = 시작 노드의 인덱스   
j = 목표 노드의 인덱스   
k = 거쳐갈 중간 노드의 인덱스   
```
void FloydWarshall()
{
    for(int k=0;k<n;k++)
    {
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(graph[i][j] > graph[i][k] + graph[k][j])
                {
                    graph[i][j] = graph[i][k] + graph[k][j];
                }
            }
        }
    }
}
```