[DFS（图论）](https://oi-wiki.org/graph/dfs/)
>特征： 递归调用自身
在遍历图时跳过已打过标记的点，以确保 每个点仅访问一次

大致结构：
```
DFS(v) // v 可以是图中的一个顶点，也可以是抽象的概念，如 dp 状态等。
  在 v 上打访问标记
  for u in v 的相邻节点
    if u 没有打过访问标记 then
      DFS(u)
    end
  end
end
```
