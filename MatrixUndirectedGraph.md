```
package graph;

import java.io.IOException;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;

public class MatrixUndirectedGraphDemo {

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
        MatrixUndirectedGraph graph= new MatrixUndirectedGraph(vexs, edges);
        //MatrixUndirectedGraph graph= new MatrixUndirectedGraph();
        graph.printMatrix();
        print("dfs:");
        graph.depthFirstSearch();
        print("bfs:");
        graph.breadthFirstSearch();
    }

    static class MatrixUndirectedGraph {
        char[] vertexs;
        int[][] edgeMatrix;

        public MatrixUndirectedGraph(){
            print("input vertex number:");
            int vertexNum = readInt();
            print("input edge number:");
            int edgeNum = readInt();
            if (vertexNum < 1 || edgeNum < 1 || edgeNum > vertexNum * (vertexNum - 1)) {
                print("invalid input");
                return;
            }
            char[] vertexs = new char[vertexNum];
            /*for (char vertex:vertexs) {

                vertex = readChar();
            }*/
            for (int i = 0 ; i < vertexNum;i++) {
                print("vertex " + i + ":");
                vertexs[i] = readChar();
            }

            char[][] edges = new char[vertexNum][vertexNum];
            for (int i = 0; i < edgeNum;i++) {
                print("edge " + i + " from vertex:");
                char fromVertex = readChar();
                print("edge " + i + " to vertex:");
                char toVertex = readChar();
                edges[i][0] = fromVertex;
                edges[i][1] = toVertex;
            }
            setupData(vertexs,edges);

        }

        private char readChar() {
            char ch = '0';
            do {
                try {
                    ch = (char) System.in.read();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }while(!(ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z'));
            return ch;
        }

        private int readInt() {
            Scanner scanner = new Scanner(System.in);
            return scanner.nextInt();
        }

        public MatrixUndirectedGraph(char[] vertex, char[][] edges) {
            setupData(vertex, edges);
        }

        private void setupData(char[] vertex, char[][] edges) {
            int vertexNum = vertex.length;
            vertexs = new char[vertexNum];
            edgeMatrix = new int[vertexNum][vertexNum];

            for (int index = 0; index < vertexNum; index ++) {
                vertexs[index] = vertex[index];
            }
            //print("edges length:" + edges.length);
            for (int i = 0; i < edges.length; i++) {
                int startPosition = getPosition(edges[i][0]);
                int endPostion = getPosition(edges[i][1]);
                edgeMatrix[startPosition][endPostion] = 1;
                edgeMatrix[endPostion][startPosition] = 1;
            }
        }

        public void breadthFirstSearch() {
            LinkedList<Integer> queue = new LinkedList<Integer>();
            boolean[] visited = new boolean[vertexs.length];
            for (boolean value:visited) {
                value = false;
            }
            for (int i = 0; i< vertexs.length; i++) {
                if (!visited[i]) {
                    print(vertexs[i] + " ");
                    visited[i] = true;
                    queue.offer(i);
                }
                while (!queue.isEmpty()) {
                    int index = queue.poll();
                    for (int adjIndex= adj(index); adjIndex >=0; adjIndex = otherAdj(index, adjIndex)) {
                        if (!visited[adjIndex]) {
                            queue.offer(adjIndex);
                            print(vertexs[adjIndex] + " ");
                            visited[adjIndex] = true;
                        }
                    }
                }
            }
        }

        public void depthFirstSearch() {
            boolean[] visited = new boolean[vertexs.length];
            for(int i = 0;i< vertexs.length; i++) {
                visited[i] = false;
            }
            for (int i = 0;i < vertexs.length; i++) {
                if (!visited[i]) {
                    depthFirstSearch(i,visited);
                }
            }
        }

        private void depthFirstSearch(int index, boolean[] visited) {
            visited[index] = true;
            print(vertexs[index] + " ");
            for (int adjIndex= adj(index); adjIndex >=0; adjIndex = otherAdj(index, adjIndex)) {
                if (!visited[adjIndex]) {
                    depthFirstSearch(adjIndex,visited);
                }
            }
        }

        private int otherAdj(int index, int adj) {
            for (int i = adj + 1; i< vertexs.length; i++) {
                if (edgeMatrix[index][i] == 1) {
                    return i;
                }
            }
            return -1;
        }

        private int adj(int index) {
            for (int i = 0; i< vertexs.length; i++) {
                if (edgeMatrix[index][i] == 1) {
                    return i;
                }
            }
            return -1;
        }


        private int getPosition(char vertex) {
            int result = -1;
            for (int i = 0; i< vertexs.length; i++) {
                if (vertex == vertexs[i]) {
                    result = i;
                }
            }
            return result;
        }

//        void printMatrix(char[] vertexs,int[][] edgeMatrix) {
        void printMatrix() {
            print("  ");
            for (char vertex:vertexs) {
                print(vertex + " ");
            }
            print("\n");
            for (int i = 0; i < vertexs.length; i++) {
                print(vertexs[i] + " ");
                for (int j=0;j<vertexs.length; j++) {
                    print(edgeMatrix[i][j] + " ");
                }
                print("\n");
            }
        }

    }

    public static void print(String s) {
        System.out.print(s);
    }
}

```
