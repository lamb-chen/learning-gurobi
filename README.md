# Learning Gurobi
 getting started! shoutout [@peter-bread](https://github.com/peter-bread)    
     
> [Following this tutorial](https://support.gurobi.com/hc/en-us/articles/17278438215313-Tutorial-Getting-Started-with-the-Gurobi-Python-API)


### Creating the Gurobi model
A Gurobi model holds a single optimization problem.
It consists of:
- a set of variables
- a set of constraints
- - the associated attributes (variable bounds, objective coefficients, variable integrality types, constraint senses, constraint right-hand side values, etc.)

```python
m = gp.Model("mip1")
```
This [gp.Model](https://docs.gurobi.com/projects/optimizer/en/current/reference/python/model.html) takes the desired model name as its argument.

