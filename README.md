# Lowest common ancestor of deepest leaves
## https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves

Given a rooted binary tree, return the lowest common ancestor of its deepest leaves.

Recall that:

The node of a binary tree is a leaf if and only if it has no children
The depth of the root of the tree is 0, and if the depth of a node is d, the depth of each of its children is d+1.
The lowest common ancestor of a set S of nodes is the node A with the largest depth such that every node in S is in the subtree with root A.
 
```
Example 1:

Input: root = [1,2,3]
Output: [1,2,3]

Explanation: 
The deepest leaves are the nodes with values 2 and 3.
The lowest common ancestor of these leaves is the node with value 1.
The answer returned is a TreeNode object (not an array) with serialization "[1,2,3]".

Example 2:

Input: root = [1,2,3,4]
Output: [4]

Example 3:

Input: root = [1,2,3,4,5]
Output: [2,4,5]
``` 

**Constraints:**

The given tree will have between 1 and 1000 nodes.
Each node of the tree will have a distinct value between 1 and 1000.

# Implementation : Iterative
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode lcaDeepestLeaves(TreeNode root) {
        if(root == null)
            return root;
        
        Map<TreeNode, TreeNode> parentMap = new HashMap<>();
        parentMap.put(root, null);
        Queue<TreeNode> q = new ArrayDeque<>();
        q.add(root);
        Queue<TreeNode> deepestLevelNodes = null;
        // First Pass : Level order traversal using BFS
        // After first pass, deepestLevelNodes queue will have leaf nodes at the deepest level
        while(!q.isEmpty()) {
            deepestLevelNodes = new ArrayDeque<>(q); 
            int size = q.size();
            for(int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                if(node.left != null) {
                   parentMap.put(node.left, node); 
                   q.add(node.left);   
                }
                if(node.right != null) {
                    parentMap.put(node.right, node);
                    q.add(node.right);
                }
            }
        }
        
        // Second Pass : We move upwards in the tree, from nodes at the deepest level to root node
        // We keep undating the deepestLevelNodes queue, when it have only one element
        // It means we found the lowest common ancestor of deepest leaves
        while(deepestLevelNodes.size() > 1) {
            Set<TreeNode> set = new HashSet<>();
            int size = deepestLevelNodes.size();
            for(int i = 0; i < size; i++) {
                set.add(parentMap.get(deepestLevelNodes.poll()));
            }
            for(TreeNode node : set) 
                deepestLevelNodes.add(node);
        }
        
        return deepestLevelNodes.poll();
        
    }
}
```

# References :
https://www.youtube.com/watch?v=QF7ZBH8mXHE
