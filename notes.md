### Graph coloring
Chaitin's algorithm
```
1 Until there are nodes with degree < k:
 choose such node and push it into the stack;
 delete the node and all its edges from the graph.
2 If the graph is non-empty (and all nodes have degree >= k), then:
 choose a node (using some heuristics) and spill it to the memory;
 delete the node and all its edges from the graph.
 if this results to some nodes with degree < k, then go to the step 1;  
 otherwise continue with the step 2.
3 Successively pop nodes off the stack and color them in the lowest color not used by some neighbor.
```
Optimistic coloring (Briggs et al)
```
If all nodes have a degree >= k, then instead of spilling order the nodes and push them into stack.
 When taking nodes back from the stack they may still be colorable!
```
Chaitin-Briggs's algorithm
```
1 Until there are nodes with degree < k:
 choose such node and push it into the stack;
 delete the node and all its edges from the graph.
2 If the graph is non-empty (and all nodes have degree >= k), then:
 choose a node, push it into the stack, and delete it (together with edges) from the graph;
 if this results to some nodes with degree < k, then go to the step 1;
 otherwise continue with the step 2.
3 Pop a node from the stack and color it by the least free color.
 If the is no free colors, then choose an uncolored node, spill it into the memory, and go to the step 1.
```
Spilling heuristics
```
 Choosing a node for spilling is a critical for efficiency.
 Chaitin's heuristics:
  to minimize the value of cost/degree , where cost is a spilling cost and degree is a current degree of the node;
  ie. choose for spilling a "cheapest" possible node which decreases the degree of most other nodes.
 Alternative popular metrics: cost/degree^2.                                                                                                                                      
 Variations:
  spilling of interference regions;
  partitioning of live ranges;
  rematerialization.
  ```
What is spill cost?
```
 Cost of extra load and store-instructions
```
### Notes
syntatic:
```bash
scaned token matches wiht grammar in parser
```
semantic:
```bash
After syntatic match, check:
1. are variables declared in current scope?
2. are variables compatible for the current operator?
3. is result compatible with left-hand side?
```
### ANTLR
```
# generate parse tree
antlr4 tiger.g4
antjc *.java
grun tiger tigerprogram -gui ../test1.tiger
```

### Download and install the C++ runtime for ANTLR
Go to https://github.com/antlr/antlr4 and click the green Clone or download button. Then click the
Download ZIP button. Save the downloaded zip anrlr4-master.zip in your home directory and unzip it.
You will get a directory antlr4-master. Change to directory antlr4-master/runtime/Cpp. Go to
https://github.com/antlr/antlr4/tree/master/runtime/Cpp and see the instructions under “Compiling on
Linux”. Type the following in the Ubuntu terminal window:
```bash
mkdir build && mkdir run && cd build
cmake .. -DANTLR_JAR_LOCATION=/Users/shxi/omscs/CS8803compiler/Summer_Compilers_Project_1/antlr/antlr-4.7.2-complete.jar -DWITH_DEMO=True
make
DESTDIR=../run make install
```
The last command above will take a while to approach 100% completion.
Enter subdirectory antlr4-master/runtime/Cpp/run/usr/local/include. Type the following command to
copy the directory containing the ANTLR header files to the standard system location:
```bash
sudo cp -r antlr4-runtime /usr/local/include
```
Enter subdirectory antlr4-master/runtime/Cpp/run/usr/local/lib. Type the following commands to
copy the ANTLR libraries to the standard system location and make them available to programs:
```bash
sudo cp * /usr/local/lib
sudo ldconfig (Linux)
sudo update_dyld_shared_cache (OSX)
```
Now you can change to your home directory and delete all of directory antlr4-master:
```bash
rm -r antlr4-mast

```
