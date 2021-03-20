## A: [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)  
> Hi guys, this is my Java solution. I read this post, which is very helpful.
> The basic idea is here:
> Say we have 2 arrays, PRE and IN.
> Preorder traversing implies that PRE[0] is the root node.
> Then we can find this PRE[0] in IN, say it's IN[5].
> Now we know that IN[5] is root, so we know that IN[0] - IN[4] is on the left side, IN[6] to the end is on the right side.
> Recursively doing this on subarrays, we can build a tree out of it :)
> Hope this helps.
```javascript
public TreeNode buildTree(int[] preorder, int[] inorder) {
    return helper(0, 0, inorder.length - 1, preorder, inorder);
}

public TreeNode helper(int preStart, int inStart, int inEnd, int[] preorder, int[] inorder) {
    if (preStart > preorder.length - 1 || inStart > inEnd) {
        return null;
    }
    TreeNode root = new TreeNode(preorder[preStart]);
    int inIndex = 0; // Index of current root in inorder
    for (int i = inStart; i <= inEnd; i++) {
        if (inorder[i] == root.val) {
            inIndex = i;
        }
    }
    root.left = helper(preStart + 1, inStart, inIndex - 1, preorder, inorder);
    root.right = helper(preStart + inIndex - inStart + 1, inIndex + 1, inEnd, preorder, inorder);
    return root;
}
```

#### 作者开头这么说，解法这么做，一脸茫然。真应该多看看评论区，评论区有人针对做出解答
```
Can anyone tell me why preStart + inIndex - inStart + 1 for the preStart of the right tree? 
```
```
@longmanzz To determine preStart for the right sub-tree, we have to move towards the right of 
the preOrder array by a certain 'offset' value. 
This means we are skipping the next 'offset' number of indexes in the preOrder array to find 
the root of the right sub-tree.
This offset is determined by finding the size of the left sub-tree 
which is equal to the no. of indexes to the left of root found the inOrder array. 
Just think of (inIndex - inStart) as this offset. In other words, offset = ( inIndex - inStart ) .

Therefore the preStart for the right sub-tree is = preStart + offset + 1

It will be simpler if you draw the problem on a paper and iterate for this example:
Inorder sequence: D B E A F C
Preorder sequence: A B D E C F
```
