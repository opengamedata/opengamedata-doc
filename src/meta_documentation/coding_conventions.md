# Chapter 2: Coding Conventions

A summary of a few conventions used throughout the various OGD codebases:

A few notes:  

- None of these are really *mandatory* in any sense, but they do help keep things consistent and organized.
- Most of the codebase(s) are written in Python, so most of these conventions are Python-focused.

## Software Platform

We use the software packages and versions listed in our Open Game Data Reference Platform [document](./software_platform.md)

## Casing

We generally use different casing conventions for different parts of the code:

- `PascalCase` : Use for classes, properties, and public functions
- `camelCase` : Use for local and "private" functions. Prefix private functions with an underscore: `_privateFunction`
- `snake_case` : Use for variables, including instance variables (e.g. `self._var`). Prefix private and temp variables with an underscore: `self._private_var` or `_temp_var`

## Alignment

## Quote Characters

## Type Hinting

Python does not have formal type-checking, but provides type "hints" to help a linter pre-check for potential type errors.
In general, we do our best to add type hints wherever practical and cut down on exceptions.  

The three primary cases for type hints are:

1. **Variables**:
When a variable is first used/bound/whatever, we attach a type hint between the variable name and the assignment operator.
    For example:  
    `some_int_var : int = 5`  
    We leave a space before and after the colon.
    For instance variables that are not initialized with a simple assignment (e.g. `self.var : str = "foo"`), simply "declare" the variable, optionally giving it a temporary variable (such as an empty list, for a `List` variable) and initialize later.
    This allows us to keep `__init__` functions relatively well-organized, with a section of one-line declarations/initializations followed by a section of complicated initializations.  
    For example:  

    ```python
    self._simple_var      : int                  = 0
    self._complicated_var : Dict[str, List[int]] = {}
    self._another_var     : str                  = "Some string"
    
    for i in range(20):
        self._complicated_var[f'Item{i}'] = [x*i for x in range(3)]
    ```

2. **Function parameters**:
Function parameters are type-hinted with a similar syntax to variables.
For function params
    <font style="color:tomato">STUB: This section is on the to-do list.</font>

3. **Function return values**:
    <font style="color:tomato">STUB: This section is on the to-do list.</font>

## Organization of Functions in a Class

Generally, the functions of a class are organized into sections, with each section marked by a comment.

The sections go in the following order:

1. **Define Abstract Functions**: This only applies to abstract base classes. Typicallly abstracts will be private functions, called from a public method that handles cross-cutting concerns (e.g. logging)

2. **Implement Built-ins**: At minimum, a constructor; typically includes a `to_string`-style function (e.g. `__str__` and/or `__repr__` in Python) and any operator overloads.

3. **Implement Abstract Functions**: Public abstracts should come first (if any, see note for **Define Abstract Functions**), followed by private abstracts.

4. **Public Statics**: No notes

5. **Public Methods**: No notes

6. **Properties**: Getter and setter functions for languages that support them. For OGD, that's basically just Python.

7. **Private Statics**: No notes

8. **Private Methods**: Generally reserved for helper functions for public methods.
