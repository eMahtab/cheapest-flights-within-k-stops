# Cheapest flights within k stops
## https://leetcode.com/problems/cheapest-flights-within-k-stops

There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.

You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

!["Cheapest flights within K stops"](cheapest-flights-within-k-stops-example.jpg?raw=true "Cheapest flights within K Stops")

For the above example the cheapest flight from V0 to V2 with at most 2 stops will be (V0 -> V1 -> V4 -> V2 = 7)

```
Constraints:

1. 1 <= n <= 100
2. 0 <= flights.length <= (n * (n - 1) / 2)
3. flights[i].length == 3
4. 0 <= fromi, toi < n
5. fromi != toi
6. 1 <= pricei <= 104
7. There will not be any multiple flights between two cities.
8. 0 <= src, dst, k < n
9. src != dst
```

```java
class Solution {
    
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
     
        // Build the adjacency matrix
        int adjMatrix[][] = new int[n][n];
        for (int[] flight: flights) {
            adjMatrix[flight[0]][flight[1]] = flight[2];
        }
        
        // Shortest distances array
        int[] distances = new int[n];
        
        // Shortest steps array
        int[] currentStops = new int[n];
        Arrays.fill(distances, Integer.MAX_VALUE);
        Arrays.fill(currentStops, Integer.MAX_VALUE);
        distances[src] = 0;
        currentStops[src] = 0;
        
        // The priority queue would contain (node, cost, stops)
        PriorityQueue<int[]> minHeap = new PriorityQueue<int[]>((a, b) -> a[1] - b[1]);
        minHeap.offer(new int[]{src, 0, 0});
        
         while (!minHeap.isEmpty()) {
             
            int[] info = minHeap.poll();
            int node = info[0], stops = info[2], cost = info[1];
             
             // If destination is reached, return the cost to get here
            if (node == dst) {
                return cost;
            }
             
            // If there are no more steps left, continue 
            if (stops > K) {
                continue;
            }
             
             
            // Examine and relax all neighboring edges if possible 
            for (int nei = 0; nei < n; nei++) {
                if (adjMatrix[node][nei] > 0) {
                    int dU = cost, dV = distances[nei], wUV = adjMatrix[node][nei];
                    
                    // Better cost?
                    if (dU + wUV < dV) {
                        minHeap.offer(new int[]{nei, dU + wUV, stops + 1});
                        distances[nei] = dU + wUV;
                    }
                    else if (stops < currentStops[nei]) {
                        // Better steps?
                        minHeap.offer(new int[]{nei, dU + wUV, stops + 1});
                    }
                    currentStops[nei] = stops;
                }
            }
         }
        
        return -1;
    }
}
```

### Code execution
!["Code Execution"](code-execution.jpg?raw=true "Code execution")


## References:
1. https://leetcode.com/problems/cheapest-flights-within-k-stops/solution
2. https://leetcode.libaoj.in/cheapest-flights-within-k-stops.html
