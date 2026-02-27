---
title: "DarkGo: Go in Darknet"
description: "Play Go using a policy network trained with Darknet"
layout: darknet
order: 5
permalink: /darknet/darkgo/
---

[AlphaGo](https://deepmind.com/alpha-go.html) got me interested in game-playing neural networks.

I haven't actually read their paper yet but I've implemented what I imagine is sort of similar to their policy network. It is a neural network that predicts the most likely next moves in a game of Go. You can play along with professional games and see what moves are likely to happen next, make it play itself, or try to play against it!

![go game](game.png)

Currently DarkGo plays at about a 1 dan level. This is pretty good for a single network with no lookahead though, it only evaluates the current board state.

Come play me on the Online-Go Server! [https://online-go.com/user/view/434218](https://online-go.com/user/view/434218)


## Playing With A Trained Model ##

First [install Darknet](/darknet/install/), this can be accomplished with:

    git clone https://github.com/pjreddie/darknet
    cd darknet
    make

 Also download the weights file:

    wget https://data.pjreddie.com/files/go.weights

Then run the Go engine in testing mode:

    ./darknet go test cfg/go.test.cfg go.weights

This will bring up an interactive Go board. You can:

- Press `enter` to just play the 1st suggested move from the computer
- Enter a number like `3` to play that number suggestion
- Enter a location like `A 15` to play that move
- Enter `c A 15` to clear any pieces at `A 15`
- Enter `b A 15` to place a black piece at `A 15`
- Enter `w A 15` to place a white piece at `A 15`
- Enter `p` to pass the turn

Have fun!

If you want your network to be more powerful, add the flag `-multi` to the testing command. This evaluates the board at multiple rotations and flips to get better probability estimates. It can be slow on CPU but it's very fast if you have CUDA.

## Data ##
I use the Go dataset from Hugh Perkins' [Github](https://github.com/hughperkins/kgsgo-dataset-preprocessor). The data I feed Darknet is a 1-channel image that encodes the current game state. `1` stands for your pieces, `-1` stands for your opponent's pieces, and `0` stands for an empty space. The network predicts where the current player is likely to play next.

The full dataset I use after post-processing can be found here (3.0 GB), only needed for training):

- [go.train](https://data.pjreddie.com/files/go.train)
