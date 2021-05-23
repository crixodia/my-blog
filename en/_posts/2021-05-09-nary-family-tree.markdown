---
layout: post
title: "Java family tree: n-ary trees"
date: 2021-05-09 15:14:00 -0500
categories: structures java tutorial gui
language: en
author: Cristian Bastidas
image: /assets/images/thumbnail-trees.webp
---
Unlike binary trees, n-ary trees do not have just two child nodes. In this post we will make an application based on n-ary trees which allow us to manage family trees.

- [N-ary trees](#n-ary-trees)
- [Family tree](#family-tree)
- [Defining the objects](#defining-the-objects)
  - [Node](#node)
  - [Tree](#tree)
  - [Files: Save and open trees](#files-save-and-open-trees)
  - [Uniqueness of the test object](#uniqueness-of-the-test-object)
- [GUI](#gui)
- [Download the entire project](#download-the-entire-project)

## N-ary trees

In a nutshell, an n-ary tree is a no linear abstract data structure whose definition could be present recursively with a set of nodes. Those nodes will have a list that points to other nodes.

<img src="https://github.com/crixodia/nary-family-tree/raw/master/assets/arbol-nario.png" style="display:block; margin-left: auto; margin-right:auto;" alt="Representación gráfica árbol nario">

## Family tree

One of its applications is based on the family tree. Well, it meets the requirements of an n-ary tree (hierarchy, parent-child relationships). The image below shows an example of a family tree based on Greek mythology.

<img src="https://github.com/crixodia/nary-family-tree/raw/master/assets/family-tree.png" style="display:block; margin-left: auto; margin-right:auto;" alt="Ejemplo de árbol familiar">

## Defining the objects

Since it is a family tree, we will implement a generic object for this. And the tree itself as specified below. It is necessary to emphasize that the methods implemented in the tree class can change depending on your needs and the type of object that is used as value in the nodes.

<img src="https://github.com/crixodia/nary-family-tree/raw/master/assets/uml.png" style="display:block; margin-left: auto; margin-right:auto;" alt="Ejemplo de árbol familiar">

### Node

Our tree node is easy to implement. As specified by its structure, we will use a parent object, another for the root, and a list of child objects.

The method to add nodes is given by:

```java
public Node add(Object o){
    Node newNode = new Node(o);
    this.children.add(newNode);
    return newNode;
}
```
See [Node](https://github.com/crixodia/nary-family-tree/blob/master/ArbolGen/src/CapaNegocio/Node.java).

### Tree

The difficulty of this project lies in the implementation of the tree. Since trees can be stored in files, a parser must be designed to translate the tree into text and another for the reverse process. In addition, to display the tree in the GUI it is also necessary to implement a parser that allows it to be translated into a `DefaultMutableTreeNode` type object.

For this work I relied on the following [pseudocode](https://stackoverflow.com/questions/21735468/parse-indented-text-tree-in-java). To understand it you must have basic notions of stacks. You can review the source code of the `text2DTree (string)` method.

```python
definir pila
pila.push(primera línea)
while existan objetos
    S1 = pila.peek()
    S2 = leer objeto
    if profundidad(S1) < profundidad(S2)
        S1.addChild(S2)
        pila.push(S2)
    else
        while profundidad(S1) >= profundidad(S2) Y pila.size() >= 2
            pila.pop()
            S1 = pila.peek()
    S1.addChild(S2)
    pila.push(S2)
return pila.get(0)
```
### Files: Save and open trees

Then the process of reading the files is simplified by saving the parent of each item. You just have to treat the file according to the specified format. For this case we have each node on a different line, its parent and value separated by `,` and the attributes of the object by `:`

```
Aang:Katara:0:0
Aang:Katara,Tenzin:Pema:0:0
Tenzin:Pema,Meelo::0:0
Tenzin:Pema,Jinora::0:0
Tenzin:Pema,Ikki::0:0
Aang:Katara,Bumi::0:0
Aang:Katara,Kya::0:0
```
When we read line by line we can get its parent and the value of the node and then execute `addNewNode (parent, value)`. For instance `addNewNode (Aang, Tenzin)`.

To save the trees we also use the pointer to the parent node. In this way, you only have to go through the entire tree in pre-order and go concatenating the parent and the value of the node according to the specified format.

<img src="https://github.com/crixodia/nary-family-tree/raw/master/assets/tree-traversal.png" style="display:block; margin-left: auto; margin-right:auto;" width="500px" alt="Recorrido de árbol en pre-orden">

The methods search, remove, depth and modify could be found in [Tree](https://github.com/crixodia/nary-family-tree/blob/master/ArbolGen/src/CapaNegocio/Tree.java).

### Uniqueness of the test object

What happens if a child has the same ID as its father? We will create uniqueness by the spouse of the father. That is, when we create an object after inserting it on a tree we will verify that this does not exist based on its spouse ID. To do this, we will override `equals(o)` method.

```java
@Override
public boolean equals(Object obj) {
    GenObj aux = (GenObj) obj;
    return nombre.equals(aux.nombre) && conyuge.equals(aux.conyuge);
}
```

## GUI

The GUI will allow us to create, modify, insert, delete; save and open files, and visualize the attributes from each node.

<img src="https://github.com/crixodia/nary-family-tree/raw/master/assets/gui.jpg" style="display:block; margin-left: auto; margin-right:auto;" alt="Recorrido de árbol en preorden">

You can [download](https://github.com/crixodia/nary-family-tree/tree/master/examples) trees samples and open them to test their functionality.

<img src="https://github.com/crixodia/nary-family-tree/raw/master/assets/open.jpg" style="display:block; margin-left: auto; margin-right:auto;" alt="Recorrido de árbol en preorden">

## Download the entire project

Donwload the entire project on [GitHub](http://github.com/crixodia/nary-family-tree). Leave your comment 😋.

<div style="margin-left: auto; text-align:right;">
<i><small>
Este artículo también está en <a href="{{ site.baseurl }}{% link _posts/2021-04-08-nary-family-tree.markdown %}">español</a>
</small></i>
</div>