# epi-smt

This is a python project that evaluates the potential worst case spread of an epidemic through a social contact network. Given a graph G, timesteps t, probability p, and integers limit and target, this project first simulates how far an epidemic starting at any node in G would spread in t timesteps with p probability of spread per timestep. With this information, it then decides whether there exists a subset of no more than limit nodes that would result in at least target total infections. It does this by reducing the problem to SMT.

I also intended this project to solve a vaccination-based variant of the problem, where we have an integer n_vaccinate and want to know if it's possible to vaccinate no more than n_vaccinate nodes and thereby ensure that there does not exist any subset of limit of fewer nodes that would cause target or more final infections. I intended to solve this problem by reducing it to quantified SMT, but this functionality does not currently work.

Python requirements:

random

networkx

pysmt

C++ requirements:

gmp3-devel

swig

msat (install via pysmt-install --msat)

bdd (install via pysmt-install --bdd)

This tool provides the decide_epidemic() and decide_quant_epidemic() methods. decide_epidemic() accepts the arguments ’source’, ’size’, ’m’, ’p’, ’limit’, ’target’, ’timesteps’, and ’p_infect’. 
’source’ can either be a path to a NetworkX graph adjlist file for an undirected graph or the string ’generate’. If it’s the string ’generate’, then the tool generate a Watts-Strogatz graph using the provided size, m and p parameters. Otherwise, it ignores size, m and p. 
’limit’ and ’target’ decide the limit for the number of initial infected and the target for the number of final infected, respectively. 
’timesteps’, and ’p_infect’ control how many timesteps the epidemic is simulated to progress for and the probability of infection spread per timestep.

Calls to decide_epidemic() return a tuple containing a boolean indicating the satisfiability of the associated SMT problem and (if the problem is satisfiable) an assignment of variables for the SMT problem. decide_quant_epidemic() has the same arguments as decide_epidemic(), with the addition of ’n_vaccinate’, which determines the number of allowed vaccines. decide_quant_epidemic() returns a tuple containing a boolean indicating the satisfiability of the associated quantified SMT problem and (if the problem is satisfiable) an assignment of variables for the problem.
