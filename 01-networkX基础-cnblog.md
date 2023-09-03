## 创建一个图
- Graph是由一组节点和节点对（边）组成的。


```python
# 创建一个没有节点和边的空图。
import networkx as nx
G = nx.Graph()
```

## 01节点
- 图G可由多种方式生成。NetWorkX中包含许多图形生成函数（[graph generator functions](https://networkx.org/documentation/stable/reference/generators.html)），用于读取和写入多种格式的图形。

### 方式1 ：一次添加一个节点


```python
G.add_node(1)
G.add_node(2)
nx.draw(G=G,with_labels=True)   # 
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200808590-1359188120.png)
​    


###  方式2：从list中添加节点
  - 一次性添加多个节点
  - 在Networkx中，节点可以是int、str、xml、另外一个Graph



```python
# G = nx.Graph()
nodes_list = ['a','1']  # 注意 str 和 int
G.add_nodes_from(nodes_list)
nx.draw(G=G,with_labels=True)  
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200808263-572673607.png)
​    


### 方式3：添加节点时附加节点属性字典
- node_attribute_dict 通常用于存储节点及其相应的属性。在这个字典中，键代表节点的标识符，而值则是另一个包含该节点各种属性的字典。



```python
# node_attribute_dict 样式如下：
[
  ('节点1', {'属性1': '值1', '属性2': '值2'}),
  ('节点2', {'属性1': '值3', '属性2': '值4'}),
  ('节点3', {'属性1': '值5', '属性2': '值6'})
]
```




    [('节点1', {'属性1': '值1', '属性2': '值2'}),
     ('节点2', {'属性1': '值3', '属性2': '值4'}),
     ('节点3', {'属性1': '值5', '属性2': '值6'})]




```python
node_attributes_dict = [
    (4,{"color": "red"}),
    (5,{"color": "green"}),
]

G.add_nodes_from(node_attributes_dict)
nx.draw(G=G,with_labels=True)  
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200807823-785762621.png)
​    


### 方式4：将一个图中的节点合并到另外一个图中


```python
# 使用path_graph()创建一个图H
H = nx.path_graph(10)
# 将H中的节点添加到G中
G.add_nodes_from(H)
nx.draw(G=G,with_labels=True)  
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200807423-651345586.png)
​    


# 02 边

### 方式1：一次添加一条边
- add_edge()


```python
G.add_edge(1,5)
nx.draw(G=G,with_labels=True)  
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200807107-1758666791.png)
​    



```python
e = (2, 3)  
G.add_edge(*e) # 在Python中，*运算符用于解包元组、列表或其他可迭代对象。 等同于G.add_edge(2,3)
nx.draw(G=G,with_labels=True)  
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200806784-709555835.png)
​    


### 方式2：列表（list）
- 边元祖
    - (u,v)
    - (u,v,{'weight':3.1415}) ，u,v后面跟着的是边属性字典 （edge attribute dict）


```python
# (u,v)
edges_list = [(1,2),(1,4)]
G.add_edges_from(edges_list)
nx.draw(G=G,with_labels=True) 
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200806476-1550113620.png)
​    


### 方式3：从另外一个G添加边


```python
G.add_edges_from(H.edges)
nx.draw(G=G,with_labels=True) 
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200806137-1147982901.png)
​    


### 方式4：使用边属性字典添加边属性
- [set_edge_attributes()](https://networkx.org/documentation/stable/reference/generated/networkx.classes.function.set_edge_attributes.html#networkx.classes.function.set_edge_attributes)
- 


```python
# 定义边属性字典
edge_attributes_dict = {
    (1, 2): 1,
    (2, 3): 2,
    (3, 4): 1.5,
    (4, 5): 2.5
}
nx.set_edge_attributes(G=G,values = edge_attributes_dict,name = 'weight') 
nx.draw(G=G,with_labels=True) 
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200805833-1555910541.png)
​    


# 03 节点视图、边视图


```python
G.edges(data = True)
```




    EdgeDataView([(1, 5, {}), (1, 2, {'weight': 1}), (1, 4, {}), (1, 0, {}), (2, 3, {'weight': 2}), (4, 3, {'weight': 1.5}), (4, 5, {'weight': 2.5}), (5, 6, {}), (6, 7, {}), (7, 8, {}), (8, 9, {})])




```python
type(G.nodes())
```




    networkx.classes.reportviews.NodeView




```python
# 查看网络中的节点
list(G.nodes(data = True))
```




    [(1, {}),
     (2, {}),
     ('a', {}),
     ('1', {}),
     (4, {'color': 'red'}),
     (5, {'color': 'green'}),
     (0, {}),
     (3, {}),
     (6, {}),
     (7, {}),
     (8, {}),
     (9, {})]




```python
# 指定属性进行查看
list(G.nodes(data = 'color'))
```




    [(1, None),
     (2, None),
     ('a', None),
     ('1', None),
     (4, 'red'),
     (5, 'green'),
     (0, None),
     (3, None),
     (6, None),
     (7, None),
     (8, None),
     (9, None)]




```python
# 查看网络中的边
list(G.edges(data = True))
```




    [(1, 5, {}),
     (1, 2, {'weight': 1}),
     (1, 4, {}),
     (1, 0, {}),
     (2, 3, {'weight': 2}),
     (4, 3, {'weight': 1.5}),
     (4, 5, {'weight': 2.5}),
     (5, 6, {}),
     (6, 7, {}),
     (7, 8, {}),
     (8, 9, {})]



# 04 移除边、节点



```python
G.remove_node('a')
# G.remove_nodes_from([1,2,3])
list(G.edges(data = True))
```




    [(1, 5, {}),
     (1, 2, {'weight': 1}),
     (1, 4, {}),
     (1, 0, {}),
     (2, 3, {'weight': 2}),
     (4, 3, {'weight': 1.5}),
     (4, 5, {'weight': 2.5}),
     (5, 6, {}),
     (6, 7, {}),
     (7, 8, {}),
     (8, 9, {})]



# 05 可视化

#### 基于节点属性
- 在这个例子中，我们使用node_attributes_dict来存储节点4和节点5的颜色属性。
- 我们通过G.add_nodes_from(node_attributes_dict)将节点及其属性添加到图G中。
- 然后，使用nx.draw(G=G, with_labels=True)进行图的可视化，其中节点标签会显示。


```python
# 基于节点的color属性为节点着色
G.nodes(data = 'color') 
```




    NodeDataView({1: None, 2: None, '1': None, 4: 'red', 5: 'green', 0: None, 3: None, 6: None, 7: None, 8: None, 9: None}, data='color')




```python
# 根据节点属性设置节点颜色
node_colors = [data.get('color', 'blue') for node,data in G.nodes(data = True)]
node_colors
```




    ['blue',
     'blue',
     'blue',
     'red',
     'green',
     'blue',
     'blue',
     'blue',
     'blue',
     'blue',
     'blue']




```python
nx.draw(G=G,with_labels=True,node_color = node_colors)  
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200805524-1990421093.png)
​    


### 基于边属性


```python
# 1.获取边属性
edge_weights = nx.get_edge_attributes(G, 'weight')
edge_weights
```




    {(1, 2): 1, (2, 3): 2, (4, 3): 1.5, (4, 5): 2.5}




```python
# 2.自定义边的可视化样式,下面是基于weight来设置边的宽度 
edge_widths = [edge_weights.get((u, v), 1) for u, v in G.edges()]  # 1为默认宽度
edge_widths
```




    [1, 1, 1, 1, 2, 1.5, 2.5, 1, 1, 1, 1]




```python
# 3.可视化
nx.draw(G=G,with_labels=True,node_color = node_colors, width=edge_widths)
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200805190-1454615224.png)
​    



```python
# 也可以设置为权重的倍数
edge_widths = [2*edge_weights.get((u, v), 1) for u, v in G.edges()]  # 1为默认宽度
edge_widths
```




    [2, 2, 2, 2, 4, 3.0, 5.0, 2, 2, 2, 2]




```python
# 3.可视化
nx.draw(G=G,with_labels=True,node_color = node_colors, width=edge_widths)
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200804870-1922209019.png)
​    


# 06 有向图的构建
- DiGraph—Directed graphs with self loops
- nx.DiGraph


```python
G_2 = nx.DiGraph()
edges_list = [(1, 2), (1, 3), (1, 4), (3, 4)]
G_2.add_edges_from(edges_list)
nx.draw_networkx(G = G_2)
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200804535-445137374.png)
​    



```python
# 自定义可视化
edge_labels = nx.get_edge_attributes(G_2, "weight")
pos = nx.spring_layout(G_2) 
nx.draw_networkx_edge_labels(G_2, pos, edge_labels=edge_labels)  # 绘制图中边的权重
nx.draw_networkx(G_2, pos)
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200804196-2143905972.png)
​    


# 07 读入、写出

## write
- 常用的有gexf、edgelist
- 这部分比较简单，[write、read](https://networkx.org/documentation/stable/reference/readwrite/index.html)


```python
# 1.gexf
nx.write_gexf(G,'./graph/G_01.gexf')
# 3. 保存为边列表
nx.write_edgelist(G, "./graph/G_01.edgelist")
```

![](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200803859-1597081028.png)

## read


```python
# 1. read_edgelist
G_read = nx.read_edgelist('./graph/G_01.edgelist')
nx.draw_networkx(G_read)
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200803506-1869210449.png)
​    



```python
# 1. read_gexf
G_read2 = nx.read_gexf('./graph/G_01.gexf')
nx.draw_networkx(G_read2)
```


​    
![png](https://img2023.cnblogs.com/blog/2522257/202309/2522257-20230903200803117-853213699.png)
​    

