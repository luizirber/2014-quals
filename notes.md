# What it should contain:

    - What is the basic procedure for overlap graph-based assembly (or string
      graph assembly)?

    - Compare the algorithms and data structures for building the overlap
      graph from a large number of reads.

    - What are the differences and disadvantages/advantages of the described
      approaches over de Bruijn graph based assembly?

    - How do the attached papers handle sequencing errors? How do they
      simplify the graphs by removing tips and bubbles? How do they take
      advantage of paired-end information?

    - Please describe one possible direction for future research.


# Paper reviews

## Pevzner 2001 (original De Bruijn graph)

  - Avoid overlap step (O(n^2) if unoptimized)
  - Problems:
    * Sequencing errors introduce many erroneous edges
    * Long perfect repeats
    * Tangles
  - Pipeline:
    * Error correction
      - in OLC it is done at the consensus step
      - Definitions:
        * S (a collection of reads from a sequencing project)
        * l (an integer)
        * $\delta$ (an upper bound on the number of error in each DNA read)
        * $S_l$ is a set of all l-tuples from reads $s_1,...,s_n$ and its
          reverse complements. (Spectrum of S)
      - Error correction problem: given S, l and $\delta$, introduce up to
        $\delta$ corrections in each read in S in such a way that $|S_l|$ is
        minimized.
      - Greedy solution: look for error corrections in the reads that reduce
        the size of $S_l$ by $2l$ -> elimitates 86.5% of errors.
      - May introduce errors (data corruption), but they are consistent errors
        that can be corrected later by a majority rule or a by a variation of
        the Churchill-Waterman algorithm.
    * Build the De Bruijn graph (dBG)
      - TODO
    * Eulerian superpaths on standard reads
      - TODO
    * Use DB data to map reads into some edge(s) of the dBG
      - Double Barreled data (pairwise end sequencing/paired end sequencing)
      - After mapping most mate-pairs of reads correspond to paths that
        connect the positions of these reads in the dBG
      - Paths are long artificial mate-reads -> rerun Eulerian superpath alg.
    * Build scaffolds, using DB data as 'bridges' between different contigs.

## Myers 2005 (original string graph)

  - e: mean error rate of the read's sequence under an exponentially
    distributed arrival rate model
  - Pipeline
    * Overlap: compute all 2e overlaps of length T or more between the reads
      - given e, choose T so that 2e is exceedingly low (i.e., e=2.5%, T=50)
      - filter based on q-grams
      - Two types of overlaps (f and g are reads, o is the overlap):
        * Containment: overlap where both ends of a read are
          extreme (0 or length of the read) -> read is contained
        * Proper: either o.f.beg(o.f.end) or o.g.beg(o.g.end) is extreme,
          but not both.
    * Remove contained reads, since there will be a tour spelling the
      sequence of reads containing it.
      - About 40% of the reads are contained
        -> 64% of the overlaps involve a contained read
    * Build the string graph.
      - For every read there will be two vertices, one for each end of the read.
      - Add the overhangs (non-overlapping regions) of the overlap as
        directed edges.
      - The graph is read coherent: any path in the graph models a valid
        assembly of the reads
        * this is not true for de Bruijn graphs built from k-mers of the reads
        * most effort for this approach is for restoring this coherence
    * Remove transitive edges
      - if overlap(f, g), overlap(g, h) and overlap(f, h)
        f->h edge is unnecessary, because f -> g -> h spell the same sequence.
    * Map contained reads
    * Estimate genome size and identify unique segments
      - Label unique edges
    * Minimum cost network flow
      - Find the minimum traversal counts that satisfy edge bounds


## Velvet (paper 1):

  - Techniques
    * de Bruijn graph
    * Tour bus error correction algorithm
      - Dijkstra-like BFS, implemented with a Fibonnaci heap: O(N logN)
      - 
    * Breadcrumb
      - Use paired end data to resolve repeats
  - Pipeline:
    * Hash the reads into k-mers
      - Roadmaps
    * Build the dBG using the roadmaps
      - Simplification:
        if node A has only one outgoing arc to node B
        and node B has only one ingoing arc
        merge the two nodes (and their twins),
        transfering arc, read and sequence information as appropriate
    * Error removal
      - Removal of low coverage nodes. Remove errors, but:
        * Are they genuine errors os biological variants?
        * Errors need to be randomly distributed in reads
          (not true for most sequencing technologies)
      - Resolve topological features:
        * tips
        * bulges / bubbles
        * erroneous connections
    * Resolution of repeats
      - with short read pairs

## SGA (paper 3):
 - Techniques
    * String graph
    * FM-index (full-text minute-space index), derived from
      compressed BWT (Burrows-Wheeler transform)
      - Compressed representation of the full set of sequence reads
  - Pipeline
    *  
  

## ReadJoiner (paper 2):

  - Techniques:
    * integer encoding
    * suffix sorting
    * integer sorting
    * binary search
    * bottom-up traversal of lcp-interval trees
      - lcp: longest common prefix

  - Avoid random access, explore data locality

  - Replace maximality constraint with a
    minimum length constraint imposed on each suffix-prefix match.
    (like Simpson and Durbin)

  - Partition: increase running time, but large reduction in
    overall memory peak

  - Pipeline

    * prefilter: remove reads
      - containing ambiguity codes
      - are prefixes or suffixes of other reads

    * overlap:
      - map prefiltered reads in memory
      - enumerate non-redundant irreducible suffix-prefix matches

    * assembly: build string graph and traverses it to output the contigs.


## Fermi (paper 4):

