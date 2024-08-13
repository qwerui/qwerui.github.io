# A* Pathfinding

## A* 알고리즘 개요
그래프에서 시점과 종점이 존재할 때 최소비용으로 가는 경로를 찾아내는 알고리즘. 다음 노드로 진행했을 경우 누적된 비용과 앞으로의 예상 비용을 합한 값(휴리스틱)이 최소가 되는 노드를 우선으로 탐색한다.

## A* 알고리즘 구현
![AStar_Result](/images/AStarResult.PNG)   
Unity 상에서 15*15 크기로 구현한 결과다. 휴리스틱으로는 단순히 목적지까지 x, y 값의 차이로 두었다. 빨강색이 탐색을 위해 방문한 노드, 파랑색이 최단경로다.   
**상세 코드**   
```
IEnumerator AStar()
{
    tiles[0, 0].gx = 0;
    queue.Add(tiles[0, 0]); //우선순위 큐

    while(queue.Count > 0)
    {
        var node = queue.Get();

        if(!set.Add(node))
        {
            continue;
        }
        node.CheckVisited(); //이미지 색 변경

        yield return new WaitForSeconds(0.1f);
            
        if(node.x == 14 && node.y == 14) //경로 발견
        {
            Debug.Log("Found!");
            yield break;
        }
        for(int i=-1;i<=1;i++)
        {
            for(int j=-1;j<=1;j++)
            {
                int nextx = Mathf.Clamp(node.x+i, 0, 14);
                int nexty = Mathf.Clamp(node.y+j, 0, 14);
                int cost = i == 0 || j == 0 ? 10 : 14;
                if(!set.Contains(tiles[nextx, nexty]) && !tiles[nextx, nexty].isWall)
                {
                    if(tiles[node.x, node.y].gx + cost + tiles[nextx,nexty].hx < tiles[nextx, nexty].gx + tiles[nextx, nexty].hx)
                    {
                        tiles[nextx, nexty].gx = tiles[node.x, node.y].gx+cost;
                        tiles[nextx, nexty].parentx = node.x;
                        tiles[nextx, nexty].parenty = node.y;
                        queue.Add(tiles[nextx, nexty]);
                    }
                }
            }
        }
    }
    //경로 미발견
    Debug.Log("Not Found!!");
}
```
