##  BST遍历

```java
	/**
     * 先序遍历
     */
    public void preOrder(){
        preOrder(root);
    }

    /**
     * 先序遍历以node为根的BST，递归算法
     * @param node
     */
    private void preOrder(Node node){
        if(node == null){
            return;
        }

        System.out.println(node.e);
        preOrder(node.left);
        preOrder(node.right);
    }

    /**
     * BST的非递归前序遍历
     * 采用栈这一种数据结构来辅助实现，先将根节点入栈，当栈不为空时，弹出栈顶元素；
     * 当当前节点右子树不为空时，入栈；然后判断左子树不为空时，入栈
     */
    public void preOrderNR(){
        Stack<Node> stack = new Stack<>();
        stack.push(root);
        while(!stack.isEmpty()){
            Node cur = stack.pop();
            System.out.println(cur.e);

            if(cur.right != null){
                stack.push(cur.right);
            }
            if(cur.left != null){
                stack.push(cur.left);
            }
        }

    }

    /**
     * 中序遍历
     */
    public void inOrder(){
        inOrder(root);
    }

    /**
     * 中序遍历以node为根的BST，递归算法
     * @param node
     */
    private void inOrder(Node node){
        if(node == null){
            return;
        }

        inOrder(node.left);
        System.out.println(node.e);
        inOrder(node.right);
    }

    /**
     * BST的非递归中序遍历
     */
    public void inOrderNR(){
        Stack<Node> stack = new Stack<>();
        Node cur = root;
        while(cur != null || !stack.isEmpty()){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            if(!stack.isEmpty()){
                cur = stack.pop();
                System.out.println(cur.e);
                cur = cur.right;
            }
        }
    }

    /**
     * 后序遍历
     */
    public void postOrder(){
        postOrder(root);
    }

    /**
     * 后序遍历以node为根的BST，递归算法
     * @param node
     */
    private void postOrder(Node node){
        if(node == null){
            return;
        }

        postOrder(node.left);
        postOrder(node.right);
        System.out.println(node.e);
    }

    /**
     * BST的非递归后序遍历
     */
    public void postOrderNR(){
        Stack<Node> stack = new Stack<>();
        Stack<Integer> stack2 = new Stack<>();    //辅助栈，用来判断子节点返回父节点时处于左节点还是右节点

        int left = 1;    //在辅助栈里表示左节点
        int right = 2;    //在辅助栈里表示右节点

        Node cur = root;

        while(cur != null || !stack.isEmpty()){
            while (cur != null){
                //将节点压入栈1，并在栈2将节点标记为左节点
                stack.push(cur);
                stack2.push(left);
                cur = cur.left;
            }

            while(!stack.isEmpty() && stack2.peek() == right){
                //如果是从右子节点返回父节点，则任务完成，将两个栈的栈顶弹出
                stack2.pop();
                System.out.println(stack.pop().e);
            }

            if(!stack.isEmpty() && stack2.peek() == left){
                //如果是从左子节点返回父节点，则将标记改为右子节点
                stack2.pop();
                stack2.push(right);
                cur = stack.peek().right;
            }
        }
    }

    /**
     * 层次遍历
     */
    public void levelOrder(){
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            Node cur = queue.remove();
            System.out.println(cur.e);

            if(cur.left != null){
                queue.add(cur.left);
            }
            if(cur.right != null){
                queue.add(cur.right);
            }
        }
    }
```


参考：
https://blog.csdn.net/coder__666/article/details/80349039