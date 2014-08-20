---
title: Overlap graph-based sequence assembly in bioinformatics
author:
- name: Luiz Carlos Irber Jr
  affiliation: Michigan State University
  email: irberlui@msu.edu
date: August 2014
abstract:
bibliography: <!-- \bibliography{bibs/qualsbib-pandoc.bib}
This is a hack for Emacs users so that RefTeX knows where your bibfile is,
and you can use RefTeX citation completion in your .md files. -->
...

# Introduction

# Theory

## Overlap-Layout-Consensus approaches

### Shortest string

  - Parsimony

### Overlap Graph

  - Hamiltonian Path (NP-Hard)

### Unitig Graph

  - Transitive reduction on Overlap Graph
  - After reduction each vertex is a unitig
  - Not Hamiltonian anymore! (but still NP-Hard to find an optimal tour)

### String Graph

  - Remove contained reads
  - Build the overlap graph
  - Map the contained reads
  - Estimate genome size (A-stats) and identify unique segments
    (label unique edges)
  - Traversal count for each edge by solving a
    minimum cost network flow problem.
  - Drop edges with zero count (false overlaps)

## De Bruijn approaches
