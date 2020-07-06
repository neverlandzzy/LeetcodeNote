# Graph 模板

### \*\*\*\*[**LC133 Clone Graph**](https://leetcode.com/problems/clone-graph/)\*\*\*\*

**BFS**

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

**DFS**

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

