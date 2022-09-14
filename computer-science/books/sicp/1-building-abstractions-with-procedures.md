# Structure and Interpretation of Computer Programs
Harold Abelson and Gerald Jay Sussman with Julie Sussman
Second Edition. Wiley (1996)
Available for [free online here](https://sarabander.github.io/sicp/html/index.xhtml).

## 1 - Building Abstractions with Procedures

### 1.1 - The Elements of Programming

A computational process manipulates abstract data via the rules established by a program.

Every powerful programming language has three features:
(1) Primitive expressions - the simplest entities
(2) Means of combination - the compounding of elements
(3) Means of abstraction - the naming and manipulation of compound elements as units

Data are the entities that one wishes to manipulate. Procedures are the rules for manipulating the data.

#### 1.1.1 Expressions ####

An interpreter evaluates a received expression. Simple arithmetic operations conssit of combinations of operators and operands. The arguments are the values of the operands.

Prefix notation writes the operators to the left of the operands. This is useful for notating expressions with varying numbers of arguments. Additionally, it can be extended and nested to account for more complex combinations. Pretty-printing can be used to present a readable format of expressions using indentation.

An interpreter functions via a read-eval-print loop (and not necessarily print).

#### 1.1.2 Naming and the Environment ####

A identifies the variable and a variable's object is the value. For the interpreter to retrieve external values and implement them inside the expression entails some sort of memory drawn from the environment.

#### 1.1.3 Evaluating Combinations ####

Evaluating a combination consists of two steps:
(1) Evaluate the subexpressions.
(2) Apply the operator to the operands.

This evaluation rule is recursive in nature as it must be invoked inside itself in order to be evaluated.

Tree accumulation is a process (or notation) by which terminal node leafs are evaluate and accumulate upstream their branches toward the root.

Special forms are exceptions to the general evaluation rule. Each special form has its own evaluation rule.

#### 1.1.4 Compound Procedures ####

A procedure definition names a compound operation as a unit. An exponent for example is a compound procedure of multiplication.

A procedure definition takes the general form `(define ({name} {formal parameters}) {body})`.

The name is a symbol associated with the procedure definition. The formal parameters are the names used in the body of the procedure. The body is an expression that will yield the value of the procedure applicaiton. The name and the formal parameters are grouped within parentheses.

#### 1.1.5 The Substitution Model for Procedure Application ####

To apply a compound procedure to arguments, evaluate the body of the procedure with each formal parameter replaced by the corresponding argument.

Normal order evalulation first fully expands an expression before reducing it. This is in contrast to applicative order evaulation that first evaluates the arguments and then applies their operations. Lisp uses applicative-order evaluation to avoid multiple evaluations of expressions.

#### 1.1.6 Conditional Expressions and Predicates ####

A case analysis is a construct that defines a procedure for different cases. In this sense, it is conditional. A conditional expression consists of paired expressions called clauses. A clause's first expression is a predicate whose value can be evaluated as either true or false. An interpreter evaluates a series of clauses until it finds the first clause whose predicate evaluates to true and then it returns the value of its corresponding consequent expressions.

#### 1.1.7 Example: Square Roots by Newton's Method ####

Mathematics is concerned with the declarative. Computer science is concerned with the imperative.

#### 1.1.8 Procedures as Black-Box Abstractions ####

A procedure definition should be able to suppress detail by virtue of its abstraction. A user should not need to know the logic of the procedure in order to implement it themselves. The implementer's choice of names should also not be relevant. An abstract procedure should be indifferent to the naming convention of the implementer.

A formal parameter of a procedure has a special role. The formal parameter's name does not matter, and so its name is a bound variable (i.e. the procedure definition binds its formal parameters). If a variable is not bound, it is free. The set of expressions for which a binding defines a name is called its scope.

The nesting of definitions via block structures is a common method for scoping names and variables. Free variables are passed into an internal definitions where it becomes bound. This is a process called lexical scoping.

### 1.2 - Procedures and the Processes They Generate

A procedure is a pattern for the local evolution of a computational process. It is difficult to do the same for the global behavior of processes but there remain general shapes.

#### 1.2.1 Linear Recursion and Iteration ####

A process that consists of a chain of deferred operations is a recursive process. The interpreter must track the number of operations that must be performed later on. The amount of tracking information grows linearly and is thus a linear recursive process.

An iterative process has a state that can be described by a fixed number of state variables. The numebr of steps grows lineraly in a linear iterative process.

Iterative processes can completely describe the state of the process at any point. The recursive process does not track where the process is.

A recursive process is not a recursive procedure. A recursive procedure refers to itself as an internal procedure. A recursive process is how the process evolves, not the syntax of how it is written.

