```
class MinHeap {
        int n;
        int maxSize;
        int[] heap;

        public MinHeap(int m) {
            maxSize = m;
            heap = new int[maxSize + 1];
            n = 0;
        }

        public void insert(int t) {
            if (n < maxSize + 1) {
                heap[++n] = t;
                if (n == 1) {
                    return;
                }
                shiftUp(n);
            }
        }

        public int getMin() {
            int min = heap[1];
            swap(1,n);
            shiftDown(--n);
            return min;
        }

        public void sort(){
            for (int i = 0; i < heap.length -1;i++) {
                swap(1,n);
                shiftDown(--n);
            }
        }

        private void shiftDown(int num) {
            int c;
            for (int i = 1;(c = 2*i) <= num;) {
                if (c + 1 <= n && heap[c + 1] < heap[c]) {
                    c++;
                }
                if (heap[i] > heap[c]) {
                    swap(i,c);
                    i = c;
                } else {
                    break;
                }
            }
        }

        private void shiftUp(int n) {
            int p;
            for (int i = n; (p = i / 2) > 0;) {
                if (heap[p] > (heap[i])) {
                    swap(p, i);
                    i = p;
                } else {
                    break;
                }
            }
        }

        private void swap(int a, int b) {
            int temp = heap[a];
            heap[a] = heap[b];
            heap[b] = temp;
        }

        public void printHeap() {
            for (int t : heap) {
                System.out.print(t + " ");
            }
        }

        public void printOrder(){
            for(int i = 0;i<heap.length-1;i++) {
                System.out.print(getMin() + " ");
            }
        }
    }
    ```
