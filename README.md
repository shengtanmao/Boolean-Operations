# 651-red-blue-segment-intersection
Shengtan Mao

### Project description
Given red and blue segments in 2-dimensional space, such that the red segments do not intersect among themselves and the blue segments do not intersect among themselves, we seek to find all intersections between the red and blue segments. We use a method that avoids explicitly calculating the coordinates of the intersections and instead depends on keep track of the ordering of the segments to determine the intersections.

0. Before the actual algorithm, a brute force algorithm is implemented. This checks all pairs of red and blue segments to count how many intersections there are. The intersections here are checked by applying the orientation determinant several times. This algorithm is inefficient since it needs to check all possible pairs.
1. We define what a flag is. It is a wrapper class that contains a colored segment, one endpoint of that segment, and whether that endpoint is the start or terminal of the segment. More importantly we define a ordering of flags and a comparison of flags to sort them. This sorted flag list is used to determine where and what order the event points are in the sweep.
2. The first algorithm [name this -- give a name to important algorithms and make them your own. Makes it easier to refer to them.  The choice of what to name is important.] sweeps a single color (red or blue) of segments, and checks if any segments cross. Note in our input, the segments with the same color are not suppose to cross, so this process must be done before finding actual intersections. This algorithm also breaks up the segments in to smaller pieces if a endpoint of a opposite colored flag lands on it. [Start with the invariant, not the procedure.] This algorithm sweeps through the event points(flags) of the previously sorted list, and keeping track of the same-colored segments intersection with the sweepline with respect to an ordering(aboveness). We add/remove segments when dealing with events with the same color or produce error if we find intersections or have endpoints within another segment of the same color. We also consider if flags of opposite color break our segments. 
3. The second algorithm [name this] finds 4 segments for each flag: the segment directly below of same color(sb), directly above of same color(sa), directly below of opposite color(ob), and directly above of opposite color(oa). It sweeps two lines over all the flags. One sweep line keeps track of the blue segments it crosses with respect to aboveness in a list, and one sweep line keeps track of the red. For each event point, we check in both active [define active before using] segments arrays which segments are sb, sa, ob, oa and we store them in the order we process the flags.
4. The third algorithm [name this] finds all red/blue intersections. [State invariant; of course, to do so, you must ive segments as an array of arrays(more ideal data structure would be a bundle tree). Each bundle will be the same color, and neighboring bundles should be opposite colors. Like before, active segments are kept in order of aboveness. When we process a flag, we read sb, sa, ob, oa, and find them in the active segments. If we find That sb is above oa or ob is above sa, we know that the event point exists in two distinct locations in the active segments, and thus witnessed intsersection(s). We have to reorder the bundles in between so the two distinct locations coincide. We do this by swapping and merging the bundles in between, and every time we swap we report intersections between every segment of the two bundles.

### Completed
Steps 0 to 3 outlined above are finished with all degeneracy handling included. Step 4 is attempted, but not finished. My previous understanding of the algorithm was incorrect, so I hope the code I have now has the correct idea, but even then there is a large number of bugs. A main source of bugs may be I am trying to using arrays instead of better suited data structures like bundle trees. The double indices in particular were hard to deal with. I did not have time to consider boolean operations or clipping as I originally wanted.

### Reflection
The algorithms can be improved in several ways. Most notably, using appropriate data structures will likely make important parts of the algorithm more efficient and easier to read. I have a good understanding of how this algorithm works, and I understand the main advantage of this algorithm: not requiring extra precision to calculate the coordinate of intersections. The most difficult part was step 4 in the outline above. It is an assembly of many smaller algorithms that are hard to verify correctness out of context, and therefore is prone to bugs. 