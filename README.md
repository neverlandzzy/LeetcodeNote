# Graph

### \*\*\*\*[**LC133 Clone Graph**](https://leetcode.com/problems/clone-graph/)\*\*\*\*

#### **BFS**

```java
class Solution {
    public Node cloneGraph(Node node) {
        if (node == null) {
            return null;
        }

        Map<Node, Node> map = new HashMap<>();
        Queue<Node> queue = new LinkedList<>();
        queue.offer(node);
        
        Node newHead = new Node(node.val);
        map.put(node, newHead);
        
        while (!queue.isEmpty()) {
            Node cur = queue.poll();
            
            for (Node neighbor: cur.neighbors) {
                if (!map.containsKey(neighbor)) {
                    Node copy = new Node(neighbor.val);
                    map.put(neighbor, copy);
                    queue.offer(neighbor);
                }
                
                map.get(cur).neighbors.add(map.get(neighbor));
            }
        }
        
        return newHead;
    }
}
```

#### **DFS**

```java
class Solution {    
    public Node cloneGraph(Node node) {
        HashMap<Node, Node> map = new HashMap<>();
        return helper(node, map);
    }
    
    private Node helper(Node node, HashMap<Node, Node> map) {
        if (node == null) {
    			return null;
    		}
    		
    		if (map.containsKey(node)) {
    			return map.get(node);
    		}
    
    		Node copy = new Node(node.val);
    		List<Node> neighbors = new ArrayList<>();
        map.put(node, copy);
    
        for (Node neighbor: node.neighbors) {
            if (neighbor == node) {
               	// add self-cycle;
                neighbors.add(copy);
            } else {
                neighbors.add(helper(neighbor, map));
            }
    		}
    
        copy.neighbors = neighbors;
        return copy;
    }
}
```

### [LC261 Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)

* 将输入2d数组转化为无向图
* 判断是否有环
* 判断是否每个点都被遍历

#### 将输入2d数组转化为无向图

```java
// 初始化模板，将输入转化为adjList
List<Set<Integer>> adjList = new ArrayList<>();
boolean[] visited = new boolean[n];
 	
for (int i = 0; i < n; i++) {
    adjList.add(new HashSet<Integer>());
}
	
for (int[] edge: edges) {
    adjList.get(edge[0]).add(edge[1]);
    adjList.get(edge[1]).add(edge[0]);
}
```

#### **BFS**

```java
public static boolean validTree(int n, int[][] edges) {
	
    // 初始化模板，将输入转化为adjList
    List<Set<Integer>> adjList = new ArrayList<>();
    boolean[] visited = new boolean[n];
     	
    for (int i = 0; i < n; i++) {
        adjList.add(new HashSet<Integer>());
    }
	
    for (int[] edge: edges) {
        adjList.get(edge[0]).add(edge[1]);
        adjList.get(edge[1]).add(edge[0]);
    }
    
    Queue<Integer> queue = new LinkedList<>();
    queue.add(0);
    
    // 判断是否有环
    while (!queue.isEmpty()) {
    	int node = queue.poll();
    	
    	if (visited[node]) {
    		return false;
    	}
    	
    	visited[node] = true;
    	
    	for (int neighbor: adjList.get(node)) {
    		queue.offer(neighbor);
    			// 从当前node的neighbor里删掉当前node，否则会重复visit
    		adjList.get(neighbor).remove(node);
    			// 若用ArrayList, 需要将node转化为Integer, 否则删的是index
    			// remove效率：ArrayList:O(n), HashSet: O(1)
    		//adjList.get(neighbor).remove((Integer)node);
    	}
    }
    
    // 判断是否每个点都被遍历
    for (boolean v: visited) {
    	if (!v) {
    		return false;
    	}
    }
    
    return true;
}
```

