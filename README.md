# CGE.jl
Julia package to compare graph embeddings.

| **Documentation** | **Build Status** |
|---------------|--------------|
|[![][docs-latest-img]][docs-dev-url]| [![Build Status][travis-img]][travis-url]  [![Coverage Status][codecov-img]][codecov-url] <br/>|

[docs-latest-img]: https://img.shields.io/badge/docs-latest-blue.svg
[docs-stable-img]: https://img.shields.io/badge/docs-stable-blue.svg
[docs-dev-url]: https://KrainskiL.github.io/CGE.jl/dev
[docs-stable-url]: https://KrainskiL.github.io/CGE.jl/stable

[travis-img]: https://travis-ci.org/KrainskiL/CGE.jl.svg?branch=master
[travis-url]: https://travis-ci.org/KrainskiL/CGE.jl

[codecov-img]: https://coveralls.io/repos/github/KrainskiL/CGE.jl/badge.svg?branch=master
[codecov-url]: https://coveralls.io/github/KrainskiL/CGE.jl?branch=master

## Details of the framework

To be presented at WAW2020: https://math.ryerson.ca/waw2020 with publication in Springer LNCS.

Framework version without landmarks (written in C) is available under: https://github.com/ftheberge/Comparing_Graph_Embeddings

## Installation

The current version uses Julia 1.4. Install `CGE.jl` by running in Julia REPL:
```julia
] add https://github.com/KrainskiL/CGE.jl
```

# Running the code

Code computes the Jenssen-Shannon divergence between two edge distributions:
(1) first one is based on the supplied graph clustering
(2) second one is based on the supplied embedding and a Geometric Chung-Lu model
When comparing embeddings, lower divergence is better.

Format:

```
julia CGE_CLI.jl -g edgelist_file -c clusters_file -e embedding_file [-a -v] [-l landmarks -f forced -m method]

## required flags:
-g: the edgelist (1 per line, whitespace separated, optionally with weights)
-c: the communities (in vertices order, 1 per line)
-e: the embedding (two formats accepted, see details below)
## optional flags:
-a: 'asis' flag, use if embedding is provided unordered with vertices in first column
-v: verbose, printing additional information
-l: number of landmarks to create
-f: number of forced landmarks to be created
-m: chosen ladnmark creation method: `rss`, `rss2`, `size`, `diamater`
```

# File Formats

For a graph with n nodes, the nodes can be represented with numbers 1 to n or 0 to n-1.

Three input files are required to run the algorithm:
1. the undirected graph, represented by a sequence of edges, 1 per line
2. a file with the node's cluster number, 1 per line, in numerical order of the nodes
3. the node embedding in the node2vec format (see below)

## Example of graph (edgelist) file

Nodes can be 0-based or 1-based
One edge per line with whitespace between nodes

```
1 32
1 22
1 20
1 18
1 14
1 13
1 12
1 11
1 9
1 8
...
```

Additional weights may be provided in third column

```
1 32 1.23
1 22 2.13
1 20 3.12
1 18 1.23
1 14 1.23
1 13 0.45
1 12 0.21
1 11 1.5
1 9 0.61
1 8 1.23
...
```

## Example of clustering file

Clusters can be 0-based or 1-based
Clusters: one value per line in the numerical order of the nodes

```
1
1
1
1
0
0
0
1
3
1
...
```
## Example of embedding file

Nodes are 0-based or 1-based in any order
Two formats are supported

**First format - unordered embedding**

First column indicates node number, the rest of the line is d-dimensional embedding

```
21 0.960689 -2.28209 3.65194 0.272646 -3.01281 1.0245 -0.329389 -2.95956
33 0.702187 -2.14331 4.25541 0.372346 -3.16427 1.41296 -0.390471 -4.49782
3 0.854487 -2.30527 4.10575 0.370613 -3.04878 1.46481 -0.120326 -4.02328
29 0.673825 -2.19518 4.00447 0.650003 -2.74663 0.757385 -0.505723 -3.2947
32 0.750248 -2.26306 4.04495 0.143616 -3.02735 1.49937 -0.400896 -4.04177
25 0.831608 -2.191 4.04712 0.786012 -2.85804 1.11308 -0.391722 -3.4645
28 1.14632 -2.20708 4.11004 0.338067 -2.86409 1.01202 -0.485711 -3.50161
...
```

**Second format - ordered embedding**

Only d-dimensional embedding in order of nodes is stored in file.
```
0.854487 -2.30527 4.10575 0.370613 -3.04878 1.46481 -0.120326 -4.02328
0.960689 -2.28209 3.65194 0.272646 -3.01281 1.0245 -0.329389 -2.95956
0.831608 -2.191 4.04712 0.786012 -2.85804 1.11308 -0.391722 -3.4645
1.14632 -2.20708 4.11004 0.338067 -2.86409 1.01202 -0.485711 -3.50161
0.702187 -2.14331 4.25541 0.372346 -3.16427 1.41296 -0.390471 -4.49782
0.673825 -2.19518 4.00447 0.650003 -2.74663 0.757385 -0.505723 -3.2947
0.750248 -2.26306 4.04495 0.143616 -3.02735 1.49937 -0.400896 -4.04177
...
```