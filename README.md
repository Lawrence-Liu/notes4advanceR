Notes on [Advanced R](http://adv-r.had.co.nz/)
==============
by Lawrence Liu

### [Introduction](http://adv-r.had.co.nz/Introduction.html)
1. R's biggest challenges: Most R users are not programmers. Which means
 *  Much of the R code is not elegant, fast or easy to understand.
 *  The knowledge of software engineering of R users is patchy. They don't use source code control or automated testing.
 *  Metaprogramming is double-edged sword
 *  Inconsistency is rife across contributed packages, even within base R.
 *  R is not a particularly fast programming language, and poorly written R code can be terribly slow.
 
2. What I can get from this book
 * Be familiar with fundamentals of R
 * Understand what functional programming means and why it is a useful tool for data analysis.
 * Appreciate the double-edged sword of metaprogramming.
 * Have a good intuition for which operations in R are slow or use a lot of memory.
 * Be comfortable reading and understanding the majority of R code.
 
 *I should bear this in mind, and check it often and ask myself if I really learn these things from this book*.

3. Meta-techniques
 * Reading source code
 * adopt a scientific mindset

4. The recommended reading seems to be good resources, I must read them when I am free.


### [Data Structure](http://adv-r.had.co.nz/Data-structures.html)
1. R has no 0-dimensional, or scalar types. The most basic class is **atomic vector**.
2. vectors comes in two flavors: atomic vectors and lists. They have three common properties:
   * Type, `typeof()`, what it is.
   * Length, `legnth()` how many elements it contains.
   * Attributes, `atrributes()`, additional arbitrary metadata.
3. Use `is.atomic(x) || is.list(x)` to test if an object is actually a vector.
4. `c()` is short for combine.
5. R considers `db_var <- c(1, 2, 3)` as double vector, it we want to create integer vector, we should use `int_var <- c(1L, 2L, 3L)`.
6. is.numeric() is a general test for "numberliness" of a vector and returns `TRUE` for both integer and double vectors. 
7. Types from least to most flexible: logical, integer, double, character.
8. You will usually get a warning message if the coercion might lose information. *This is a good habitat when i develop my R package.*
9. `is.recursive()` and `is.atomic()`
10. When combining a vector and a list, the result is quite different between `c()` and `list()`
11. list is actually a kind of vector. If we use `is.vector()` to test it, it will return `TRUE`.
12. We should use `unlist()` to convert a list to an atomic vector. `as.vector` doesn't work.
13. All objects can have arbitrary additional attributes. Attributes can be thought of as a names list(with unique names).
14. By default, most attributes are lost when modifying a vector. The only attributes not lost are the three most important: Names `names()`, Dimensions `dim()` and Class `class()`.
15. **class()** is a special attribute.
16. Factor is a special integer vector, what makes it special is its attributes of class and levels.
17. We can't combine **factors**, because the levels are different. When we combine factors, it's coerced into a integer vector.
18. While factors look (and often behave) like character vectors, they are actually integers. 
19. Some string methods (like `gsub()` and `grepl()`) will coerce factors to strings, while others (like `nchar()`) will throw an error, and still others (like `c()`) will use the underlying integer values. So we should explicitly convert factors to character vectors.
20. Matrices and arrays are atomic vectors with a `dim()` attribute.

### [Subsetting](http://adv-r.had.co.nz/Subsetting.html)
1. **Nothing** returns whole vector. `x[]` returns all elements of x.
2. `outer()` is a very helpful function.
3. You can also subset higher-dimensional data structures with an integer matrix (or, if named, a character matrix)  *I didn't know this before!*
4. We can use **matrix subsetting** and **list subsetting** to subset a data frame. One thing worth noting is that if I select one single column, matrix subsetting simplifies by default, while list subsetting does not.
5. S3 objects are made up of atomic vectors, arrays and lists, so you can always pull apart an S3 object using the techniques described above and the knowledge you gain from `str()`.
6. Three subsetting opertors: `[`, `[[` and `$`. `$` is a useful shorthand for `[[` combined with character subsetting. 
7. `[[` can return only a single value, so we must use it with either a single positive integer and string.
8. If we do supply a vector in `[[`, it indexes recursively.
9. S3 and S4 objects can override the standard behavior of `[` and `[[`.
10. Simplifying subsetting and preserving subsetting are different, and it's very important to understand those differences.
11. The behavior of simplifying subsetting varies slightly between different data types, as described below:
   * **Atomic vector**: remove names.
   * **List**: return the object inside the list, not a single element list.
   * **Factor**: drops any unused levels. By default, `Drop = FALSE`.
   * **Matrix** or **array**: if any of the dimensions has length 1, drops that dimension. By default, `Drop = TRUE`.
   * **Data frame**: If output is a single column, returns a vector instead of a data frame. By default, `Drop = TRUE`.
12. `$` is a shorthand operator, where `x$y` is equivalent to `x[["y", exact = FALSE]]`. So apparently `$` and `[[` differ slightly.
13. All subsetting operators can be combined with assignment to modify selected values of the input vector. Something worth noting:
   * The length of LHS needs to match the RHS
   * There is no checking for duplicated indices.
   * We can't combine integer indices with NA, but we can combine logical indices with NA.
   * If we use logical indices to subset, the length of indices must match the length of the vector, otherwise, R will replicate the logical indices.
14. Subsetting with nothing can be useful in conjunction with assignment because it will preserve the original class and structure.

### [Vocabulary](http://adv-r.had.co.nz/Vocabulary.html)
####
1. `<<-`: this operators is assignment operator, it will search for the varible in LHS through parent environment.
2. Difference between `<-` and `=`: *still unknown*.
3. 

#### Workspace
1. `exists`: test if an R object exists.

#### Help
1. `apropos()` and `find()`: look for objects named in parameter.


#### Files and directories
1. `dir()`, `list.files()`, `list.dirs()`: list the files and directories under a directory.
2. `basename()`, `dirname()`, `tools::file_ext()`:return basement, dirname and extension of a file.
3. `file.path()`: connects different parts of a file path with platform's path separator.
4. `file.exists()`: This series of functions provide a low level interface to the system's file system, and the use of these functions are quite straightforward.
5. `download.file()`: download file from Internet.
6. Package `downloader` makes it possible to download from https links in R.

### [Style](http://adv-r.had.co.nz/Style.html)
1. First of all, no style is better than others, so we should keep a consistent style. And when collabrate with other, we should discuss and decide a common style.
2. If R files need to be run in sequence, prefix them with numbers
3. Place spaces around all infix operators. The same rules applies when using = in function calls. Always put a space after a comma, and never before, except `:`, `::` and `:::`.
4. Place a space before left parentheses, except in a function call.
5. Do not place spaces around code in parentheses or square brackets.
6. An opening curly brace should never go on its own line and should always be followed by a new line. A closing curly brace should always go on its own line, unless it's followed by `else`.
7. It's OK to leave very short statements on the same line.
8. Strive to limit your code to 80 characters per line. If you find yourself running out of room, this is a good indication that you should encapsulate some of the work in a separate function.
9. Each line of a comment should begin with the comment symbol and a single space. **Comments should explain the why, not the what**.  *And why is that?*
10. Use commented lines of `-` and `=` to break up your file into easily readable chunks.

### [Functions](http://adv-r.had.co.nz/Functions.html)
1. The three components of a function are its body, arguments and environment. We can use `body()`, `formals()` and `environment()` to extract the three components.
2. There is one exception to the above rule. Primitive functions, like `sum()`, calls C code directly with `.Primitive()` and contain no R code. Their three components are all `NULL`.
3. Primitive functions are only found in the `base` package. 
4. `is.function` and `is.primitive()` check whether its argument is a (primitive) function. *So every time I name a variable, I can use exist() to check if I already gave another variable the same name*.
5. Scoping is the set of rules that govern how R looks up the value of a symbol.
6. R has two types of scoping: lexical scoping and dynamic scoping.
7. There are four basic principles behind R's implementation of lexical scoping:
   * name masking
   * functions vs. variables
   * a fresh start
   * dynamic lookup
8. If you are using a name in a context where it's obvious that you want a function (e.g. `f(3)`), R will ignore objects that are not functions while it is searching.
9. Every time a function is called, a new environment is created to host execution. 
10. R uses dynamic lookup, R looks for values when the function is run, not when it's created. This means that the output of a function can be different depending on objects outside its environment.
11. However, it's never possible to make a function completely self-contained because you must always rely on functions defined in base R or other packages.
12. All standard operators in R are actually functions.
13. Everything that exists is an object. Everything that happens is a function call.
14. When we use `+` inside sapply, we can use `+` and ``. 
15. The formal arguments are a property of the function, whereas the actual or calling arguments can vary each time you call the function.
16. When calling a function you can specify arguments by position, by complete name, or by partial name. And the priority is exact name, prefix matching, and by position.
17. `do.call()` constructs and executes a function call from a name or a function and a list of arguments to be passed to it.
18. Since arguments in R are evaluated lazily (more on that below), the default value can be defined in terms of other arguments. E.g., `f <- function(a = 1, b = a * 2)`
19. Default arguments can even be defined in terms of variables created within the function. This is used frequently in base R functions. E.g.,  `h <- function(a = 1, b = d)  d <- (a + 1) ^2; c(a, b)`. This is frequently used in base R functions.
20. `missing()`:  used to test whether a value was specified as an argument to a function.
21. By default, R functions are lazy - they're only evaluated if they're actually used.  
22. If you want to ensure that an argument is evaluated you can use `force`.
23. *This is very tricky when using `sapply` or loop to create closures.*
24. Default arguments are evaluated inside the function. This means that if the expression depends on the current environment the results will differ depending on whether you use the default value or explicitly provide one.
25. The argument `...` will match any arguments not otherwise matched. 
26. infix and prefix function, and how to create our own infix function.
27. Some functions can be used to alter the value of an object, they are called  replacement functions. 
28. Replacement functions actually create a modified copy.
29. `pryr:address()` can be used to find the memory address of the underlying object.
30. Built in functions that are implemented using .Primitive will modify in place. *This explains why changing the name of a very large object can be memory-consuming*.
31. Functions can return `invisible` values, which are not printed out by default when you call the function. E.g., the most common function returns invisibly is `<-`.
32. What `library()` does is loading a package and modifying the search path.


### [OO field guide](http://adv-r.had.co.nz/OO-essentials.html)

### [Environments](http://adv-r.had.co.nz/Environments.html)
1. The job of an environment is to associate, or bind, a set of names to a set of values. You can think of an environment as a bag of names. Each name points to an object stored elsewhere in memory.
2. If an object has no names pointing to it, it gets automatically deleted by the garbage collector(`gc`).
3. Every environment has a parent except empty environment. But given an environment, we have no way to find its children.
4. Generally an environment is similar to a list, with four important exceptions:
	* Every object in an environment has a unique name
	* The objects in an environment are not ordered. 
	* An environment has a parent.
	* Environments have reference semantics.
5. Four special environments: `globalenv()`, `baseenv()`, `emptyenv()`, `environment()`.
6. The default parent provided by `new.env()` is environment from which it is called. 
7. The easiest way to modify the bindings in an environment is to treat it like a list.
8. By default, ls() only shows names that don't begin with `.`
9. To compare environments, you must use `identical()` not `==`.
10. The enclosing environment is the environment where the function was created. Every function has one and only one enclosing environment. We can find the enclosing environment of a function by calling `environment()` with a function as its first argument.
11. The enclosing function determines how the function finds values; the binding environments determine how we find  the function.
12. 