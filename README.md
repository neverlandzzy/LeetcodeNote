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

#### DFS

```java
public static boolean validTree(int n, int[][] edges) {

	  List<List<Integer>> adjList = new ArrayList<>();
	  boolean[] visited = new boolean[n];
	
	  for (int i = 0; i < n; i++) {
        adjList.add(new ArrayList<Integer>());
	  }
	
    for (int[] edge: edges) {
        adjList.get(edge[0]).add(edge[1]);
        adjList.get(edge[1]).add(edge[0]);
    }
    
    // check cycle
    if (hasCycle(adjList, 0, -1, visited)) {
    	return false;
    }
    
    // check if all vertices are visited
    for (boolean v: visited) {
    	if (!v) {
    		return false;
    	}
    }
    
    return true;
}

// check if an undirected graph has cycle started from vertex u
// v is the next vertex from u
private static boolean hasCycle(List<List<Integer>> adjList, int u, int pre, boolean[] visited) {
    visited[u] = true;
	
    for (int i = 0; i < adjList.get(u).size(); i++) {
		    int v = adjList.get(u).get(i);
		
		    if ((visited[v] && pre != v) || (!visited[v] && hasCycle(adjList, v, u, visited))) {
			      return true;
		    }
		}
		
		return false;
}
```

### \*\*\*\*[**LC207 Course Schedule**](https://leetcode.com/problems/course-schedule/)\*\*\*\*

* 将输入2d数组转化为有向图
* 判断是否有拓扑排序

**BFS**

```java
class Graph {
    private int V;
    private int E;
    private int[] indegree;
    private ArrayList<List<Integer>> neighbors;
    
    public Graph(int V, int[][] edges) {
        this.V = V;
        this.E = edges.length;
        indegree = new int[V];
        neighbors = new ArrayList<>();
        
        for (int i = 0; i < V; i++) {
            neighbors.add(new ArrayList<>());
        }
        
        for (int i = 0; i < E; i++) {
            int m = edges[i][1];
            int n = edges[i][0];
            neighbors.get(m).add(n);
            indegree[n]++;
        }
    }
    
    public boolean hasTopologicalOrder() {
        Queue<Integer> queue = new LinkedList<>();
        
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        int counter = 0;
        
        while (!queue.isEmpty()) {
            int v = queue.poll();
            counter++;
            
            for (int w: neighbors.get(v)) {
                if (--indegree[w] == 0) {
                    queue.offer(w);
                }
            }
        }
        
        return counter == V;
    }
}
```

**DFS**

```java
class Digraph {
	private int V;
	private int E;
	private List<ArrayList<Integer>> adj;
	private boolean hasCycle;
	private boolean[] marked;
	private boolean[] onStack;
	
	public Digraph(int n, int[][] edges) {
		this.V = n;
		this.E = edges.length;
		adj = new ArrayList<ArrayList<Integer>>();
		for (int i = 0; i < V; i++) {
			ArrayList<Integer> list = new ArrayList<Integer>();
			adj.add(list);
		}
		for(int i = 0; i < E; i++) {
			int v = edges[i][1];
			int w = edges[i][0];
			
			adj.get(v).add(w);
		}
		
	}
	
	public boolean hasTopologicalOrder() {
		marked = new boolean[V];
		onStack = new boolean[V];
		
		for (int i = 0; i < V; i++) {
			if (!marked[i]) {
				dfs(i);
			}
		}
		
		return !hasCycle;
	}
	
	private void dfs(int v) {
		marked[v] = true;
		onStack[v] = true;
		
		for (int w: adj.get(v)) {
			if (hasCycle) return;
			if (!marked[w]) dfs(w);
			if (onStack[w]) hasCycle = true;
		}
		
		onStack[v] = false;
	}
}

```

### \*\*\*\*[**LC210 Course Schedule**](https://leetcode.com/problems/course-schedule-ii/)\*\*\*\*

* 将输入2d数组转化为有向图
* 求得拓扑排序

**BFS**

```java
class Digraph {
	private int V; 
	private int E;
	private List<List<Integer>> adj;
	private int[] indegree;
	private Queue<Integer> queue;
	
	public Digraph(int n, int[][] edges) {
		this.V = n;
		this.E = edges.length;
		indegree = new int[V];
		adj = new ArrayList<>();
		queue = new LinkedList<>();
		
		for (int i = 0; i < V; i++) {
			ArrayList<Integer> list = new ArrayList<>();
			adj.add(list);
		}
		
		for (int i = 0; i < E; i++) {
			int v = edges[i][1];
			int w = edges[i][0];
			
			adj.get(v).add(w);
			indegree[w]++;
		}
	}
	
	public int[] topologicalOrders() {
		int[] topologicalOrder = new int[V];
		
		for (int i = 0; i < V; i++) {
			if (indegree[i] == 0) {
				queue.add(i);
			}
		}
		
		int counter = 0;
		
		while (!queue.isEmpty()) {
			int v = queue.poll();
			topologicalOrder[counter] = v;
			counter++;
			
			for (int w: adj.get(v)) {
				if (--indegree[w] == 0) {
					queue.add(w);
				}
			}
		}
		
		if (counter < V) return new int[0];
		else return topologicalOrder;
	}
}
```

**DFS**

```java
class Digraph {
	private int V;
	private int E;
	private List<ArrayList<Integer>> adj;
	private boolean hasCycle;
	private boolean[] marked;
	private boolean[] onStack;
	private Stack<Integer> postorder;
	
	public Digraph(int n, int[][] edges) {
		this.V = n;
		this.E = edges.length;
		adj = new ArrayList<ArrayList<Integer>>();
		
		for (int i = 0; i < V; i++) {
			ArrayList<Integer> list = new ArrayList<Integer>();
			adj.add(list);
		}
		
		for (int i = 0; i < E; i++) {
			int v = edges[i][1];
			int w = edges[i][0];
			adj.get(v).add(w);
		}
		
	}
	
	public int[] topologicalOrders() {
		marked = new boolean[V];
		onStack = new boolean[V];
		postorder = new Stack<Integer>();
		
		for (int i = 0; i < V; i++) {
			if (!marked[i]) dfs(i);
		}
		
		if (hasCycle) return new int[0];
		else {
			int[] topoloicalOrder = new int[V];
			for (int i = 0; i < V; i++) {
				topoloicalOrder[i] = postorder.pop();
			}
			return topoloicalOrder;
		}
	}
	
	private void dfs(int v) {
		marked[v] = true;
		onStack[v] = true;
		
		for (int w: adj.get(v)) {
			if (hasCycle) return;
			else if (!marked[w]) dfs(w);
			else if (onStack[w]) hasCycle = true;
		}
		postorder.push(v);
		onStack[v] = false;
	}
}
```

