---
layout: post
title: "The game of life with Java"
date: 2021-05-16 11:55:00 -0500
categories: structures java challenge gui
language: en
author: Cristian Bastidas
image: /assets/images/thumbnail-life.webp
---
The game of life is a cellular automaton designed by John Horton Conway in 1970. This is a game of zero players, which means that its evolution is determined by the initial state, and it is not required a data input after the game starts.

- [Rules](#rules)
- [GUI](#gui)
  - [Saving and loading patterns](#saving-and-loading-patterns)
  - [Grid](#grid)
  - [Animations and GIF files](#animations-and-gif-files)
- [Examples](#examples)

## Rules

The game board is a flat grid which represents a toroid. It is composed by squares (the cells) extended on it. Therefore, each cell has 8 neighbors including its diagonal. The cells have two states, "live" or "dead" (on and off). The cells state evolve within discrete time units (turns). The state of the whole cells is required to compute the next turn. All cells update simultaneously on each turn, following this rules.

1. **A dead cell with exactly 3 living cells will born.**
2. **A living cell with 2 or 3 living cells will still be alive, otherwise will die.**

From [Wikipedia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)

## GUI

The controls window will allow us to run the game. Furthermore, we can see that the population of each state and its generation.

<img src="https://github.com/crixodia/java-game-of-life/raw/master/images/contro-gui.png" alt="Controles" style="display:block; margin-left: auto; margin-right:auto;">

### Saving and loading patterns

An important feature is that we can load and save patterns. If you want to save one, you must draw a pattern on the grid.

<img src="https://github.com/crixodia/java-game-of-life/raw/master/images/save-dialog.png" alt="Controles" style="display:block; margin-left: auto; margin-right:auto;">

Once we have drawn our patterns, we can also open them. This operation will clean the grid and load the pattern.

<img src="https://github.com/crixodia/java-game-of-life/raw/master/images/open-dialog.png" alt="Controles" style="display:block; margin-left: auto; margin-right:auto;">

### Grid

In the grid we will see the state changes. As the board has a toroidal form, **the grid is finite**.

<img src="https://github.com/crixodia/java-game-of-life/raw/master/images/grid-gui.png" alt="Controles" style="display:block; margin-left: auto; margin-right:auto;">

### Animations and GIF files

You can generate animations. Before that you have to provide a path (folder) to save all generate images. You can also change the colors. In addition, you can use a  file to generate random patterns based on it.

<img src="https://github.com/crixodia/java-game-of-life/raw/master/images/GIF_dialog.png" alt="Controles" style="display:block; margin-left: auto; margin-right:auto;">

## Examples

Download some patterns [here](https://github.com/crixodia/java-game-of-life/blob/master/examples/). My loved one is ([osc.jglf](https://github.com/crixodia/java-game-of-life/blob/master/examples/osc.jglf)).

<img src="https://github.com/crixodia/java-game-of-life/raw/master/images/grid-gif.gif" alt="Controles" style="display:block; margin-left: auto; margin-right:auto;">

When you generate an animation, you will get a GIF file similar to the next image. Notice that you could also create an infinite loop.

<img src="https://github.com/crixodia/java-game-of-life/raw/master/examples/GIFgen/Profile_Life_NFT/animation.gif" alt="Controles" style="display:block; margin-left: auto; margin-right:auto;">

Download this [project](https://github.com/crixodia/java-game-of-life) 🧐 Please, leave your comment and feedback.

<div style="margin-left: auto; text-align:right;">
<i><small>
Este artículo también está en <a href="{{ site.baseurl }}{% link _posts/2021-04-11-java-game-of-life.markdown %}">español</a>
</small></i>
</div>