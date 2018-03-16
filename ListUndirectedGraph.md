```
package graph;

public class ListUndirectedGraphDemo {
    public static void main(String[] args) {
        char[] vexs = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        char[][] edges = new char[][]{
                {'A', 'C'},
                {'A', 'D'},
                {'A', 'F'},
                {'B', 'C'},
                {'C', 'D'},
                {'E', 'G'},
                {'F', 'G'}};
        ListUndirectedGraph graph = new ListUndirectedGraph(vexs, edges);
        graph.printGraph();
    }

    static class ListUndirectedGraph{
        HeadNode[] vertexs;

        public ListUndirectedGraph(char[] vers, char[][] edgs) {
            int vertexNum = vers.length;
            vertexs = new HeadNode[vertexNum];
            for (int i = 0; i< vertexNum;i++) {
                vertexs[i] = new HeadNode(vers[i]);
            }
            for (int i = 0; i < vertexNum; i++) {
                //for (int j = 0; j < vertexNum; j++) {
                    vertexs[getPosition(edgs[i][0])].insert(getPosition(edgs[i][1]));
                    vertexs[getPosition(edgs[i][1])].insert(getPosition(edgs[i][0]));
                //}
            }

        }

        private int getPosition(char vertex) {
            int result = -1;
            for (int i = 0; i< vertexs.length; i++) {
                if (vertex == vertexs[i].vertex) {
                    result = i;
                }
            }
            return result;
        }

        public void printGraph() {
            Node node;
            for (int i = 0; i< vertexs.length; i++) {
                print(vertexs[i].vertex + "-> ");
                node = vertexs[i].toIndex;
                while (node != null) {
                    print(node.index + " ");
                    node = node.nextNode;
                }
                print("\n");
            }
        }

        public static void print(String s) {
            System.out.print(s);
        }
    }

    static class HeadNode {
        char vertex;
        Node toIndex;

        public HeadNode(char ch) {
            vertex = ch;
        }

        public void insert(int index) {
            if (toIndex == null) {
                toIndex = new Node(index);
                return;
            }
            Node node = toIndex;
            Node preNode = node;
            while(node != null) {
                preNode = node;
                node = node.nextNode;
            }
            preNode.nextNode = new Node(index);
        }
    }

    static class Node {
        int index;
        Node nextNode;

        public Node(int index) {
            this.index = index;
        }
    }
}

```
