# Learning Gurobi
 getting started! shoutout [@peter-bread](https://github.com/peter-bread)    
     
> [Following this tutorial](https://support.gurobi.com/hc/en-us/articles/17278438215313-Tutorial-Getting-Started-with-the-Gurobi-Python-API)


### Creating the Gurobi model
A Gurobi model holds a single optimization problem.
It consists of:
- a set of variables
- a set of constraints
- the associated attributes (variable bounds, objective coefficients, variable integrality types, constraint senses, constraint right-hand side values, etc.)

```python
m = gp.Model("mip1")
```
This [gp.Model](https://docs.gurobi.com/projects/optimizer/en/current/reference/python/model.html) takes the desired model name as its argument.

There's a Python Interface and a Python matrix Interface.   
Can combine Python interfaces and the Python matrix interface when working on projects that involve both object-oriented design and numerical computations. 

### Adding variables to the model
##### Python Interface
```python
# Create variables
x = m.addVar(vtype=GRB.BINARY, name="x")
y = m.addVar(vtype=GRB.BINARY, name="y")
z = m.addVar(vtype=GRB.BINARY, name="z")
```
Variables are added through the Model.addVar method on a model object 
Use Model.addVars to add more than one at a time. 
A variable is always associated with a particular model.

##### Python matrix Interface
The next step adds a matrix variable to the model:
```python
# Create variables
x = m.addMVar(shape=3, vtype=GRB.BINARY, name="x")
```
A matrix variable is added through the Model.addMVar method on a model object. 
In this case the matrix variable consists of a 1-D array of 3 binary variables. 





