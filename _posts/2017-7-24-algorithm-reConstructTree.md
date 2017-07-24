---
layout: post
title: 根据前序序列和中序序列构造二叉树
categories: algorithm
tags: 数据结构
author: NoPerfectName
excerpt: 根据前序序列和中序序列构造二叉树
---

* content
{:toc}


```java
public class RecoverTree {
    private class TreeNode {
        public int value;
        public TreeNode left = null;
        public TreeNode right = null;
        public TreeNode(int value) {
            this.value = value;
        }
    }

    public TreeNode reConstructBinaryTree (int[] pre, int[] in) {
        return reConstructBinaryTree(pre, in, 0, 0, pre.length);
    }

    public TreeNode reConstructBinaryTree (int[] pre, int[] in, int preStart, int inStart, int length) {
        if (length <= 0) {
            return null;
        }

        TreeNode treeNode = new TreeNode(pre[preStart]);
        int index = find(in, inStart, inStart + length, pre[preStart]);
        if (index < 0) {
            return null;
        }
        int len1 = index - inStart;
        int len2 = length - len1 - 1;
        treeNode.left = reConstructBinaryTree(pre, in, preStart + 1, inStart, len1);
        treeNode.right = reConstructBinaryTree(pre, in, preStart + len1 + 1, index + 1, len2);
        return treeNode;
    }

    public void preOrder (TreeNode treeNode) {
        if (treeNode == null) return;

        System.out.println(treeNode.value);
        preOrder(treeNode.left);
        preOrder(treeNode.right);
    }

    public void inOrder (TreeNode treeNode) {
        if (treeNode == null) return;

        preOrder(treeNode.left);
        System.out.println(treeNode.value);
        preOrder(treeNode.right);
    }

    public int find(int[] array, int fromIndex, int toIndex, int key) {
        if (fromIndex >= toIndex || toIndex > array.length || fromIndex < 0) return -1;

        for (int i = fromIndex; i < toIndex; i++) {
            if (array[i] == key) {
                return i;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        RecoverTree recoverTree = new RecoverTree();
        int[] pre = {1,2,4,7,3,5,6,8};
        int[] in = {4,7,2,1,5,3,8,6};
        TreeNode treeNode = recoverTree.reConstructBinaryTree(pre,in);
        recoverTree.preOrder(treeNode);
    }
}
```
