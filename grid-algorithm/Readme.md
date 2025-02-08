# Grid Algorithm

## To find an optimal solution for a responsive grid layout, we followed the following steps:

First we asked Chat GPT for a good way of creating a responsive grid layout.
It suggested a python script to generate a grid layout with a given number of columns from a pattern like

```
abbccccddddeeefg
hbbccccddddeeeij
klllmmmmmnneeeop
qlllmmmmmnnrrrst
ulllmmmmmnnrrrvw
xyz           äö
```

We then iterated on this algorithm with Chat GPT to make it also work with some edge cases like items that would come to big for the grid.
In version 4 of the algorithm we were happy with the results. You can find the detailed process [here](./ReadMe.md).

## Implementing it for Homarr

In a next step it was time to implement it with javascript and for the final usage. For this we iterated on a version of the algorithm that followed the same logic as the python script but adjusted for an array of items instead of the character-pattern.

Once the algorithm was implemented we added tests that should check if the algorithm gives the same results as the python script.

## More each cases

Because we also have dynamic-sections in Homarr we needed to use recursion and make a small adjustment to the algorithm to make it work with dynamic sections.
This was necessary, because a dynamic section could potentially also have to shrink which would result in it getting higher than previously. This had to be considered in the algorithm.

For this we also added tests to make sure the algorithm works with dynamic sections.

## Conclusion

We are quite happy with the algorithm. It is currently used to automatically generate the layouts of newly added layouts of a board in Homarr. It is also planned to add a layout system where only a base layout exists and the other layouts are generated from the base layout.

## Graphical representation of the algorithm

The unit tests test two things, if it generally works with items and if it works with dynamic sections. The expected changes are reflected in the below images.

### Items

![Grid Algorithm](./grid-algorithm.drawio.svg)

### Dynamic Sections

![Grid Algorithm with dynamic sections](./grid-with-dynamic-sections.drawio.svg)
