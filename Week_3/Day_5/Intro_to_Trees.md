# Intro to Trees - Org Charts

* A tree shows the relationship between different data and how they correspond to one another.

* Each circle, or data/dataset in the tree is called a **node**
  * The node at the very top is called the **root** node, and nodes at the very bottom are called **leaf** nodes (no children)
  * Every node, except for the root node, has a single **parent** node. Every node that is not a leaf node has one or more **chilren** nodes
  * All children with the same parent are **siblings**

* All the connections between the nodes are called **edges**

* Trees are a great data structure to use when your data has **hierarchical relationships**. Trees allow us to use these relationships in code making the data easier to process.
  * We can build trees as long as our data structures have single parents and many children.

* A great place to have tree structures is a browser's DOM
  - Parent: Parent Element
  - Children: Child Elements
  - Data: Attributes: (id, classes, etc)

* Or the File System

  - Parent: Parent directory `../`
  - Children: All the sub-directories or files with in this directory
  - Data: Contents of the file (if file) + metadata (permissions, etc)