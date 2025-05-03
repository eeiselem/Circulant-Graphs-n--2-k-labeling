# Circulant-Graphs-(n,r-2)-k-labeling
Using branch and bound algorithmic approach

Best data structure to store graph
The recursive structure of the solve function relies heavily on the implicit call stack to manage the Depth -
First Search. Each recursive call adds a new layer to this stack, storing local variables like the current
node index, loop counters, and references to the shared state (labels, sums). This stack mechanism
inherently controls the traversal; when a call finishes and the layer is popped, execution resumes in the
previous layer, naturally ‘backtracking’. Since essentially only one path is stored in memory at a time, it
is highly memory-efficient compared to algorithms that might need to store wider sections of the search
tree simultaneously

Design strategy
The algorithm recursively searches for complete labelings or encounters dead -end results where
constraints cannot be met. When it reaches either state, it ‘backtracks’ by undoing the last label
assignment and attempting the next valid label for the current node, potentially backtracking further up
the recursion tree if necessary. Throughout each recursive step, the set of already used vertex labels and
the set of unique edge sums generated so far are tracked and updated. Branch and bound techniques are
employed to optimize the search for the minimal k. While this involves checking constraints at each step
(ensuring no repeats in vertex labels or edge sums), the core branch and bound pruning relies on
comparing the progress against the best k value identified so far from completed solutions. Specifically, if
the maximum label assigned in the current path already exceeds the best-known k, that path is considered
a ‘dead end’ for finding a better solution and is abandoned. Furthermore, potential labels are skipped
during the assignment process if using them would cause the path's maximum label to meet or exceed this
best k. The algorithm initiates by setting an upper bound for k of log 2n *(n2−3n)/2 [2] (see note in
comparison to mathematical property section below for a caveat on n=5, r=2 ) which establishes the initial
target value used for this bounding and pruning process, thus guiding the initial attempts to minimize k.
Note: This implementation requires unique vertex labels and unique edge sums. Although duplicate
vertex labels do work for n=5 (when r=n-3), this exception is disallowed in the code to avoid the
significant computational complexity it would introduce for o ther graph sizes.

How traversing will be applied
This algorithm does not traverse the original graph in a meaningful sense but rather traverses the state -
space tree that forms transiently during its execution. This tree is not explicitly built; it is more of a
conceptual model that underlies how the algorithm explores possible solutions. The traversal of this state-
space tree most closely follows a DFS traversal, as the recursive nature of the algorithm first explores
down the tree in search of possible solutions. It then moves back up when a solution or dead end is found,
explores the next alternative branch, and potentially starts another traversal down. On its way up, when
returning from a recursive call, is when the backtracking step occurs: the algorithm undoes the last
assignment and prepares to explore the next available branch.

Time complexity of algorithm
The worst-case time complexity could be characterized as O(cn), where n is the number of vertices and c
is some greater than 1 representing the effective branching factor. A potential upper bound of O(nn) might
be considered if the algorithm explored a complete search tree where each of the n levels had a branching
factor of n, which this algorithm nearly has in the first layer. However, this algorithm's design
incorporates mechanisms that prune this hypothetical search space tree. Constraint satisfaction checks
such as enforcing unique vertex labels and unique edge sums, eliminate invalid branches at each step. The
algorithm aslo restricts the search by discarding partial solutions determined incapable of improving upon
the best solution found up to any point. Consequently, the effective branching factor at each level is
substantially less than n on avertage and should decrease on every level. The constant c in the O(cn)
notation explains this reduced average branching, providing a more accurate representation of the
algorithm's constrained exponential growth in the worst case.
However, even in the best case the algorithm must still traverse a considerable number of states. This is
because its objective is not just to find any valid labeling, but to discover the optimal solution according
to the defined criteria (minimal k, with minimal label sum as a tiebreaker). Therefore, even if an optimal
path is found early, the algorithm explores or prunes other branches sufficiently to confirm that no better
solution exists.
