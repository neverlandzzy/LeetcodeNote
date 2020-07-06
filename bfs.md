# BFS

### [LC200 Number of Islands](https://leetcode.com/problems/number-of-islands/)

```java
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0] == null || grid[0].length == 0) {
            return 0;
        }

        int count = 0;

        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    bfs(grid, i, j);
                }
            }
        }
        
        return count;
    }
    
    private void bfs(char[][] grid, int i, int j) {
        Queue<int[]> queue = new LinkedList<>();
        
        visit(grid, queue, i, j);
        
        while (!queue.isEmpty()) {
            int[] pos = queue.poll();
            
            int x = pos[0];
            int y = pos[1];
            
            visit(grid, queue, x + 1, y);
            visit(grid, queue, x - 1, y);
            visit(grid, queue, x, y + 1);
            visit(grid, queue, x, y - 1);
        }
    }
    
    private void visit(char[][] grid, Queue<int[]> queue, int i, int j) {
        int m = grid.length;
        int n = grid[0].length;
        
        if (i < 0 || i >= m || j < 0 || j >= n || grid[i][j] != '1') {
            return;
        }
        
        grid[i][j] = '*';
        queue.offer(new int[]{i, j});
    }
}
```

### 

