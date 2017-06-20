```
public class BinarySearchTree<T extends Comparable<T>> {

    private Node<T> root;

    class Node<T> {
        T data;
        Node<T> leftChild;
        Node<T> rightChild;

        public Node(T element) {
            this.data = element;
        }
    }

    public boolean insertBST(T element) {
        return insertBST(root, element);
    }

    public boolean insertBST(Node<T> parent, T element) {
        if (root == null) {
            Node<T> s = new Node<>(element);
            s.leftChild = s.rightChild = null;
            root = s;
        } else if (parent.data == element) {
            return false;
        } else {
            if (element.compareTo(parent.data) < 0) {
                if (parent.leftChild == null) {
                    parent.leftChild = new Node<>(element);
                } else {
                    insertBST(parent.leftChild, element);
                }
            } else {
                if (parent.rightChild == null) {
                    parent.rightChild = new Node<>(element);
                } else {
                    insertBST(parent.rightChild, element);
                }
            }
        }
        return true;
    }

    public boolean delete(T element) {
        Node<T> node = search(root,element);
        Node<T> nodeTemp;
        if (node == null) {
            return false;
        }
        if (node.leftChild == null && node.rightChild == null) {
            node = null;
        } else if (node.rightChild == null) {
            nodeTemp = node.leftChild;
            node.data = nodeTemp.data;
            node.rightChild = nodeTemp.rightChild;
            node.leftChild = nodeTemp.leftChild;
            nodeTemp = null;
        } else if (node.leftChild == null) {
            nodeTemp = node.rightChild;
            node.data = nodeTemp.data;
            node.leftChild = nodeTemp.leftChild;
            node.rightChild = nodeTemp.rightChild;
            nodeTemp = null;
        } else {
            Node<T> s = node.leftChild;
            while (s.rightChild.rightChild != null) {
                s = s.rightChild;
            }
            node.data = s.rightChild.data;
            s.rightChild = null;
        }
        return true;
    }

    private void delete(Node<T> node) {

    }

    public Node<T> search(Node<T> parent, T element) {
        if (parent == null) {
            return null;
        }
        if (parent.data == element) {
            return parent;
        } else if (parent.data.compareTo(element) < 0 ) {
            return search(parent.rightChild, element);
        } else {
            return search(parent.leftChild, element);
        }
    }



    public void printTree() {
        printTree(root);
    }

    public void printTree(Node<T> parent) {
        if (parent == null) {
            return;
        }
        if (parent.leftChild != null) {
            printTree(parent.leftChild);
        }
        System.out.println("printTree : " + parent.data);
        if (parent.rightChild != null) {
            printTree(parent.rightChild);
        }

    }
}
```
