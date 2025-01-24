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


### Adding constraints to the model
##### Python Interface
The next step in the example is to add the linear constraints. The first constraint is added here:
```python
# Add constraint: x + 2 y + 3 z <= 4
m.addConstr(x + 2 * y + 3 * z <= 4, "c0")
```
As with variables, constraints are always associated with a specific model. They are created using the Model.addConstr method on the model object.
Overloaded arithmetic operators are used to build linear expressions. 
> Overloaded arthimetic operators meaning
> * used for producing symbolic expression e.g. 2*y -> 2y
> + used to combine symbolic expressions i.e. x +2y + z
The comparison operators are also overloaded to make it easier to build constraints e.g. <= used to create a constraint object instead of evaluating a boolean result 

The second argument to Model.addConstr gives the (optional) constraint name.

Again, this simple example builds the linear expression for the constraint in a single statement using an explicit list of terms. More complex programs will typically build the expression incrementally.

The second constraint is created in a similar manner:
```python
# Add constraint: x + y >= 1
m.addConstr(x + y >= 1, "c1")
```
##### Python matrix Interface
The next step in the example is to add our two linear constraints. This is done by building a sparse matrix that captures the constraint matrix:
```python
# Build (sparse) constraint matrix
val = np.array([1.0, 2.0, 3.0, -1.0, -1.0])
row = np.array([0, 0, 0, 1, 1])
col = np.array([0, 1, 2, 0, 1])

A = sp.csr_matrix((val, (row, col)), shape=(2, 3))
```
The matrix has two rows, one for each constraint, and three columns, one for each variable in our matrix variable.    
The row and col arrays gives the row and column indices for the 5 non-zero values in the sparse matrix, respectively.    
The val array gives the numerical values.    
Note that we multiply the greater-than constraint by -1 to transform it to a less-than constraint (see John Lapinskas linear programming notes for Algos II)

We also capture the right-hand side in a NumPy array:
```python
# Build rhs vector
rhs = np.array([4.0, -1.0])
```
We then use the overloaded @ operator to build a linear matrix expression, and then use the overloaded less-than-or-equal operator to add two constraints (one for each row in the matrix expression) using Model.addConstr:  
```python
# Add constraints
m.addConstr(A @ x <= rhs, name="c")
```


### Setting the objective
#### In the Python interface
The next step in the example is to set the optimization objective:
```python
# Set objective
m.setObjective(x + y + 2 * z, GRB.MAXIMIZE)
```
The objective is built here using overloaded operators and calling Model.setObjective. The Python API overloads the arithmetic operators to allow you to build linear and quadratic expressions involving Gurobi variables.

The second argument indicates that the sense is maximization.  

Note that while this simple example builds the objective in a single statement using an explicit list of terms, more complex programs will typically build it incrementally. For example:
```python
obj = gp.LinExpr()
obj += x
obj += y
obj += 2 * z
m.setObjective(obj, GRB.MAXIMIZE)
```
#### In the Python matrix interface
The next step is to set the optimization objective:
```python
# Set objective
obj = np.array([1.0, 1.0, 2.0])
m.setObjective(obj @ x, GRB.MAXIMIZE)
```
The objective is built here by computing a dot product between a constant vector and our matrix variable using the overloaded @ operator.   
Note that the constant vector must have the same length as our matrix variable.

The second argument indicates that the sense is maximization.


### Optimising the model
Now that the model has been built, the next step is to optimise it:
```python
# Optimize model
m.optimize()
```
This routine performs the optimization and populates several internal model attributes (including the status of the optimization, the solution, etc.).   



