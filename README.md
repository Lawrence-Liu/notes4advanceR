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