# epi-smt

This is a python project that evaluates the potential worst case spread of an epidemic through a social contact network. Given a graph G, timesteps t, probability p, and integers limit and target, this project first simulates how far an epidemic starting at any node in G would spread in t timesteps with p probability of spread per timestep. With this information, it then decides whether there exists a subset of no more than limit nodes that would result in at least target total infections. It does this by reducing the problem to SMT.

I also intended this project to solve a vaccination-based variant of the problem, where we have an integer n_vaccinate and want to know if it's possible to vaccinate no more than n_vaccinate nodes and thereby ensure that there does not exist any subset of limit of fewer nodes that would cause target or more final infections. I intended to solve this problem by reducing it to quantified SMT, but this functionality does not currently work.

Python requirements:

- random

- networkx

- pysmt

C++ requirements:

- gmp3-devel

- swig

- msat (install via pysmt-install --msat)

- cvc4 (install via pysmt-install --cvc4) (slow to install, only used for decide_quantified_epidemic() which doesn't even work, can probably skip this)

This tool provides the decide_epidemic() and decide_quant_epidemic() methods. decide_epidemic() accepts the arguments ’source’, ’size’, ’m’, ’p’, ’limit’, ’target’, ’timesteps’, and ’p_infect’. 
’source’ can either be a path to a NetworkX graph adjlist file for an undirected graph or the string ’generate’. If it’s the string ’generate’, then the tool generate a [Watts-Strogatz](https://en.wikipedia.org/wiki/Watts%E2%80%93Strogatz_model) graph using the provided size, m and p parameters. Otherwise, it ignores size, m and p. 
’limit’ and ’target’ decide the limit for the number of initial infected and the target for the number of final infected, respectively. 
’timesteps’, and ’p_infect’ control how many timesteps the epidemic is simulated to progress for and the probability of infection spread per timestep.

Calls to decide_epidemic() return a tuple containing a boolean indicating the satisfiability of the associated SMT problem and (if the problem is satisfiable) an assignment of variables for the SMT problem. decide_quant_epidemic() has the same arguments as decide_epidemic(), with the addition of ’n_vaccinate’, which determines the number of allowed vaccines. decide_quant_epidemic() returns a tuple containing a boolean indicating the satisfiability of the associated quantified SMT problem and (if the problem is satisfiable) an assignment of variables for the problem. Note that currently decide_quantified_epidemic() incorrectly states that all vaccination problems are unsatisfiable.



Example usage:

```
from epismt import decide_epidemic
answer, model = decide_epidemic(source='generate', size=20, m=4, p=0.1, limit=2, target=15, timesteps=2, p_infect=1)
print('Solution exists:', answer)
print('\nSolution:')
print(model)
```

Output:
```
Solution exists: True

Solution:
final_int_19 := 0
final_int_18 := 0
final_int_17 := 0
final_int_16 := 1
final_int_15 := 0
final_int_14 := 1
final_int_13 := 1
final_int_12 := 1
final_int_11 := 1
final_int_10 := 1
final_int_9 := 1
final_int_8 := 1
final_int_7 := 1
final_int_6 := 1
final_int_5 := 1
final_int_4 := 1
final_int_3 := 1
final_int_2 := 1
final_int_1 := 0
final_int_0 := 1
init_int_19 := 0
init_int_18 := 0
init_int_17 := 0
init_int_16 := 0
init_int_15 := 0
init_int_14 := 0
init_int_13 := 0
init_int_12 := 1
init_int_11 := 0
init_int_10 := 0
init_int_9 := 0
init_int_8 := 0
init_int_7 := 0
init_int_6 := 1
init_int_5 := 0
init_int_4 := 0
init_int_3 := 0
init_int_2 := 0
init_int_1 := 0
init_int_0 := 0
```
