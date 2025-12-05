# Class 06: R functions
Libby Gilmore (PID: A69047570)

- [Our first (silly) function](#our-first-silly-function)
- [A second function](#a-second-function)
- [A protein generating function](#a-protein-generating-function)

All functions in R have at least 3 things:

- A **name**, we pick this and use it to call our function,
- Input **arguments** (there can be multiple)
- The **body** lines of R code that do the work

### Our first (silly) function

Write a function to add some numbers

``` r
# some functions have parameters that are absolutely required, others you can build defaults into, so they will still work if the user does not specify all the arguments
add <- function(x, y=1) {
  x + y
}
```

Now we can call this function:

``` r
add(10) # uses default argument for y, adds 1 by default
```

    [1] 11

``` r
add(12,7) # sets x and y arguments, and adds them together
```

    [1] 19

``` r
#add(10,10,100) # Error in add(10, 10, 100) : unused argument (100)

add(c(10,10),100) # alternative to get 100 to add to each element of x vector
```

    [1] 110 110

## A second function

Write a function to generate random nucleotide sequences of a user
specified length:

the `sample()` function can be helpful here.

``` r
# will keep giving you 3 random letters, BUT as replace=FALSE, it'll produce an error if length is over 4
sample(c("A", "C", "G", "T"), size=3) 
```

    [1] "C" "A" "G"

``` r
# with replacement on, you can get a longer sampling
sample(c("A", "C", "G", "T"), size=100, replace=TRUE) 
```

      [1] "T" "G" "T" "G" "A" "C" "G" "T" "C" "A" "T" "T" "G" "G" "T" "C" "T" "A"
     [19] "T" "G" "G" "T" "C" "T" "T" "C" "A" "C" "C" "A" "A" "A" "C" "G" "C" "C"
     [37] "A" "A" "A" "T" "C" "T" "A" "A" "G" "A" "A" "C" "A" "G" "G" "C" "T" "T"
     [55] "C" "A" "G" "T" "G" "C" "A" "T" "G" "A" "A" "T" "C" "C" "G" "A" "G" "T"
     [73] "T" "T" "G" "G" "T" "C" "G" "A" "T" "T" "A" "G" "A" "C" "T" "G" "T" "G"
     [91] "A" "C" "G" "C" "C" "G" "A" "G" "C" "T"

I want a 1 element long character vector that looks like “CACAGC” and
not “C” “A” “C” “A” “G” “C”

``` r
# v <- sample(c("A", "C", "G", "T"), size=100, replace=TRUE) 
# paste(v, collapse="")

generate_DNA <- function(size=50){
  nucleotides <- c("A", "C", "G", "T")
  x <- sample(nucleotides, size, replace = TRUE)
  paste(x, collapse="")
}
```

Test it:

``` r
generate_DNA(60)
```

    [1] "GGAAGTCTTAAGCGCTCGTAATATACGGCCGCCTTCTCCCCGGATGGATCCTTGACGGAA"

``` r
# used for flow control
fasta<-FALSE # when TRUE it returns the first clause
if(fasta){
  cat("HELLO You!")
} else {
  cat("No you don't")
  
}
```

    No you don't

Add the ability to return a mutli-element vector or a single element
fasta like vector.

``` r
generate_fasta <- function(size=50, fasta=TRUE){
  nucleotides <- c("A", "C", "G", "T")
  v <- sample(nucleotides, size, replace = TRUE)
  if(fasta){
    return(paste(v, collapse="")) # you can also return without return and just the variable, but for future readability it is better
  } else{
    return(v)
  }
}
```

Test generate_fasta():

``` r
generate_fasta(50, FALSE)
```

     [1] "C" "T" "G" "C" "T" "G" "T" "T" "A" "C" "C" "A" "C" "G" "G" "G" "T" "T" "T"
    [20] "A" "G" "T" "A" "T" "C" "A" "A" "C" "T" "C" "C" "T" "G" "C" "C" "T" "C" "T"
    [39] "A" "C" "A" "A" "T" "C" "A" "G" "A" "T" "T" "G"

``` r
generate_fasta(20, TRUE)
```

    [1] "AACCTAGAATTGGATGGCGT"

## A protein generating function

``` r
# generates a random protein vector
generate_protein <- function(size=50, fasta=TRUE){
  amino_acids <- c("A", "R", "N", "D", "C", "Q", "E", "G", "H", "I", 
                   "L", "K", "M", "F", "P", "S", "T", "W", "Y", "V")
  v <- sample(amino_acids, size, replace = TRUE)
  if(fasta){
    return(paste(v, collapse=""))
  } else{
    return(v)
  }
}
```

``` r
generate_protein(6)
```

    [1] "HSKWVL"

Use our new `generate_protein()` function to make random protein
sequences of length 6 to 12 (i.e. one length 6, one length 7, etc. up to
length 12)

``` r
lengths <- 6:12
lengths
```

    [1]  6  7  8  9 10 11 12

``` r
for (i in lengths) {
  cat(">", i, "\n", sep="")
  aa <- generate_protein(i)
  cat(aa)
  cat("\n")
}
```

    >6
    QTVILE
    >7
    ESEFRQW
    >8
    CVEASKSL
    >9
    VQGEHAICL
    >10
    ANSFHKEQVH
    >11
    QKALFDFMFQC
    >12
    GKKDEVLWMQCR

``` r
# but the `apply` functions are typically more efficient than `for loops`
sapply(lengths, generate_protein)
```

    [1] "HLNHEA"       "RRIAAAC"      "VHNFMFWI"     "ISNGQRCND"    "RQAWQYIEGP"  
    [6] "HEFNLYCPQEY"  "VYPWGWKWNEDD"
