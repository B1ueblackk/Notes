

## Data Structure

### Hash Pointers

Blockchain is a linked list using hash pointers.

 ![](https://i-blog.csdnimg.cn/blog_migrate/a67f05bd86c6f6651e8bdfafd55b36e0.png)

The hash value of block **n** is related to the value of block **n-1**. So it only needs the value of the last block to check whether the entire chain has been modified.

### Merkle Tree

familiar with binary tree, the difference is the pointers in Merkle tree is hash pointers

![](https://i-blog.csdnimg.cn/blog_migrate/3548bfd7b35898835792cf37ba70b77a.png)

### Block

contains two parts: **header** and **body**

* the block header has the information like root hash(the hash value of the root of Merkle tree)...
* the block body has the list of transactions

