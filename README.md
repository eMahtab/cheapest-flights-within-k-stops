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

## Implementation : O(N^2 ∗ Log(N))
```java
class Solution {
    
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        int[][] adjMatrix = new int[n][n];
        for (int[] flight : flights) {
            adjMatrix[flight[0]][flight[1]] = flight[2];
        }
        int[] distances = new int[n];
        int[] currentStops = new int[n];
        Arrays.fill(distances, Integer.MAX_VALUE);
        Arrays.fill(currentStops, Integer.MAX_VALUE);
        distances[src] = 0;
        currentStops[src] = 0;
        
        // The priority queue will contain (node, cost, stops)
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        minHeap.offer(new int[] { src, 0, 0 });
        while (!minHeap.isEmpty()) {
            int[] info = minHeap.poll();
            int node = info[0], cost = info[1], stops = info[2];
            if (node == dst) {
                return cost;
            }
            if (stops > K) {
                continue;
            }
            for (int nei = 0; nei < n; nei++) {
                if (adjMatrix[node][nei] > 0) {
                    int newCost = cost + adjMatrix[node][nei];
                    // Relaxation based on cost and stops
                    if (newCost < distances[nei] || stops + 1 < currentStops[nei]) {
                        minHeap.offer(new int[] { nei, newCost, stops + 1 });
                        distances[nei] = newCost;
                        currentStops[nei] = stops + 1;
                    }
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
