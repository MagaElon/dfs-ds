# dfs-ds
dfs
import java.util.*;

public class TarjanSCC {
    private int V;
    private List<List<Integer>> graph;

    private int time = 0;
    private int[] low;
    private int[] disc;
    private boolean[] inStack;
    private Deque<Integer> stack;

    public TarjanSCC(int V) {
        this.V = V;
        graph = new ArrayList<>();
        for (int i = 0; i < V; i++)
            graph.add(new ArrayList<>());

        low = new int[V];
        disc = new int[V];
        inStack = new boolean[V];
        stack = new ArrayDeque<>();

        Arrays.fill(disc, -1);
    }

    public void addEdge(int u, int v) {
        graph.get(u).add(v);
    }

    public void dfs(int u) {
        disc[u] = low[u] = time++;
        stack.push(u);
        inStack[u] = true;

        // Explore all neighbors
        for (int v : graph.get(u)) {
            if (disc[v] == -1) {
                dfs(v);
                low[u] = Math.min(low[u], low[v]);
            }
            else if (inStack[v]) {
                low[u] = Math.min(low[u], disc[v]);
            }
        }

        // Root of SCC found
        if (low[u] == disc[u]) {
            System.out.print("SCC: ");
            while (true) {
                int v = stack.pop();
                inStack[v] = false;
                System.out.print(v + " ");
                if (v == u) break;
            }
            System.out.println();
        }
    }

    public void findSCCs() {
        for (int i = 0; i < V; i++)
            if (disc[i] == -1)
                dfs(i);
    }

    public static void main(String[] args) {
        TarjanSCC g = new TarjanSCC(7);

        g.addEdge(0, 1);
        g.addEdge(1, 2);
        g.addEdge(2, 0);
        g.addEdge(1, 3);
        g.addEdge(3, 4);
        g.addEdge(4, 5);
        g.addEdge(5, 3);
        g.addEdge(5, 6);

        g.findSCCs();
    }
}
