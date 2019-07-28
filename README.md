### MIPS
|Service | System Call Code | Arguments | Result|
|--------|------------------|-----------|-------|
print_int | 1 | $a0 = integer | 
print_float | 2 | $f12 = float | 
print_double | 3 | $f12 = double |
print_string | 4 | $a0 = string |
read_int | 5 | | integer (in $v0)
read_float | 6 | |float (in $f0)
read_double | 7 | |double (in $f0)
read_string | 8 | $a0 = buffer, $a1 = length |
sbrk | 9 | $a0 = amount | address (in $v0)
exit | 10 | |

Register Name | Number | Usage
|-------------|--------|--------|
zero | 0 | Constant 0                   
at | 1 | Reserved for assembler
v0 | 2 | Value (return from function)
v1 | 3 | Value (return from function)
a0 | 4 | Argument 1
a1 | 5 | Argument 2
a2 | 6 | Argument 3
a3 | 7 | Argument 4
t0 | 8 | Temporary (not preserved across call)
t1 | 9 | Temporary (not preserved across call)
t2 | 10 | Temporary (not preserved across call)
t3 | 11 | Temporary (not preserved across call)
t4 | 12 | Temporary (not preserved across call)
t5 | 13 | Temporary (not preserved across call)
t6 | 14 | Temporary (not preserved across call)
t7 | 15 | Temporary (not preserved across call)
s0 | 16 | Saved temporary (preserved across call)
s1 | 17 | Saved temporary (preserved across call)
s2 | 18 | Saved temporary (preserved across call)
s3 | 19 | Saved temporary (preserved across call)
s4 | 20 | Saved temporary (preserved across call)
s5 | 21 | Saved temporary (preserved across call)
s6 | 22 | Saved temporary (preserved across call)
s7 | 23 | Saved temporary (preserved across call)
t8 | 24 | Temporary (not preserved across call)
t9 | 25 | Temporary (not preserved across call)
k0 | 26 | Reserved for OS kernel
k1 | 27 | Reserved for OS kernel
gp | 28 | Pointer to global area
sp | 29 | Stack pointer
fp | 30 | Frame pointer
ra | 31 | Return address (used by function call)

Switch/Case statement
```
1. Create an array of jump targets
2. Load the entry indexed by the variable two bits
3. Jump to that address using the jump register, or jr, instruction
```
Function control flow
```
MIPS uses the jump-and-link instruction jal to call functions.
1. The jal saves the return address (the address of the next instruction) 
in the dedicated register $ra, before jumping to the function.
2. jal is the only MIPS instruction that can access the value of the
program counter, so it can store the return address PC+4 in $ra.
```
#### MIPS assembler directives
```
SPIM supports a subset of the assembler directives provided by the actual MIPS assembler:

.align n	
Align the next datum on a 2n byte boundary.

For example, .align 2 aligns the next value on a word boundary. .align 0 turns off automatic alignment of .half, .word, .float, and .double directives until the next .data or .kdata directive.

.ascii str	Store the string in memory, but do not null-terminate it.
.asciiz str	Store the string in memory and null-terminate it.
.byte b1,..., bn	Store the n values in successive bytes of memory.
.data <addr>	
The following data items should be stored in the data segment. If the optional argument addr is present, the items are stored beginning at address addr.

.double d1,..., dn	
Store the n floating point double precision numbers in successive memory locations.

.extern sym size	
Declare that the datum stored at sym is size bytes large and is a global symbol. This directive enables the assembler to store the datum in a portion of the data segment that is efficiently accessed via register $gp.

.float f1,..., fn	
Store the n floating point single precision numbers in successive memory locations.

.globl sym	
Declare that symbol sym is global and can be referenced from other files.

.half h1,..., hn	Store the n 16-bit quantities in successive memory halfwords.
.kdata <addr>	
The following data items should be stored in the kernel data segment. If the optional argument addr is present, the items are stored beginning at address addr.

.ktext <addr>	
The next items are put in the kernel text segment. In SPIM, these items may only be instructions or words (see the .word directive below). If the optional argument addr is present, the items are stored beginning at address addr.

.space n	
Allocate n bytes of space in the current segment (which must be the data segment in SPIM).

.text <addr>	
The next items are put in the user text segment. In SPIM, these items may only be instructions or words (see the .word directive below). If the optional argument addr is present, the items are stored beginning at address addr.

.word w1,..., wn	
Store the n 32-bit quantities in successive memory words. SPIM does not distinguish various parts of the data segment (.data, .rdata and .sdata).
```

### Three rules for blocks : 
```
Rule 1: The first statement in the program is the leader
Rule 2: Any statement that is the target of a branch statement is a leader
Rule 3: Any statement that immediately follows a branch or return statement is a leader
```
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
#### characteristics that make a good ISA:
```
Data and Memory Operations
Arithmetic and Logic operations
Control Flow operations
Coprocessor Operations (if there is a co processor)
```
### Instruction Costs
| Instruction | Cost |
|-------------|------|
|add r2, r1   | 1 cycle |
|muli c, r1   | 10 cycles |
|load r2, r1  | 3 cycles
|store r2, r1  | 3 cycles |
|movem r2, r1  | 4 cycles |
|movex r3, r2, r1  | 5 cycles |


### Notes
#### Deterministic Finite Automata
```
Deterministic: Machine is in a state. Upon receipt of a symbol will go to a unique state.
Finite: Have a finite number of states
Automata: (pl. of automaton) Self operating machine
```
#### syntatic, semantic
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
