# Graph 模板

### \*\*\*\*[**LC133 Clone Graph**](https://leetcode.com/problems/clone-graph/)\*\*\*\*

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

