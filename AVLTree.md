```
class AVLTree {

        Node root;

        static class Node {
            int value;
            Node parent;
            Node left;
            Node right;

            public Node(int value, Node parent) {
                this.value = value;
                this.parent = parent;
            }
        }

        public void insert(int value) {
            Node n = root;
            if (n == null) {
                root = new Node(value, null);
                return;
            }
            int height = 1;
            Node parent;
            do {
                parent = n;
                if (n.value > value) {
                    //n.balance++;
                    n = n.left;
                } else if (n.value < value) {
                    //n.balance--;
                    n = n.right;
                } else {
                    return;
                }
                height++;
            } while (n != null);
            Node newNode = new Node(value, parent);
            if (parent.value < value) {
                parent.right = newNode;
            } else {
                parent.left = newNode;
            }
            balance(newNode);
        }

        public void remove(int value) {
            Node removeNode = getNode(value);
            if (removeNode == null) {
                System.out.println("no such value");
                return;
            }
            if (removeNode.left == null && removeNode.right == null) {
                /*if (removeNode == leftOf(removeNode.parent)) {//不准
                    removeNode.parent.left = null;
                    removeNode = null;
                } else {
                    removeNode.parent.right = null;
                    removeNode = null;
                }*/
                Node parent = removeNode.parent;
                removeLeaf(removeNode);
                balance(parent);
                return;
            } else if (removeNode.left == null) {
                Node right = removeNode.right;
                if (removeNode.parent == null) {
                    root = right;
                } else if (removeNode == leftOf(removeNode.parent)) {//不准
                    removeNode.parent.left = right;
                } else {
                    removeNode.parent.right = right;
                }
                right.parent = removeNode.parent;
                balance(right);
                removeNode = null;
            } else if (removeNode.right == null) {
                Node left = removeNode.left;
                if (removeNode.parent == null) {
                    root = left;
                } else if (removeNode == leftOf(removeNode.parent)) {//不准
                    removeNode.parent.left = left;
                } else {
                    removeNode.parent.right = left;
                }
                left.parent = removeNode.parent;
                balance(left);
                removeNode = null;
            } else {
                int cmp = leftDepth(removeNode) - rightDepth(removeNode);
                if (cmp > 0) {
                    Node maxleft = max(removeNode.left);
                    Node parent = maxleft.parent;
                    removeNode.value = maxleft.value;
                    maxleft.value = value;
                    removeLeaf(maxleft);
                    balance(parent);
                } else {
                    Node minRight = min(removeNode.right);
                    Node parent = minRight.parent;
                    removeNode.value = minRight.value;
                    minRight.value = value;
                    removeLeaf(minRight);
                    balance(parent);
                }
            }
        }

        private void removeLeaf(Node leaf) {
            if (leaf.parent == null) {
                root = null;
                leaf = null;
                return;
            }
            if (leaf == leftOf(leaf.parent)) {
                leaf.parent.left = null;
            } else {
                leaf.parent.right = null;
            }
            leaf = null;
        }

        private Node min(Node node) {
            while (node.left != null) {
                node = node.left;
            }
            return node;
        }

        private Node max(Node node) {
            while (node.right != null) {
                node = node.right;
            }
            return node;
        }

        private Node getNode(int value) {
            Node n = root;
            int cmp;
            while (n != null) {
                cmp = n.value - value;
                if (cmp == 0) {
                    return n;
                } else if (cmp > 0) {
                    n = n.left;
                } else if (cmp < 0) {
                    n = n.right;
                }
            }
            return null;
        }

        private void balance(Node newNode) {
            if (newNode == null) {
                return;
            }
            Node unBalancedNode = isNotBalance(newNode);
            if (unBalancedNode != null) {
                if (unBalancedNode.left != null && unBalancedNode.left.left != null) {
                    rotateRight(unBalancedNode.left);
                } else if (unBalancedNode.left != null && unBalancedNode.left.right != null) {
                    rotateLeft(unBalancedNode.left.right);
                    rotateRight(unBalancedNode.left);
                } else if (unBalancedNode.right != null && unBalancedNode.right.right != null) {
                    rotateLeft(unBalancedNode.right);
                } else if (unBalancedNode.right != null && unBalancedNode.right.left != null) {
                    rotateRight(unBalancedNode.right.left);
                    rotateLeft(unBalancedNode.right);
                }
            }
        }

        private void rotateLeft(Node node) {
            Node parent = node.parent;
            if (parent.parent != null) {
                if (parent == leftOf(parent.parent)) {
                    parent.parent.left = node;
                } else {
                    parent.parent.right = node;
                }
            }
            parent.right = node.left;
            if (node.left != null) {
                node.left.parent = parent;
            }
            node.left = parent;
            node.parent = parent.parent;
            parent.parent = node;
            if (node.parent == null) {
                root = node;
            }
        }

        private void rotateRight(Node node) {
            Node parent = node.parent;
            if (parent.parent != null) {
                if (parent == leftOf(parent.parent)) {
                    parent.parent.left = node;
                } else {
                    parent.parent.right = node;
                }
            }
            parent.left = node.right;
            if (node.right != null) {
                node.right.parent = parent;
            }
            node.parent = parent.parent;
            parent.parent = node;
            node.right = parent;
            if (node.parent == null) {
                root = node;
            }
        }

        private Node rightOf(Node parent) {
            return parent.right;
        }

        private Node leftOf(Node parent) {
            return parent.left;
        }

        private Node isNotBalance(Node node) {
            do {
                if (!isBalancedNode(node)) {
                    return node;
                }
                node = node.parent;
            } while (node != null);
            return null;
        }

        private boolean isBalancedNode(Node node) {
            int balance = leftDepth(node) - rightDepth(node);
            return Math.abs(balance) < 2;
        }

        private int rightDepth(Node node) {
            if (node.left == null) {
                return 0;
            }
            return 1 + Math.max(leftDepth(node.left), rightDepth(node.left));
        }

        private int leftDepth(Node node) {
            if (node.right == null) {
                return 0;
            }
            return 1 + Math.max(leftDepth(node.right), rightDepth(node.right));
        }

        public void printTree() {
            printMidNode(root);
            /*System.out.println();
            printPreNode(root);
            System.out.println();
            printEndNode(root);*/
        }

        private void printEndNode(Node node) {
            if (node.left != null) {
                printMidNode(node.left);
            }
            if (node.right != null) {
                printMidNode(node.right);
            }
            System.out.print(node.value + " ");
        }

        private void printPreNode(Node node) {
            System.out.print(node.value + " ");
            if (node.left != null) {
                printMidNode(node.left);
            }
            if (node.right != null) {
                printMidNode(node.right);
            }

        }

        private void printMidNode(Node node) {
            if (node.left != null) {
                printMidNode(node.left);
            }
            System.out.print(node.value + " ");
            if (node.right != null) {
                printMidNode(node.right);
            }
        }

        public void bfsTree() {
            System.out.print("\n------------------ 树形打印开始 ------------------");

            if (this.root == null) {
                System.out.print("\n树为pw");
                return;
            }

            // 二叉树的高度
            int maxLevel = this.getDepth(this.root);
            // 满二叉树时的总结点数
            int fullTotal = (int) Math.pow(2, maxLevel) - 1;
            // 水平数组
            int[] nodes = new int[fullTotal];

            // 上一个节点的层次
            int prevLevel = 1;
            // 每层的起始下标
            int start = 0;
            // 每一层的元素的间距
            int stepSize = 0;

            // 广度优先遍历
            Queue<Node> queue = new LinkedList<Node>();
            queue.offer(this.root);
            Node node = null;
            // 如果用数组存储二叉树，indexMap中存储各节点对应数组的下标
            Map<Integer, Integer> indexMap = new HashMap<Integer, Integer>();
            while (!queue.isEmpty()) {
                // 删除队列头结点
                node = queue.poll();
                // 某节点在整棵树的第几层(ROOT为第1层)
                int levelOfFullTree = this.getLevelOfFullTree(node);

                // 如果当前深度比前一个节点的尝试大,则开始的新一层的节点访问
                if (levelOfFullTree > prevLevel) {
                    // 打印层次的节点
                    this.printTreeLevel(nodes);
                }

                // 计算新的层次的开始下标与步长
                start = (int) Math.pow(2, maxLevel - levelOfFullTree) - 1;
                stepSize = (int) Math.pow(2, maxLevel - levelOfFullTree + 1);
                // System.out.print("\n" + "start:" + start + ",stepSize:" + stepSize);

                // 记录节点的下标
                int idx = -1;
                if (node == this.root) {
                    indexMap.put(node.value, 1);
                    idx = 1;
                } else {
                    if (node == node.parent.left) {
                        idx = 2 * indexMap.get(node.parent.value) - 1;
                    } else if (node == node.parent.right) {
                        idx = 2 * indexMap.get(node.parent.value);
                    }
                    indexMap.put(node.value, idx);
                }

                // 计算映射到水平数组的位置
                int y = start + (idx - 1) * stepSize;
                nodes[y] = node.value;
                // System.out.print("\n" + "node.value:" + node.value + ",y:" + y);

                // 将节点的左孩子加入队列
                if (node.left != null) {
                    queue.offer(node.left);
                }
                // 将节点的右孩子加入队列
                if (node.right != null) {
                    queue.offer(node.right);
                }

                // 保留层次数,为下次循环使用
                prevLevel = levelOfFullTree;
            }

            // 打印最底层的节点，因为while的推出，最底层次的节点没有打印，在这里单独打印
            this.printTreeLevel(nodes);
            System.out.print("\n------------------ 树形打印结束 ------------------");
        }

        private void printTreeLevel(int[] nodes) {
            System.out.print("\n|");
            for (int j = 0; j < nodes.length; j++) {
                if (nodes[j] == 0) {
                    // 打印两位数字的占位符
                    System.out.printf("--");
                } else {
                    // 打印节点
                    System.out.printf("%02d", nodes[j]);
                    // 重置数组
                    nodes[j] = 0;
                }
            }
            System.out.print("|");
        }

        public int getDepth(Node node) {
            if (node == null) {
                return 0;
            }

            return 1 + Math.max(this.getDepth(node.left), this.getDepth(node.right));
        }

        public int getLevelOfFullTree(Node node) {
            int depth = 0;
            Node t = node;
            while (t != null) {
                depth++;
                t = t.parent;
            }
            return depth;
        }

    }
```
