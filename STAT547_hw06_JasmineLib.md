STAT547\_hw06\_JasmineLib
================

Jasmine's STAT 547 Homework 06
==============================

Sections tackled:

Option 1 Working with Character Data - Completed the Exercises from Strings chapter in R for Data Science

-   provide solutions to the exercises, to serve as an example and reference point for future work and assignments.
-   create relevant "test vectors" and examples to improve understanding.

Option 2 Writing functions:

-   worked through the exercise on a linear regression function customized for gapminder data ()
-   as part of this assignment, made a function to perform a quadratic regression on gapminder data.
-   also made functions for simple visualization of regression line data on a basic ggplot.

``` r
library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.7
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ──────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(stringr)
library(dplyr)
library(stringi)
```

Part 1: Exercises in R for Data Science Strings Chapter:
--------------------------------------------------------

here I work through all the exercises from the Strings Chapter. in the [R for Data Science Textbook](https://r4ds.had.co.nz/strings.html).

### 14.2 String basics

What is difference between paste() and paste0() and what is the equivalent stringr function?

How do they differ in their handling of NA?
- using paste or paste0 , the NA will get coerced into a character, then pasted.
- using str\_c, if there is an NA in either vector, it will return NA.

``` r
test_vector1 = c("A", "B", NA, "C")
test_vector2 = c("D", "E", "F", NA)

#paste() will concatenate vectors using sep = " "
paste(test_vector1, test_vector2)
```

    ## [1] "A D"  "B E"  "NA F" "C NA"

``` r
#paste0() will concatenate vectors without any separation
paste0(test_vector1, test_vector2)
```

    ## [1] "AD"  "BE"  "NAF" "CNA"

``` r
#str_c is the equivalent stringr function. Here we specify the separation using sep. 
str_c(test_vector1, test_vector2, sep = " ")
```

    ## [1] "A D" "B E" NA    NA

In your own words, describe the difference between the sep and collapse arguments to str\_c().

``` r
# using collapse will combine ALL entries within a vector, separated by ", " in this case. Using Sep will separate the two components being combined. 
str_c(letters, LETTERS, collapse = ", " )
```

    ## [1] "aA, bB, cC, dD, eE, fF, gG, hH, iI, jJ, kK, lL, mM, nN, oO, pP, qQ, rR, sS, tT, uU, vV, wW, xX, yY, zZ"

``` r
str_c(letters, LETTERS, sep = ", " )
```

    ##  [1] "a, A" "b, B" "c, C" "d, D" "e, E" "f, F" "g, G" "h, H" "i, I" "j, J"
    ## [11] "k, K" "l, L" "m, M" "n, N" "o, O" "p, P" "q, Q" "r, R" "s, S" "t, T"
    ## [21] "u, U" "v, V" "w, W" "x, X" "y, Y" "z, Z"

Use str\_length() and str\_sub() to extract the middle character from a string.

``` r
#check length of a string:
str_length("teststring")
```

    ## [1] 10

``` r
#for even numbers I chose to return a substring composed of the 5th and 6th (middle characters) of the string
str_sub("teststring", 5,6)
```

    ## [1] "st"

``` r
#what does str_wrap( ) do?
#str wrap helps "wrap" strings into paragraphs, where width is how many characters can fit on one line, and indent/exdent determines the indentations within the paragraph. this would be useful for times when you want to display paragraphs.

str_wrap("teststring, string, testing", width = 10, indent = 6, exdent = 5)
```

    ## [1] "      teststring,\n     string,\n     testing"

``` r
#what does str_trim() do? 

#removes whitespace at start and end of string.
#opposite of str_trim is str_pad()
str_trim("     teststring, string, testing     ")
```

    ## [1] "teststring, string, testing"

Write a function that turns (e.g.) a vector c("a", "b", "c") into the string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2.

``` r
test_vector_3 = c("1", "2")

vector_to_string = function(x) {
  to_return = "vector input invalid"
  if (length(x) == 0) to_return
  else if (length(x) == 1) to_return = str_c(x[1])
  else if (length(x) == 2) to_return = str_c(x[1], ", ", x[2])
  to_return
}
  

vector_to_string(test_vector_3)
```

    ## [1] "1, 2"

14.3 Matching patterns with regular expressions
-----------------------------------------------

Explain why each of these strings don’t match a : "", "\\", "\\".
-  corresponds to a  used for an escape symbol also used in strings.
- \\ is used for escaping a "." character in regular expressions.
- \\ is not used. to escape a  symbol, we will need: \\\\

``` r
#How would you match the sequence "'\?
#I would match the sequence using: 

j = "\"'\\" #need to add extra \ to the sequence due to needing to "escape" some characters.
str_view(j, "\\\"'\\\\")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-8194ea24fe28cf404e0c">{"x":{"html":"<ul>\n  <li><span class='match'>\"'\\<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#What patterns will the regular expression \..\..\.. match? How would you represent it as a string?

#will match expressions starting with a dot then any character three times.
str_view(c(".d.e.f", "......", ".e.e.ddee"), c("\\..\\..\\.."))
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-07c3217855dabe1c72cf">{"x":{"html":"<ul>\n  <li><span class='match'>.d.e.f<\/span><\/li>\n  <li><span class='match'>......<\/span><\/li>\n  <li><span class='match'>.e.e.d<\/span>dee<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
14.3.2 Anchors
--------------

``` r
# How would you match the literal string "$^$"?
x = "$^$"
str_view(x, "\\$\\^\\$")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-8f9229aed7f2f1dee2e0">{"x":{"html":"<ul>\n  <li><span class='match'>$^$<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Given the corpus of common words in stringr::words, create regular expressions that find all words that:

#Start with “y”.
str_view(words, "^y", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-7d7534ff434220ca8034">{"x":{"html":"<ul>\n  <li><span class='match'>y<\/span>ear<\/li>\n  <li><span class='match'>y<\/span>es<\/li>\n  <li><span class='match'>y<\/span>esterday<\/li>\n  <li><span class='match'>y<\/span>et<\/li>\n  <li><span class='match'>y<\/span>ou<\/li>\n  <li><span class='match'>y<\/span>oung<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#End with “x”
str_view(words, "x$", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-33e367c1dc7e66de64a1">{"x":{"html":"<ul>\n  <li>bo<span class='match'>x<\/span><\/li>\n  <li>se<span class='match'>x<\/span><\/li>\n  <li>si<span class='match'>x<\/span><\/li>\n  <li>ta<span class='match'>x<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Are exactly three letters long. (Don’t cheat by using str_length()!)
str_view(words, "^...$", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-fc592c1c42e1cbfa6b78">{"x":{"html":"<ul>\n  <li><span class='match'>act<\/span><\/li>\n  <li><span class='match'>add<\/span><\/li>\n  <li><span class='match'>age<\/span><\/li>\n  <li><span class='match'>ago<\/span><\/li>\n  <li><span class='match'>air<\/span><\/li>\n  <li><span class='match'>all<\/span><\/li>\n  <li><span class='match'>and<\/span><\/li>\n  <li><span class='match'>any<\/span><\/li>\n  <li><span class='match'>arm<\/span><\/li>\n  <li><span class='match'>art<\/span><\/li>\n  <li><span class='match'>ask<\/span><\/li>\n  <li><span class='match'>bad<\/span><\/li>\n  <li><span class='match'>bag<\/span><\/li>\n  <li><span class='match'>bar<\/span><\/li>\n  <li><span class='match'>bed<\/span><\/li>\n  <li><span class='match'>bet<\/span><\/li>\n  <li><span class='match'>big<\/span><\/li>\n  <li><span class='match'>bit<\/span><\/li>\n  <li><span class='match'>box<\/span><\/li>\n  <li><span class='match'>boy<\/span><\/li>\n  <li><span class='match'>bus<\/span><\/li>\n  <li><span class='match'>but<\/span><\/li>\n  <li><span class='match'>buy<\/span><\/li>\n  <li><span class='match'>can<\/span><\/li>\n  <li><span class='match'>car<\/span><\/li>\n  <li><span class='match'>cat<\/span><\/li>\n  <li><span class='match'>cup<\/span><\/li>\n  <li><span class='match'>cut<\/span><\/li>\n  <li><span class='match'>dad<\/span><\/li>\n  <li><span class='match'>day<\/span><\/li>\n  <li><span class='match'>die<\/span><\/li>\n  <li><span class='match'>dog<\/span><\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>due<\/span><\/li>\n  <li><span class='match'>eat<\/span><\/li>\n  <li><span class='match'>egg<\/span><\/li>\n  <li><span class='match'>end<\/span><\/li>\n  <li><span class='match'>eye<\/span><\/li>\n  <li><span class='match'>far<\/span><\/li>\n  <li><span class='match'>few<\/span><\/li>\n  <li><span class='match'>fit<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>for<\/span><\/li>\n  <li><span class='match'>fun<\/span><\/li>\n  <li><span class='match'>gas<\/span><\/li>\n  <li><span class='match'>get<\/span><\/li>\n  <li><span class='match'>god<\/span><\/li>\n  <li><span class='match'>guy<\/span><\/li>\n  <li><span class='match'>hit<\/span><\/li>\n  <li><span class='match'>hot<\/span><\/li>\n  <li><span class='match'>how<\/span><\/li>\n  <li><span class='match'>job<\/span><\/li>\n  <li><span class='match'>key<\/span><\/li>\n  <li><span class='match'>kid<\/span><\/li>\n  <li><span class='match'>lad<\/span><\/li>\n  <li><span class='match'>law<\/span><\/li>\n  <li><span class='match'>lay<\/span><\/li>\n  <li><span class='match'>leg<\/span><\/li>\n  <li><span class='match'>let<\/span><\/li>\n  <li><span class='match'>lie<\/span><\/li>\n  <li><span class='match'>lot<\/span><\/li>\n  <li><span class='match'>low<\/span><\/li>\n  <li><span class='match'>man<\/span><\/li>\n  <li><span class='match'>may<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>new<\/span><\/li>\n  <li><span class='match'>non<\/span><\/li>\n  <li><span class='match'>not<\/span><\/li>\n  <li><span class='match'>now<\/span><\/li>\n  <li><span class='match'>odd<\/span><\/li>\n  <li><span class='match'>off<\/span><\/li>\n  <li><span class='match'>old<\/span><\/li>\n  <li><span class='match'>one<\/span><\/li>\n  <li><span class='match'>out<\/span><\/li>\n  <li><span class='match'>own<\/span><\/li>\n  <li><span class='match'>pay<\/span><\/li>\n  <li><span class='match'>per<\/span><\/li>\n  <li><span class='match'>put<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n  <li><span class='match'>rid<\/span><\/li>\n  <li><span class='match'>run<\/span><\/li>\n  <li><span class='match'>say<\/span><\/li>\n  <li><span class='match'>see<\/span><\/li>\n  <li><span class='match'>set<\/span><\/li>\n  <li><span class='match'>sex<\/span><\/li>\n  <li><span class='match'>she<\/span><\/li>\n  <li><span class='match'>sir<\/span><\/li>\n  <li><span class='match'>sit<\/span><\/li>\n  <li><span class='match'>six<\/span><\/li>\n  <li><span class='match'>son<\/span><\/li>\n  <li><span class='match'>sun<\/span><\/li>\n  <li><span class='match'>tax<\/span><\/li>\n  <li><span class='match'>tea<\/span><\/li>\n  <li><span class='match'>ten<\/span><\/li>\n  <li><span class='match'>the<\/span><\/li>\n  <li><span class='match'>tie<\/span><\/li>\n  <li><span class='match'>too<\/span><\/li>\n  <li><span class='match'>top<\/span><\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>two<\/span><\/li>\n  <li><span class='match'>use<\/span><\/li>\n  <li><span class='match'>war<\/span><\/li>\n  <li><span class='match'>way<\/span><\/li>\n  <li><span class='match'>wee<\/span><\/li>\n  <li><span class='match'>who<\/span><\/li>\n  <li><span class='match'>why<\/span><\/li>\n  <li><span class='match'>win<\/span><\/li>\n  <li><span class='match'>yes<\/span><\/li>\n  <li><span class='match'>yet<\/span><\/li>\n  <li><span class='match'>you<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Have seven letters or more.
str_view(words, "^.......", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-482f731beb9ad2a111dc">{"x":{"html":"<ul>\n  <li><span class='match'>absolut<\/span>e<\/li>\n  <li><span class='match'>account<\/span><\/li>\n  <li><span class='match'>achieve<\/span><\/li>\n  <li><span class='match'>address<\/span><\/li>\n  <li><span class='match'>adverti<\/span>se<\/li>\n  <li><span class='match'>afterno<\/span>on<\/li>\n  <li><span class='match'>against<\/span><\/li>\n  <li><span class='match'>already<\/span><\/li>\n  <li><span class='match'>alright<\/span><\/li>\n  <li><span class='match'>althoug<\/span>h<\/li>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>another<\/span><\/li>\n  <li><span class='match'>apparen<\/span>t<\/li>\n  <li><span class='match'>appoint<\/span><\/li>\n  <li><span class='match'>approac<\/span>h<\/li>\n  <li><span class='match'>appropr<\/span>iate<\/li>\n  <li><span class='match'>arrange<\/span><\/li>\n  <li><span class='match'>associa<\/span>te<\/li>\n  <li><span class='match'>authori<\/span>ty<\/li>\n  <li><span class='match'>availab<\/span>le<\/li>\n  <li><span class='match'>balance<\/span><\/li>\n  <li><span class='match'>because<\/span><\/li>\n  <li><span class='match'>believe<\/span><\/li>\n  <li><span class='match'>benefit<\/span><\/li>\n  <li><span class='match'>between<\/span><\/li>\n  <li><span class='match'>brillia<\/span>nt<\/li>\n  <li><span class='match'>britain<\/span><\/li>\n  <li><span class='match'>brother<\/span><\/li>\n  <li><span class='match'>busines<\/span>s<\/li>\n  <li><span class='match'>certain<\/span><\/li>\n  <li><span class='match'>chairma<\/span>n<\/li>\n  <li><span class='match'>charact<\/span>er<\/li>\n  <li><span class='match'>Christm<\/span>as<\/li>\n  <li><span class='match'>colleag<\/span>ue<\/li>\n  <li><span class='match'>collect<\/span><\/li>\n  <li><span class='match'>college<\/span><\/li>\n  <li><span class='match'>comment<\/span><\/li>\n  <li><span class='match'>committ<\/span>ee<\/li>\n  <li><span class='match'>communi<\/span>ty<\/li>\n  <li><span class='match'>company<\/span><\/li>\n  <li><span class='match'>compare<\/span><\/li>\n  <li><span class='match'>complet<\/span>e<\/li>\n  <li><span class='match'>compute<\/span><\/li>\n  <li><span class='match'>concern<\/span><\/li>\n  <li><span class='match'>conditi<\/span>on<\/li>\n  <li><span class='match'>conside<\/span>r<\/li>\n  <li><span class='match'>consult<\/span><\/li>\n  <li><span class='match'>contact<\/span><\/li>\n  <li><span class='match'>continu<\/span>e<\/li>\n  <li><span class='match'>contrac<\/span>t<\/li>\n  <li><span class='match'>control<\/span><\/li>\n  <li><span class='match'>convers<\/span>e<\/li>\n  <li><span class='match'>correct<\/span><\/li>\n  <li><span class='match'>council<\/span><\/li>\n  <li><span class='match'>country<\/span><\/li>\n  <li><span class='match'>current<\/span><\/li>\n  <li><span class='match'>decisio<\/span>n<\/li>\n  <li><span class='match'>definit<\/span>e<\/li>\n  <li><span class='match'>departm<\/span>ent<\/li>\n  <li><span class='match'>describ<\/span>e<\/li>\n  <li><span class='match'>develop<\/span><\/li>\n  <li><span class='match'>differe<\/span>nce<\/li>\n  <li><span class='match'>difficu<\/span>lt<\/li>\n  <li><span class='match'>discuss<\/span><\/li>\n  <li><span class='match'>distric<\/span>t<\/li>\n  <li><span class='match'>documen<\/span>t<\/li>\n  <li><span class='match'>economy<\/span><\/li>\n  <li><span class='match'>educate<\/span><\/li>\n  <li><span class='match'>electri<\/span>c<\/li>\n  <li><span class='match'>encoura<\/span>ge<\/li>\n  <li><span class='match'>english<\/span><\/li>\n  <li><span class='match'>environ<\/span>ment<\/li>\n  <li><span class='match'>especia<\/span>l<\/li>\n  <li><span class='match'>evening<\/span><\/li>\n  <li><span class='match'>evidenc<\/span>e<\/li>\n  <li><span class='match'>example<\/span><\/li>\n  <li><span class='match'>exercis<\/span>e<\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experie<\/span>nce<\/li>\n  <li><span class='match'>explain<\/span><\/li>\n  <li><span class='match'>express<\/span><\/li>\n  <li><span class='match'>finance<\/span><\/li>\n  <li><span class='match'>fortune<\/span><\/li>\n  <li><span class='match'>forward<\/span><\/li>\n  <li><span class='match'>functio<\/span>n<\/li>\n  <li><span class='match'>further<\/span><\/li>\n  <li><span class='match'>general<\/span><\/li>\n  <li><span class='match'>germany<\/span><\/li>\n  <li><span class='match'>goodbye<\/span><\/li>\n  <li><span class='match'>history<\/span><\/li>\n  <li><span class='match'>holiday<\/span><\/li>\n  <li><span class='match'>hospita<\/span>l<\/li>\n  <li><span class='match'>however<\/span><\/li>\n  <li><span class='match'>hundred<\/span><\/li>\n  <li><span class='match'>husband<\/span><\/li>\n  <li><span class='match'>identif<\/span>y<\/li>\n  <li><span class='match'>imagine<\/span><\/li>\n  <li><span class='match'>importa<\/span>nt<\/li>\n  <li><span class='match'>improve<\/span><\/li>\n  <li><span class='match'>include<\/span><\/li>\n  <li><span class='match'>increas<\/span>e<\/li>\n  <li><span class='match'>individ<\/span>ual<\/li>\n  <li><span class='match'>industr<\/span>y<\/li>\n  <li><span class='match'>instead<\/span><\/li>\n  <li><span class='match'>interes<\/span>t<\/li>\n  <li><span class='match'>introdu<\/span>ce<\/li>\n  <li><span class='match'>involve<\/span><\/li>\n  <li><span class='match'>kitchen<\/span><\/li>\n  <li><span class='match'>languag<\/span>e<\/li>\n  <li><span class='match'>machine<\/span><\/li>\n  <li><span class='match'>meaning<\/span><\/li>\n  <li><span class='match'>measure<\/span><\/li>\n  <li><span class='match'>mention<\/span><\/li>\n  <li><span class='match'>million<\/span><\/li>\n  <li><span class='match'>ministe<\/span>r<\/li>\n  <li><span class='match'>morning<\/span><\/li>\n  <li><span class='match'>necessa<\/span>ry<\/li>\n  <li><span class='match'>obvious<\/span><\/li>\n  <li><span class='match'>occasio<\/span>n<\/li>\n  <li><span class='match'>operate<\/span><\/li>\n  <li><span class='match'>opportu<\/span>nity<\/li>\n  <li><span class='match'>organiz<\/span>e<\/li>\n  <li><span class='match'>origina<\/span>l<\/li>\n  <li><span class='match'>otherwi<\/span>se<\/li>\n  <li><span class='match'>paragra<\/span>ph<\/li>\n  <li><span class='match'>particu<\/span>lar<\/li>\n  <li><span class='match'>pension<\/span><\/li>\n  <li><span class='match'>percent<\/span><\/li>\n  <li><span class='match'>perfect<\/span><\/li>\n  <li><span class='match'>perhaps<\/span><\/li>\n  <li><span class='match'>photogr<\/span>aph<\/li>\n  <li><span class='match'>picture<\/span><\/li>\n  <li><span class='match'>politic<\/span><\/li>\n  <li><span class='match'>positio<\/span>n<\/li>\n  <li><span class='match'>positiv<\/span>e<\/li>\n  <li><span class='match'>possibl<\/span>e<\/li>\n  <li><span class='match'>practis<\/span>e<\/li>\n  <li><span class='match'>prepare<\/span><\/li>\n  <li><span class='match'>present<\/span><\/li>\n  <li><span class='match'>pressur<\/span>e<\/li>\n  <li><span class='match'>presume<\/span><\/li>\n  <li><span class='match'>previou<\/span>s<\/li>\n  <li><span class='match'>private<\/span><\/li>\n  <li><span class='match'>probabl<\/span>e<\/li>\n  <li><span class='match'>problem<\/span><\/li>\n  <li><span class='match'>proceed<\/span><\/li>\n  <li><span class='match'>process<\/span><\/li>\n  <li><span class='match'>produce<\/span><\/li>\n  <li><span class='match'>product<\/span><\/li>\n  <li><span class='match'>program<\/span>me<\/li>\n  <li><span class='match'>project<\/span><\/li>\n  <li><span class='match'>propose<\/span><\/li>\n  <li><span class='match'>protect<\/span><\/li>\n  <li><span class='match'>provide<\/span><\/li>\n  <li><span class='match'>purpose<\/span><\/li>\n  <li><span class='match'>quality<\/span><\/li>\n  <li><span class='match'>quarter<\/span><\/li>\n  <li><span class='match'>questio<\/span>n<\/li>\n  <li><span class='match'>realise<\/span><\/li>\n  <li><span class='match'>receive<\/span><\/li>\n  <li><span class='match'>recogni<\/span>ze<\/li>\n  <li><span class='match'>recomme<\/span>nd<\/li>\n  <li><span class='match'>relatio<\/span>n<\/li>\n  <li><span class='match'>remembe<\/span>r<\/li>\n  <li><span class='match'>represe<\/span>nt<\/li>\n  <li><span class='match'>require<\/span><\/li>\n  <li><span class='match'>researc<\/span>h<\/li>\n  <li><span class='match'>resourc<\/span>e<\/li>\n  <li><span class='match'>respect<\/span><\/li>\n  <li><span class='match'>respons<\/span>ible<\/li>\n  <li><span class='match'>saturda<\/span>y<\/li>\n  <li><span class='match'>science<\/span><\/li>\n  <li><span class='match'>scotlan<\/span>d<\/li>\n  <li><span class='match'>secreta<\/span>ry<\/li>\n  <li><span class='match'>section<\/span><\/li>\n  <li><span class='match'>separat<\/span>e<\/li>\n  <li><span class='match'>serious<\/span><\/li>\n  <li><span class='match'>service<\/span><\/li>\n  <li><span class='match'>similar<\/span><\/li>\n  <li><span class='match'>situate<\/span><\/li>\n  <li><span class='match'>society<\/span><\/li>\n  <li><span class='match'>special<\/span><\/li>\n  <li><span class='match'>specifi<\/span>c<\/li>\n  <li><span class='match'>standar<\/span>d<\/li>\n  <li><span class='match'>station<\/span><\/li>\n  <li><span class='match'>straigh<\/span>t<\/li>\n  <li><span class='match'>strateg<\/span>y<\/li>\n  <li><span class='match'>structu<\/span>re<\/li>\n  <li><span class='match'>student<\/span><\/li>\n  <li><span class='match'>subject<\/span><\/li>\n  <li><span class='match'>succeed<\/span><\/li>\n  <li><span class='match'>suggest<\/span><\/li>\n  <li><span class='match'>support<\/span><\/li>\n  <li><span class='match'>suppose<\/span><\/li>\n  <li><span class='match'>surpris<\/span>e<\/li>\n  <li><span class='match'>telepho<\/span>ne<\/li>\n  <li><span class='match'>televis<\/span>ion<\/li>\n  <li><span class='match'>terribl<\/span>e<\/li>\n  <li><span class='match'>therefo<\/span>re<\/li>\n  <li><span class='match'>thirtee<\/span>n<\/li>\n  <li><span class='match'>thousan<\/span>d<\/li>\n  <li><span class='match'>through<\/span><\/li>\n  <li><span class='match'>thursda<\/span>y<\/li>\n  <li><span class='match'>togethe<\/span>r<\/li>\n  <li><span class='match'>tomorro<\/span>w<\/li>\n  <li><span class='match'>tonight<\/span><\/li>\n  <li><span class='match'>traffic<\/span><\/li>\n  <li><span class='match'>transpo<\/span>rt<\/li>\n  <li><span class='match'>trouble<\/span><\/li>\n  <li><span class='match'>tuesday<\/span><\/li>\n  <li><span class='match'>underst<\/span>and<\/li>\n  <li><span class='match'>univers<\/span>ity<\/li>\n  <li><span class='match'>various<\/span><\/li>\n  <li><span class='match'>village<\/span><\/li>\n  <li><span class='match'>wednesd<\/span>ay<\/li>\n  <li><span class='match'>welcome<\/span><\/li>\n  <li><span class='match'>whether<\/span><\/li>\n  <li><span class='match'>without<\/span><\/li>\n  <li><span class='match'>yesterd<\/span>ay<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
14.3.3 Character classes and alternatives
-----------------------------------------

``` r
# Create Regular expressions to find all words that:
  #start with a vowel: 
str_view(words, "^[aeiou]", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-3e2e55db7566bca038f4">{"x":{"html":"<ul>\n  <li><span class='match'>a<\/span><\/li>\n  <li><span class='match'>a<\/span>ble<\/li>\n  <li><span class='match'>a<\/span>bout<\/li>\n  <li><span class='match'>a<\/span>bsolute<\/li>\n  <li><span class='match'>a<\/span>ccept<\/li>\n  <li><span class='match'>a<\/span>ccount<\/li>\n  <li><span class='match'>a<\/span>chieve<\/li>\n  <li><span class='match'>a<\/span>cross<\/li>\n  <li><span class='match'>a<\/span>ct<\/li>\n  <li><span class='match'>a<\/span>ctive<\/li>\n  <li><span class='match'>a<\/span>ctual<\/li>\n  <li><span class='match'>a<\/span>dd<\/li>\n  <li><span class='match'>a<\/span>ddress<\/li>\n  <li><span class='match'>a<\/span>dmit<\/li>\n  <li><span class='match'>a<\/span>dvertise<\/li>\n  <li><span class='match'>a<\/span>ffect<\/li>\n  <li><span class='match'>a<\/span>fford<\/li>\n  <li><span class='match'>a<\/span>fter<\/li>\n  <li><span class='match'>a<\/span>fternoon<\/li>\n  <li><span class='match'>a<\/span>gain<\/li>\n  <li><span class='match'>a<\/span>gainst<\/li>\n  <li><span class='match'>a<\/span>ge<\/li>\n  <li><span class='match'>a<\/span>gent<\/li>\n  <li><span class='match'>a<\/span>go<\/li>\n  <li><span class='match'>a<\/span>gree<\/li>\n  <li><span class='match'>a<\/span>ir<\/li>\n  <li><span class='match'>a<\/span>ll<\/li>\n  <li><span class='match'>a<\/span>llow<\/li>\n  <li><span class='match'>a<\/span>lmost<\/li>\n  <li><span class='match'>a<\/span>long<\/li>\n  <li><span class='match'>a<\/span>lready<\/li>\n  <li><span class='match'>a<\/span>lright<\/li>\n  <li><span class='match'>a<\/span>lso<\/li>\n  <li><span class='match'>a<\/span>lthough<\/li>\n  <li><span class='match'>a<\/span>lways<\/li>\n  <li><span class='match'>a<\/span>merica<\/li>\n  <li><span class='match'>a<\/span>mount<\/li>\n  <li><span class='match'>a<\/span>nd<\/li>\n  <li><span class='match'>a<\/span>nother<\/li>\n  <li><span class='match'>a<\/span>nswer<\/li>\n  <li><span class='match'>a<\/span>ny<\/li>\n  <li><span class='match'>a<\/span>part<\/li>\n  <li><span class='match'>a<\/span>pparent<\/li>\n  <li><span class='match'>a<\/span>ppear<\/li>\n  <li><span class='match'>a<\/span>pply<\/li>\n  <li><span class='match'>a<\/span>ppoint<\/li>\n  <li><span class='match'>a<\/span>pproach<\/li>\n  <li><span class='match'>a<\/span>ppropriate<\/li>\n  <li><span class='match'>a<\/span>rea<\/li>\n  <li><span class='match'>a<\/span>rgue<\/li>\n  <li><span class='match'>a<\/span>rm<\/li>\n  <li><span class='match'>a<\/span>round<\/li>\n  <li><span class='match'>a<\/span>rrange<\/li>\n  <li><span class='match'>a<\/span>rt<\/li>\n  <li><span class='match'>a<\/span>s<\/li>\n  <li><span class='match'>a<\/span>sk<\/li>\n  <li><span class='match'>a<\/span>ssociate<\/li>\n  <li><span class='match'>a<\/span>ssume<\/li>\n  <li><span class='match'>a<\/span>t<\/li>\n  <li><span class='match'>a<\/span>ttend<\/li>\n  <li><span class='match'>a<\/span>uthority<\/li>\n  <li><span class='match'>a<\/span>vailable<\/li>\n  <li><span class='match'>a<\/span>ware<\/li>\n  <li><span class='match'>a<\/span>way<\/li>\n  <li><span class='match'>a<\/span>wful<\/li>\n  <li><span class='match'>e<\/span>ach<\/li>\n  <li><span class='match'>e<\/span>arly<\/li>\n  <li><span class='match'>e<\/span>ast<\/li>\n  <li><span class='match'>e<\/span>asy<\/li>\n  <li><span class='match'>e<\/span>at<\/li>\n  <li><span class='match'>e<\/span>conomy<\/li>\n  <li><span class='match'>e<\/span>ducate<\/li>\n  <li><span class='match'>e<\/span>ffect<\/li>\n  <li><span class='match'>e<\/span>gg<\/li>\n  <li><span class='match'>e<\/span>ight<\/li>\n  <li><span class='match'>e<\/span>ither<\/li>\n  <li><span class='match'>e<\/span>lect<\/li>\n  <li><span class='match'>e<\/span>lectric<\/li>\n  <li><span class='match'>e<\/span>leven<\/li>\n  <li><span class='match'>e<\/span>lse<\/li>\n  <li><span class='match'>e<\/span>mploy<\/li>\n  <li><span class='match'>e<\/span>ncourage<\/li>\n  <li><span class='match'>e<\/span>nd<\/li>\n  <li><span class='match'>e<\/span>ngine<\/li>\n  <li><span class='match'>e<\/span>nglish<\/li>\n  <li><span class='match'>e<\/span>njoy<\/li>\n  <li><span class='match'>e<\/span>nough<\/li>\n  <li><span class='match'>e<\/span>nter<\/li>\n  <li><span class='match'>e<\/span>nvironment<\/li>\n  <li><span class='match'>e<\/span>qual<\/li>\n  <li><span class='match'>e<\/span>special<\/li>\n  <li><span class='match'>e<\/span>urope<\/li>\n  <li><span class='match'>e<\/span>ven<\/li>\n  <li><span class='match'>e<\/span>vening<\/li>\n  <li><span class='match'>e<\/span>ver<\/li>\n  <li><span class='match'>e<\/span>very<\/li>\n  <li><span class='match'>e<\/span>vidence<\/li>\n  <li><span class='match'>e<\/span>xact<\/li>\n  <li><span class='match'>e<\/span>xample<\/li>\n  <li><span class='match'>e<\/span>xcept<\/li>\n  <li><span class='match'>e<\/span>xcuse<\/li>\n  <li><span class='match'>e<\/span>xercise<\/li>\n  <li><span class='match'>e<\/span>xist<\/li>\n  <li><span class='match'>e<\/span>xpect<\/li>\n  <li><span class='match'>e<\/span>xpense<\/li>\n  <li><span class='match'>e<\/span>xperience<\/li>\n  <li><span class='match'>e<\/span>xplain<\/li>\n  <li><span class='match'>e<\/span>xpress<\/li>\n  <li><span class='match'>e<\/span>xtra<\/li>\n  <li><span class='match'>e<\/span>ye<\/li>\n  <li><span class='match'>i<\/span>dea<\/li>\n  <li><span class='match'>i<\/span>dentify<\/li>\n  <li><span class='match'>i<\/span>f<\/li>\n  <li><span class='match'>i<\/span>magine<\/li>\n  <li><span class='match'>i<\/span>mportant<\/li>\n  <li><span class='match'>i<\/span>mprove<\/li>\n  <li><span class='match'>i<\/span>n<\/li>\n  <li><span class='match'>i<\/span>nclude<\/li>\n  <li><span class='match'>i<\/span>ncome<\/li>\n  <li><span class='match'>i<\/span>ncrease<\/li>\n  <li><span class='match'>i<\/span>ndeed<\/li>\n  <li><span class='match'>i<\/span>ndividual<\/li>\n  <li><span class='match'>i<\/span>ndustry<\/li>\n  <li><span class='match'>i<\/span>nform<\/li>\n  <li><span class='match'>i<\/span>nside<\/li>\n  <li><span class='match'>i<\/span>nstead<\/li>\n  <li><span class='match'>i<\/span>nsure<\/li>\n  <li><span class='match'>i<\/span>nterest<\/li>\n  <li><span class='match'>i<\/span>nto<\/li>\n  <li><span class='match'>i<\/span>ntroduce<\/li>\n  <li><span class='match'>i<\/span>nvest<\/li>\n  <li><span class='match'>i<\/span>nvolve<\/li>\n  <li><span class='match'>i<\/span>ssue<\/li>\n  <li><span class='match'>i<\/span>t<\/li>\n  <li><span class='match'>i<\/span>tem<\/li>\n  <li><span class='match'>o<\/span>bvious<\/li>\n  <li><span class='match'>o<\/span>ccasion<\/li>\n  <li><span class='match'>o<\/span>dd<\/li>\n  <li><span class='match'>o<\/span>f<\/li>\n  <li><span class='match'>o<\/span>ff<\/li>\n  <li><span class='match'>o<\/span>ffer<\/li>\n  <li><span class='match'>o<\/span>ffice<\/li>\n  <li><span class='match'>o<\/span>ften<\/li>\n  <li><span class='match'>o<\/span>kay<\/li>\n  <li><span class='match'>o<\/span>ld<\/li>\n  <li><span class='match'>o<\/span>n<\/li>\n  <li><span class='match'>o<\/span>nce<\/li>\n  <li><span class='match'>o<\/span>ne<\/li>\n  <li><span class='match'>o<\/span>nly<\/li>\n  <li><span class='match'>o<\/span>pen<\/li>\n  <li><span class='match'>o<\/span>perate<\/li>\n  <li><span class='match'>o<\/span>pportunity<\/li>\n  <li><span class='match'>o<\/span>ppose<\/li>\n  <li><span class='match'>o<\/span>r<\/li>\n  <li><span class='match'>o<\/span>rder<\/li>\n  <li><span class='match'>o<\/span>rganize<\/li>\n  <li><span class='match'>o<\/span>riginal<\/li>\n  <li><span class='match'>o<\/span>ther<\/li>\n  <li><span class='match'>o<\/span>therwise<\/li>\n  <li><span class='match'>o<\/span>ught<\/li>\n  <li><span class='match'>o<\/span>ut<\/li>\n  <li><span class='match'>o<\/span>ver<\/li>\n  <li><span class='match'>o<\/span>wn<\/li>\n  <li><span class='match'>u<\/span>nder<\/li>\n  <li><span class='match'>u<\/span>nderstand<\/li>\n  <li><span class='match'>u<\/span>nion<\/li>\n  <li><span class='match'>u<\/span>nit<\/li>\n  <li><span class='match'>u<\/span>nite<\/li>\n  <li><span class='match'>u<\/span>niversity<\/li>\n  <li><span class='match'>u<\/span>nless<\/li>\n  <li><span class='match'>u<\/span>ntil<\/li>\n  <li><span class='match'>u<\/span>p<\/li>\n  <li><span class='match'>u<\/span>pon<\/li>\n  <li><span class='match'>u<\/span>se<\/li>\n  <li><span class='match'>u<\/span>sual<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
  # that contain only consonants
str_view(words, "^[^aeiou]*$", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-e5352408c21493152574">{"x":{"html":"<ul>\n  <li><span class='match'>by<\/span><\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>why<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
  # that end with ed but not eed
str_view(words, "[^e]ed$", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-975d24098c39528be979">{"x":{"html":"<ul>\n  <li><span class='match'>bed<\/span><\/li>\n  <li>hund<span class='match'>red<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
  #that end with ing or ise
str_view(words, "i(se|ng)$", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-48e8878405ca4220579c">{"x":{"html":"<ul>\n  <li>advert<span class='match'>ise<\/span><\/li>\n  <li>br<span class='match'>ing<\/span><\/li>\n  <li>dur<span class='match'>ing<\/span><\/li>\n  <li>even<span class='match'>ing<\/span><\/li>\n  <li>exerc<span class='match'>ise<\/span><\/li>\n  <li>k<span class='match'>ing<\/span><\/li>\n  <li>mean<span class='match'>ing<\/span><\/li>\n  <li>morn<span class='match'>ing<\/span><\/li>\n  <li>otherw<span class='match'>ise<\/span><\/li>\n  <li>pract<span class='match'>ise<\/span><\/li>\n  <li>ra<span class='match'>ise<\/span><\/li>\n  <li>real<span class='match'>ise<\/span><\/li>\n  <li>r<span class='match'>ing<\/span><\/li>\n  <li>r<span class='match'>ise<\/span><\/li>\n  <li>s<span class='match'>ing<\/span><\/li>\n  <li>surpr<span class='match'>ise<\/span><\/li>\n  <li>th<span class='match'>ing<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Empirically verify the rule “i before e except after c”.
str_view(words, "cie", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-e497f8fe3edb2f22774d">{"x":{"html":"<ul>\n  <li>s<span class='match'>cie<\/span>nce<\/li>\n  <li>so<span class='match'>cie<\/span>ty<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
str_view(words, "[^c]ei", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-80e8a3809e94af744731">{"x":{"html":"<ul>\n  <li><span class='match'>wei<\/span>gh<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#rule not always true. 


#Is “q” always followed by a “u”?
str_view(words, "q[^u]", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-04476b0464132d3a7673">{"x":{"html":"<ul>\n  <li><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#in this dataset, yes. 


#Write a regular expression that matches a word if it’s probably written in British English, not American English.
str_view(words, "our", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-b484e727ab3263938a2e">{"x":{"html":"<ul>\n  <li>col<span class='match'>our<\/span><\/li>\n  <li>c<span class='match'>our<\/span>se<\/li>\n  <li>c<span class='match'>our<\/span>t<\/li>\n  <li>enc<span class='match'>our<\/span>age<\/li>\n  <li>fav<span class='match'>our<\/span><\/li>\n  <li>f<span class='match'>our<\/span><\/li>\n  <li>h<span class='match'>our<\/span><\/li>\n  <li>lab<span class='match'>our<\/span><\/li>\n  <li>res<span class='match'>our<\/span>ce<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Create a regular expression that will match telephone numbers as commonly written in your country.
phone_number_list = c("604-111-2345","514-456-7765", "12344556-232-22", "abcd", "1-604-928-3481", "223-33333" )

str_view(phone_number_list, "^(\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d|^\\d-\\d\\d\\d-\\d\\d\\d-\\d\\d\\d\\d)$")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-6e6db05cfa982a4e6fba">{"x":{"html":"<ul>\n  <li><span class='match'>604-111-2345<\/span><\/li>\n  <li><span class='match'>514-456-7765<\/span><\/li>\n  <li>12344556-232-22<\/li>\n  <li>abcd<\/li>\n  <li><span class='match'>1-604-928-3481<\/span><\/li>\n  <li>223-33333<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
14.3.4 Repetition
-----------------

``` r
#Describe the equivalents of ?, +, * in {m,n} form.
#? = 0 or 1 = {0,1}
#+ = 1 or more = {1,}
#* = 0 or more = {0,}


#make a vector to test repeats:
test_repeats = c("abcde", "aabbccde", "ccccccdddddd")

str_view(test_repeats, "a{0,1}")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-5b441fa5ddd3c6a39eee">{"x":{"html":"<ul>\n  <li><span class='match'>a<\/span>bcde<\/li>\n  <li><span class='match'>a<\/span>abbccde<\/li>\n  <li><span class='match'><\/span>ccccccdddddd<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
str_view(test_repeats, "b{1,}")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-3cbd69d45ec961726af9">{"x":{"html":"<ul>\n  <li>a<span class='match'>b<\/span>cde<\/li>\n  <li>aa<span class='match'>bb<\/span>ccde<\/li>\n  <li>ccccccdddddd<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
str_view(test_repeats, "a{0,}")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-362c26785f0f6224dedc">{"x":{"html":"<ul>\n  <li><span class='match'>a<\/span>bcde<\/li>\n  <li><span class='match'>aa<\/span>bbccde<\/li>\n  <li><span class='match'><\/span>ccccccdddddd<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Describe in words what these regular expressions match: 
# ^.*$ will match to any string. 
# "\\{.+\\}" will match to 1 or more characters surrounded by curly braces in a string. ex: {a} or {abcde}
# \d{4}-\d{2}-\d{2} will match to any numbers that fit the following format: 1111-11-11
#"\\\\{4}" will match to any string containing four backslashes. 
# for example: \\\\abcd (written as a string: "\\\\\\\\abcd")

str_view("\\\\\\\\abcd", "\\\\{4}")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-bae01c42d8a9994b78ee">{"x":{"html":"<ul>\n  <li><span class='match'>\\\\\\\\<\/span>abcd<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Create regular expressions to find all words that:
#start with three consonants:
str_view(words, "^[^aeiou]{3}", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-0acf24cf12a268fa1698">{"x":{"html":"<ul>\n  <li><span class='match'>Chr<\/span>ist<\/li>\n  <li><span class='match'>Chr<\/span>istmas<\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>sch<\/span>eme<\/li>\n  <li><span class='match'>sch<\/span>ool<\/li>\n  <li><span class='match'>str<\/span>aight<\/li>\n  <li><span class='match'>str<\/span>ategy<\/li>\n  <li><span class='match'>str<\/span>eet<\/li>\n  <li><span class='match'>str<\/span>ike<\/li>\n  <li><span class='match'>str<\/span>ong<\/li>\n  <li><span class='match'>str<\/span>ucture<\/li>\n  <li><span class='match'>sys<\/span>tem<\/li>\n  <li><span class='match'>thr<\/span>ee<\/li>\n  <li><span class='match'>thr<\/span>ough<\/li>\n  <li><span class='match'>thr<\/span>ow<\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>typ<\/span>e<\/li>\n  <li><span class='match'>why<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#have three or more vowels in a row:
str_view(words, "[aeiou]{3,}", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-75354ed20e9c70ec66b0">{"x":{"html":"<ul>\n  <li>b<span class='match'>eau<\/span>ty<\/li>\n  <li>obv<span class='match'>iou<\/span>s<\/li>\n  <li>prev<span class='match'>iou<\/span>s<\/li>\n  <li>q<span class='match'>uie<\/span>t<\/li>\n  <li>ser<span class='match'>iou<\/span>s<\/li>\n  <li>var<span class='match'>iou<\/span>s<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Have two or more vowel-consonant pairs in a row.
str_view(words, "([aeiou][^aeiou]){2,}", match = TRUE)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-a3d322d8797e85b0e2ff">{"x":{"html":"<ul>\n  <li>abs<span class='match'>olut<\/span>e<\/li>\n  <li><span class='match'>agen<\/span>t<\/li>\n  <li><span class='match'>alon<\/span>g<\/li>\n  <li><span class='match'>americ<\/span>a<\/li>\n  <li><span class='match'>anot<\/span>her<\/li>\n  <li><span class='match'>apar<\/span>t<\/li>\n  <li>app<span class='match'>aren<\/span>t<\/li>\n  <li>auth<span class='match'>orit<\/span>y<\/li>\n  <li>ava<span class='match'>ilab<\/span>le<\/li>\n  <li><span class='match'>awar<\/span>e<\/li>\n  <li><span class='match'>away<\/span><\/li>\n  <li>b<span class='match'>alan<\/span>ce<\/li>\n  <li>b<span class='match'>asis<\/span><\/li>\n  <li>b<span class='match'>ecom<\/span>e<\/li>\n  <li>b<span class='match'>efor<\/span>e<\/li>\n  <li>b<span class='match'>egin<\/span><\/li>\n  <li>b<span class='match'>ehin<\/span>d<\/li>\n  <li>b<span class='match'>enefit<\/span><\/li>\n  <li>b<span class='match'>usines<\/span>s<\/li>\n  <li>ch<span class='match'>arac<\/span>ter<\/li>\n  <li>cl<span class='match'>oses<\/span><\/li>\n  <li>comm<span class='match'>unit<\/span>y<\/li>\n  <li>cons<span class='match'>ider<\/span><\/li>\n  <li>c<span class='match'>over<\/span><\/li>\n  <li>d<span class='match'>ebat<\/span>e<\/li>\n  <li>d<span class='match'>ecid<\/span>e<\/li>\n  <li>d<span class='match'>ecis<\/span>ion<\/li>\n  <li>d<span class='match'>efinit<\/span>e<\/li>\n  <li>d<span class='match'>epar<\/span>tment<\/li>\n  <li>d<span class='match'>epen<\/span>d<\/li>\n  <li>d<span class='match'>esig<\/span>n<\/li>\n  <li>d<span class='match'>evelop<\/span><\/li>\n  <li>diff<span class='match'>eren<\/span>ce<\/li>\n  <li>diff<span class='match'>icul<\/span>t<\/li>\n  <li>d<span class='match'>irec<\/span>t<\/li>\n  <li>d<span class='match'>ivid<\/span>e<\/li>\n  <li>d<span class='match'>ocumen<\/span>t<\/li>\n  <li>d<span class='match'>urin<\/span>g<\/li>\n  <li><span class='match'>econom<\/span>y<\/li>\n  <li><span class='match'>educat<\/span>e<\/li>\n  <li><span class='match'>elec<\/span>t<\/li>\n  <li><span class='match'>elec<\/span>tric<\/li>\n  <li><span class='match'>eleven<\/span><\/li>\n  <li>enco<span class='match'>urag<\/span>e<\/li>\n  <li>env<span class='match'>iron<\/span>ment<\/li>\n  <li>e<span class='match'>urop<\/span>e<\/li>\n  <li><span class='match'>even<\/span><\/li>\n  <li><span class='match'>evenin<\/span>g<\/li>\n  <li><span class='match'>ever<\/span><\/li>\n  <li><span class='match'>ever<\/span>y<\/li>\n  <li><span class='match'>eviden<\/span>ce<\/li>\n  <li><span class='match'>exac<\/span>t<\/li>\n  <li><span class='match'>exam<\/span>ple<\/li>\n  <li><span class='match'>exer<\/span>cise<\/li>\n  <li><span class='match'>exis<\/span>t<\/li>\n  <li>f<span class='match'>amil<\/span>y<\/li>\n  <li>f<span class='match'>igur<\/span>e<\/li>\n  <li>f<span class='match'>inal<\/span><\/li>\n  <li>f<span class='match'>inan<\/span>ce<\/li>\n  <li>f<span class='match'>inis<\/span>h<\/li>\n  <li>fr<span class='match'>iday<\/span><\/li>\n  <li>f<span class='match'>utur<\/span>e<\/li>\n  <li>g<span class='match'>eneral<\/span><\/li>\n  <li>g<span class='match'>over<\/span>n<\/li>\n  <li>h<span class='match'>oliday<\/span><\/li>\n  <li>h<span class='match'>ones<\/span>t<\/li>\n  <li>hosp<span class='match'>ital<\/span><\/li>\n  <li>h<span class='match'>owever<\/span><\/li>\n  <li><span class='match'>iden<\/span>tify<\/li>\n  <li><span class='match'>imagin<\/span>e<\/li>\n  <li>ind<span class='match'>ivid<\/span>ual<\/li>\n  <li>int<span class='match'>eres<\/span>t<\/li>\n  <li>intr<span class='match'>oduc<\/span>e<\/li>\n  <li><span class='match'>item<\/span><\/li>\n  <li>j<span class='match'>esus<\/span><\/li>\n  <li>l<span class='match'>evel<\/span><\/li>\n  <li>l<span class='match'>ikel<\/span>y<\/li>\n  <li>l<span class='match'>imit<\/span><\/li>\n  <li>l<span class='match'>ocal<\/span><\/li>\n  <li>m<span class='match'>ajor<\/span><\/li>\n  <li>m<span class='match'>anag<\/span>e<\/li>\n  <li>me<span class='match'>anin<\/span>g<\/li>\n  <li>me<span class='match'>asur<\/span>e<\/li>\n  <li>m<span class='match'>inis<\/span>ter<\/li>\n  <li>m<span class='match'>inus<\/span><\/li>\n  <li>m<span class='match'>inut<\/span>e<\/li>\n  <li>m<span class='match'>omen<\/span>t<\/li>\n  <li>m<span class='match'>oney<\/span><\/li>\n  <li>m<span class='match'>usic<\/span><\/li>\n  <li>n<span class='match'>atur<\/span>e<\/li>\n  <li>n<span class='match'>eces<\/span>sary<\/li>\n  <li>n<span class='match'>ever<\/span><\/li>\n  <li>n<span class='match'>otic<\/span>e<\/li>\n  <li><span class='match'>okay<\/span><\/li>\n  <li><span class='match'>open<\/span><\/li>\n  <li><span class='match'>operat<\/span>e<\/li>\n  <li>opport<span class='match'>unit<\/span>y<\/li>\n  <li>org<span class='match'>aniz<\/span>e<\/li>\n  <li><span class='match'>original<\/span><\/li>\n  <li><span class='match'>over<\/span><\/li>\n  <li>p<span class='match'>aper<\/span><\/li>\n  <li>p<span class='match'>arag<\/span>raph<\/li>\n  <li>p<span class='match'>aren<\/span>t<\/li>\n  <li>part<span class='match'>icular<\/span><\/li>\n  <li>ph<span class='match'>otog<\/span>raph<\/li>\n  <li>p<span class='match'>olic<\/span>e<\/li>\n  <li>p<span class='match'>olic<\/span>y<\/li>\n  <li>p<span class='match'>olitic<\/span><\/li>\n  <li>p<span class='match'>osit<\/span>ion<\/li>\n  <li>p<span class='match'>ositiv<\/span>e<\/li>\n  <li>p<span class='match'>ower<\/span><\/li>\n  <li>pr<span class='match'>epar<\/span>e<\/li>\n  <li>pr<span class='match'>esen<\/span>t<\/li>\n  <li>pr<span class='match'>esum<\/span>e<\/li>\n  <li>pr<span class='match'>ivat<\/span>e<\/li>\n  <li>pr<span class='match'>obab<\/span>le<\/li>\n  <li>pr<span class='match'>oces<\/span>s<\/li>\n  <li>pr<span class='match'>oduc<\/span>e<\/li>\n  <li>pr<span class='match'>oduc<\/span>t<\/li>\n  <li>pr<span class='match'>ojec<\/span>t<\/li>\n  <li>pr<span class='match'>oper<\/span><\/li>\n  <li>pr<span class='match'>opos<\/span>e<\/li>\n  <li>pr<span class='match'>otec<\/span>t<\/li>\n  <li>pr<span class='match'>ovid<\/span>e<\/li>\n  <li>qu<span class='match'>alit<\/span>y<\/li>\n  <li>re<span class='match'>alis<\/span>e<\/li>\n  <li>re<span class='match'>ason<\/span><\/li>\n  <li>r<span class='match'>ecen<\/span>t<\/li>\n  <li>r<span class='match'>ecog<\/span>nize<\/li>\n  <li>r<span class='match'>ecom<\/span>mend<\/li>\n  <li>r<span class='match'>ecor<\/span>d<\/li>\n  <li>r<span class='match'>educ<\/span>e<\/li>\n  <li>r<span class='match'>efer<\/span><\/li>\n  <li>r<span class='match'>egar<\/span>d<\/li>\n  <li>r<span class='match'>elat<\/span>ion<\/li>\n  <li>r<span class='match'>emem<\/span>ber<\/li>\n  <li>r<span class='match'>epor<\/span>t<\/li>\n  <li>repr<span class='match'>esen<\/span>t<\/li>\n  <li>r<span class='match'>esul<\/span>t<\/li>\n  <li>r<span class='match'>etur<\/span>n<\/li>\n  <li>s<span class='match'>atur<\/span>day<\/li>\n  <li>s<span class='match'>econ<\/span>d<\/li>\n  <li>secr<span class='match'>etar<\/span>y<\/li>\n  <li>s<span class='match'>ecur<\/span>e<\/li>\n  <li>s<span class='match'>eparat<\/span>e<\/li>\n  <li>s<span class='match'>even<\/span><\/li>\n  <li>s<span class='match'>imilar<\/span><\/li>\n  <li>sp<span class='match'>ecific<\/span><\/li>\n  <li>str<span class='match'>ateg<\/span>y<\/li>\n  <li>st<span class='match'>uden<\/span>t<\/li>\n  <li>st<span class='match'>upid<\/span><\/li>\n  <li>t<span class='match'>elep<\/span>hone<\/li>\n  <li>t<span class='match'>elevis<\/span>ion<\/li>\n  <li>th<span class='match'>erefor<\/span>e<\/li>\n  <li>tho<span class='match'>usan<\/span>d<\/li>\n  <li>t<span class='match'>oday<\/span><\/li>\n  <li>t<span class='match'>oget<\/span>her<\/li>\n  <li>t<span class='match'>omor<\/span>row<\/li>\n  <li>t<span class='match'>onig<\/span>ht<\/li>\n  <li>t<span class='match'>otal<\/span><\/li>\n  <li>t<span class='match'>owar<\/span>d<\/li>\n  <li>tr<span class='match'>avel<\/span><\/li>\n  <li><span class='match'>unit<\/span><\/li>\n  <li><span class='match'>unit<\/span>e<\/li>\n  <li><span class='match'>univer<\/span>sity<\/li>\n  <li><span class='match'>upon<\/span><\/li>\n  <li>v<span class='match'>isit<\/span><\/li>\n  <li>w<span class='match'>ater<\/span><\/li>\n  <li>w<span class='match'>oman<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Solve the beginner regexp crosswords at https://regexcrossword.com/challenges/beginner.
#was not able to recreate the crossword here
#but I was able to solve 2 of the beginner crosswords that I attempted.
```

14.3.5 Grouping and backreferences
----------------------------------

``` r
#describe in words what these expressions will match:
#(.)\1\1 will match any character that repeats 3 times

#"(.)(.)\\2\\1" will match any characters that fit the following pattern: "lool" or "saas"

#(..)\1 will match any characters that repeat twice such as "haha" or "hehe"

#"(.).\\1.\\1" will match any characters where the first character is repeated three times, 
#starting once at the start of the pattern, then two more times but with any character in between the two other times. 
#for example "nanin" "tgtkt"

#"(.)(.)(.).*\\3\\2\\1" will match strings with the pattern: "abckcba" or "123i321" or "123ijklmnop321"
a = "123iiddssfe32144444"
str_view(a, "(.)(.)(.).*\\3\\2\\1")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-0a424ee57f600989a0a9">{"x":{"html":"<ul>\n  <li><span class='match'>123iiddssfe321<\/span>44444<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Construct regular expressions to match words that:
#Start and end with the same character.

str_view(c("bob","snacks","test"),"(.).*\\1")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-3a14a3e8697df2a4c2b4">{"x":{"html":"<ul>\n  <li><span class='match'>bob<\/span><\/li>\n  <li><span class='match'>snacks<\/span><\/li>\n  <li><span class='match'>test<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Contain a repeated pair of letters (e.g. “church” contains “ch” repeated twice.)
str_view(c("church","gg123gg"), "(..).*\\1")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-d3e809559d354ab9a894">{"x":{"html":"<ul>\n  <li><span class='match'>church<\/span><\/li>\n  <li><span class='match'>gg123gg<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#Contain one letter repeated in at least three places (e.g. “eleven” contains three “e”s.)
str_view(c("eleven", "notamatch", "caravans", "carraavvans"), "(.).*\\1.*\\1.*")
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-011bb98c124eb45843ce">{"x":{"html":"<ul>\n  <li><span class='match'>eleven<\/span><\/li>\n  <li>notamatch<\/li>\n  <li>c<span class='match'>aravans<\/span><\/li>\n  <li>c<span class='match'>arraavvans<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
14.4.1 Detect matches
---------------------

``` r
library(stringi)
library(stringr)
#For each of the following challenges, try solving it by using both a single regular expression, 
#and a combination of multiple str_detect() calls.

#words starting or ending in x
words_in_x = c("six", "seven", "xylophone", "exit")
endx = str_detect(words_in_x,"x$")
startx = str_detect(words_in_x,"^x")
words_in_x[endx|startx]
```

    ## [1] "six"       "xylophone"

``` r
#words starting with vowel and ending in consonant
vowel_consonant = c("notamatch","elect", "too", "all", "alice")
start_vowel = str_detect(vowel_consonant, "^[aeiou]")
end_consonant = str_detect(vowel_consonant, "[^aeiou]$")
vowel_consonant[start_vowel & end_consonant]
```

    ## [1] "elect" "all"

``` r
#Are there any words that contain at least one of each different vowel?
#no there are not, as no words are returned from the call below:
contain_a = str_detect(words, "a")
contain_e = str_detect(words, "e")
contain_i = str_detect(words,"i")
contain_o = str_detect(words,"o")
contain_u = str_detect(words,"u")
words[contain_a & contain_e & contain_i & contain_o & contain_u]
```

    ## character(0)

``` r
#What word has the highest number of vowels? 
number_vowels = str_count(words, "[aeiou]")
words[which(number_vowels == max(number_vowels))] %>% 
  head()
```

    ## [1] "appropriate" "associate"   "available"   "colleague"   "encourage"  
    ## [6] "experience"

``` r
#What word has the highest proportion of vowels? (Hint: what is the denominator?)

#highest proportion: 
proportion_vowels = str_count(words, "[aeiou]")/str_length(words)
words[which(proportion_vowels == max(proportion_vowels))] %>% 
  head()
```

    ## [1] "a"

14.4.3 Extract matches
----------------------

``` r
#In the previous example, you might have noticed that the regular expression matched “flickered”, which is not a colour. Modify the regex to fix the problem.
#adding [^a-z] will remove any a-z characters before the word red.
#using \\b \\b to surround the word of interest will avoid matching with words that contain it. 

colours <- c("\\bred\\b", "orange", "yellow", "green", "blue", "purple")
colour_match <- str_c(colours, collapse = "|")
more <- sentences[str_count(sentences, colour_match) > 1]
str_view_all(more, colour_match)
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-5c744e23e2022233907d">{"x":{"html":"<ul>\n  <li>It is hard to erase <span class='match'>blue<\/span> or <span class='match'>red<\/span> ink.<\/li>\n  <li>The sky in the west is tinged with <span class='match'>orange<\/span> <span class='match'>red<\/span>.<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#extract the first word from each sentence:
sentences %>% 
  str_extract("[A-Za-z]+") %>% 
  head()
```

    ## [1] "The"   "Glue"  "It"    "These" "Rice"  "The"

``` r
#extract all words ending in ing: 

unlist(str_extract_all(sentences, "[a-zA-Z]+ing")) %>% 
  head()
```

    ## [1] "stocking" "spring"   "evening"  "morning"  "winding"  "living"

``` r
#plurals
#for this exercise I will look at words ending in "s"

unlist(str_extract_all(sentences, "[a-zA-Z]+s")) %>% 
  head()
```

    ## [1] "planks" "eas"    "Thes"   "days"   "is"     "dis"

14.4.4 Grouped matches
----------------------

``` r
#Find all words that come after a “number” like “one”, “two”, “three” etc. 
#Pull out both the number and the word.
tibble(sentence = sentences) %>% 
 str_extract_all("(one|two|three|four|five|six|seven|eight|nine|ten) ([^ ]+)"
  )
```

    ## Warning in stri_extract_all_regex(string, pattern, simplify = simplify, :
    ## argument is not an atomic vector; coercing

    ## [[1]]
    ##  [1] "ten served"      "one over"        "seven books"    
    ##  [4] "two met"         "two factors"     "one and"        
    ##  [7] "three lists"     "seven is"        "two when"       
    ## [10] "one floor.\","   "ten inches.\","  "one with"       
    ## [13] "one war"         "one button"      "six minutes.\","
    ## [16] "ten years"       "one in"          "ten chased"     
    ## [19] "one like"        "two shares"      "two distinct"   
    ## [22] "one costs"       "five cents"      "ten two"        
    ## [25] "five robins.\"," "four kinds"      "one rang"       
    ## [28] "ten him.\","     "three story"     "ten by"         
    ## [31] "one wall.\","    "three inches"    "ten your"       
    ## [34] "six comes"       "ten than"        "one before"     
    ## [37] "three batches"   "two leaves.\","

``` r
#Find all contractions.

tibble(sentence = sentences) %>% 
  str_extract_all("[^ ]+\\'[^ .]")
```

    ## Warning in stri_extract_all_regex(string, pattern, simplify = simplify, :
    ## argument is not an atomic vector; coercing

    ## [[1]]
    ##  [1] "\"It's"     "man's"      "don't"      "store's"    "workmen's" 
    ##  [6] "\"Let's"    "sun's"      "child's"    "king's"     "\n\"It's"  
    ## [11] "don't"      "queen's"    "don't"      "pirate's"   "neighbor's"

14.4.5 Replacing matches
------------------------

``` r
#Replace all forward slashes in a string with backslashes.

slashes = c("ab/cd", "/efgh", "/////", "defghi//")
backslashes = str_replace_all(slashes,"\\/", "\\\\")
#returns the raw contents of the string:
writeLines(backslashes)
```

    ## ab\cd
    ## \efgh
    ## \\\\\
    ## defghi\\

``` r
#Implement a simple version of str_to_lower() using replace_all().
str_replace_all(sentences, c("A" = "a", "B" = "b", "C" = "c", "D" = "d", "E" = "e", "F" = "f", "G" = "g", "H" = "h", "I" = "i", "J" = "j", "K" = "k", "L" = "l", "M" = "m", "N" = "n", "O" = "o", "P"="p", "Q"="q", "R" = "r", "S" = "s", "T" = "t", "U" = "u", "V" = "v", "W" = "w", "X" ="x", "Y" = "y", "Z" ="z" )) %>% 
  head()
```

    ## [1] "the birch canoe slid on the smooth planks." 
    ## [2] "glue the sheet to the dark blue background."
    ## [3] "it's easy to tell the depth of a well."     
    ## [4] "these days a chicken leg is a rare dish."   
    ## [5] "rice is often served in round bowls."       
    ## [6] "the juice of lemons makes fine punch."

``` r
#Switch the first and last letters in words. Which of those strings are still words?
words [words %in% str_replace(words, "^([a-zA-Z])(.*)([a-z])$", "\\3\\2\\1")] %>% 
  head()
```

    ## [1] "a"       "america" "area"    "dad"     "dead"    "deal"

14.4.6 Splitting
----------------

``` r
#Split up a string like "apples, pears, and bananas" into individual components.

str_split("apples, pears, and bananas", ", and |, ")
```

    ## [[1]]
    ## [1] "apples"  "pears"   "bananas"

``` r
#Why is it better to split up by boundary("word") than " "?
#using boundary will handle spaces and other characters not part of a word (linke punctuation or numbers)

#What does splitting with an empty string ("") do? Experiment, and then read the documentation.
str_split("apples, pears, and bananas", "")
```

    ## [[1]]
    ##  [1] "a" "p" "p" "l" "e" "s" "," " " "p" "e" "a" "r" "s" "," " " "a" "n"
    ## [18] "d" " " "b" "a" "n" "a" "n" "a" "s"

``` r
#splitting by "" will split the string into individual characters, 
#including spaces (whitespace), and punctuation.
```

14.5 Other types of pattern
---------------------------

``` r
#How would you find all strings containing \ with regex() vs. with fixed()?
#using regex(): 
test_vector_4 = c("\\\\\\", "abcde", "\\abc")
writeLines(test_vector_4)
```

    ## \\\
    ## abcde
    ## \abc

``` r
str_view_all(test_vector_4, regex("\\\\"))
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-2c941b43f7f4c341819e">{"x":{"html":"<ul>\n  <li><span class='match'>\\<\/span><span class='match'>\\<\/span><span class='match'>\\<\/span><\/li>\n  <li>abcde<\/li>\n  <li><span class='match'>\\<\/span>abc<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#using fixed():
str_view_all(test_vector_4, fixed("\\"))
```

<!--html_preserve-->

<script type="application/json" data-for="htmlwidget-5309c6286c61fddae380">{"x":{"html":"<ul>\n  <li><span class='match'>\\<\/span><span class='match'>\\<\/span><span class='match'>\\<\/span><\/li>\n  <li>abcde<\/li>\n  <li><span class='match'>\\<\/span>abc<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script>
<!--/html_preserve-->
``` r
#What are the five most common words in sentences?
#obtain a vector of all words in the sentences list by using str_extract_all and use unlist to convert the list to a vector:
words_in_sentences = unlist(str_extract_all(str_to_lower(sentences), boundary("word")))

topwords = words_in_sentences %>% 
  tibble() %>% 
  set_names("commonwords") %>% 
  group_by(commonwords) %>% 
  count() 



topwords
```

    ## # A tibble: 1,904 x 2
    ## # Groups:   commonwords [1,904]
    ##    commonwords     n
    ##    <chr>       <int>
    ##  1 a             202
    ##  2 about           1
    ##  3 abrupt          1
    ##  4 absent          1
    ##  5 account         1
    ##  6 acid            1
    ##  7 across          3
    ##  8 act             4
    ##  9 actor           1
    ## 10 actress         1
    ## # ... with 1,894 more rows

``` r
#I was not able to get past this point. 
#could not determine how to adequately sort the data to obtain top most used words.
#visually, by sorting through this counted data, I was able to determine that some the most common words include:
# "a", "and", "in", "is"
```

14.7 stringi
------------

``` r
#Find the stringi functions that:
#https://www.rdocumentation.org/packages/stringi/versions/1.2.4
#Count the number of words. = stri_count_boundaries
#Find duplicated strings. = stri_duplicated
#Generate random text. = stri_rand_strings

#How do you control the language that stri_sort() uses for sorting?
#use locale = " " to control the language based on the as a ISO 639 language code https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes
```

Part 2: Write one (or more) functions that do something useful to pieces of the Gapminder data.
===============================================================================================

``` r
library(gapminder)
suppressPackageStartupMessages(library(ggplot2))
suppressPackageStartupMessages(library(dplyr))
```

The first part of this exercise is based on the tutorial provided on [linear regression functions](http://stat545.com/block012_function-regress-lifeexp-on-year.html)
The first part is heavily based on the tutorial, but is necessary to provide context and understanding for the functions that I make next. I specify below the point where I depart from the tutorial and start with my own work.

The second part of this exercise will branch off from this starting point.

First step, pick one country to work with as a starting point:

``` r
country_ = "Canada"
country_data_Canada = gapminder %>% 
  filter (country =="Canada")
country_data_Canada
```

    ## # A tibble: 12 x 6
    ##    country continent  year lifeExp      pop gdpPercap
    ##    <fct>   <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Canada  Americas   1952    68.8 14785584    11367.
    ##  2 Canada  Americas   1957    70.0 17010154    12490.
    ##  3 Canada  Americas   1962    71.3 18985849    13462.
    ##  4 Canada  Americas   1967    72.1 20819767    16077.
    ##  5 Canada  Americas   1972    72.9 22284500    18971.
    ##  6 Canada  Americas   1977    74.2 23796400    22091.
    ##  7 Canada  Americas   1982    75.8 25201900    22899.
    ##  8 Canada  Americas   1987    76.9 26549700    26627.
    ##  9 Canada  Americas   1992    78.0 28523502    26343.
    ## 10 Canada  Americas   1997    78.6 30305843    28955.
    ## 11 Canada  Americas   2002    79.8 31902268    33329.
    ## 12 Canada  Americas   2007    80.7 33390141    36319.

Next, make a rough plot to get an idea of the data:

``` r
country_data_Canada %>% 
  ggplot() +aes(x = year, y = lifeExp) +
  geom_smooth(method = "lm") + 
  geom_point()
```

![](STAT547_hw06_JasmineLib_files/figure-markdown_github/unnamed-chunk-21-1.png)

Visually, it appears that the linear regression line does fit the data, in this case. We should still check the fit of the regression:

``` r
fit_lm_regression_canada = lm(lifeExp ~ year, country_data_Canada)
#look at the intercepts:
coefficients(fit_lm_regression_canada)
```

    ##  (Intercept)         year 
    ## -358.3488923    0.2188692

From this intercept, we know that something is off in our fit, because the intercept should no go all the way down to -358 years. Here, we can reset the parameters for the plot so that we make the intercept match up to 1952, our earliest datapoint:

``` r
fit_lm_regression_canada = lm(lifeExp ~I(year-1952), country_data_Canada)
coefficients(fit_lm_regression_canada)
```

    ##    (Intercept) I(year - 1952) 
    ##     68.8838462      0.2188692

Now we can take this and apply it to a function:

``` r
linear_regression_fit = function(gap_data, offset = 1952) {
  linear_fit = lm(lifeExp~I(year-offset), gap_data)
  setNames(coef(linear_fit), c("intercept", "slope"))
} 

linear_regression_fit(country_data_Canada)
```

    ##  intercept      slope 
    ## 68.8838462  0.2188692

Testing this model on another country:

``` r
country_data_Kenya = gapminder %>% 
  filter(country =="Kenya")

linear_regression_fit(country_data_Kenya)
```

    ##  intercept      slope 
    ## 47.0020385  0.2065077

``` r
country_data_Kenya %>% 
  ggplot() + aes(x = year, y = lifeExp) +
  geom_smooth(method = "lm") +
  geom_point()
```

![](STAT547_hw06_JasmineLib_files/figure-markdown_github/unnamed-chunk-25-1.png)

*Here is where I start original work*

We can see here that while the function worked, it appears to fit a quadratic model, rather than linear. This is how we can generalize the lm method to fit a quadratic model:

``` r
#first, make a ggplot of what we would be modeling in our function to visualize:
country_data_Kenya %>% 
  ggplot() + aes(x = year, y = lifeExp) +
  geom_smooth(method ="lm", formula = y~x+I(x^2)) +
  geom_point()
```

![](STAT547_hw06_JasmineLib_files/figure-markdown_github/unnamed-chunk-26-1.png)

``` r
#write a function for quadratic equation fitting to the data
quadratic_regression_fit = function(gap_data, offset = 1952) {
  #add a square function here:
  quadratic_fit = lm(lifeExp~I(year-offset)+I((year-offset)^2), gap_data)
  setNames(coefficients(quadratic_fit), c("intercept", "coefficient x", "coefficient x^2"))

} 

quadratic_regression_fit(country_data_Kenya)
```

    ##       intercept   coefficient x coefficient x^2 
    ##     40.72898901      0.95927363     -0.01368665

This is great, but it would be easier if we didn't have to create a dataset each time we want to call our function. This can be modified by having our function create it's own filtered dataset by filtering by country name.

``` r
#update linear regression function: 
linear_regression_fit = function(countryname = "", offset = 1952) {
  gap_data = gapminder %>%
    filter(country == countryname)
  linear_fit = lm(lifeExp~I(year-offset), gap_data)
  setNames(coef(linear_fit), c("intercept", "slope"))
} 

#test to see if we get the same coefficients for Canada as before:
linear_regression_fit("Canada")
```

    ##  intercept      slope 
    ## 68.8838462  0.2188692

``` r
#update quadratic regression function:
quadratic_regression_fit = function(countryname = "", offset = 1952) {
  gap_data = gapminder %>% 
  filter(country == countryname)
  quadratic_fit = lm(lifeExp~I(year-offset)+I((year-offset)^2), gap_data)
  setNames(coefficients(quadratic_fit), c("intercept", "coefficient x", "coefficient x^2"))
}

#test to see if we get the same coefficients for Kenya as before:
quadratic_regression_fit("Kenya")
```

    ##       intercept   coefficient x coefficient x^2 
    ##     40.72898901      0.95927363     -0.01368665

Conclude: Yes, the coefficients obtained for Canada's linear fit and Kenya's quadratic are the same as previously obtained.

For the sake of this assignment, I also found it useful to have a way to directly plot either a linear or quadratic fit into ggplot data:

``` r
linearfit_ggplot = function (countryname = "") {
  gapminder %>% 
  filter (country ==countryname) %>% 
  ggplot() + aes(x = year, y = lifeExp) +
  geom_smooth(method ="lm") +
  geom_point() + 
  ggtitle("Linear Fit of Life Expectancy Over Time") 
  
}

linearfit_ggplot("Japan")
```

![](STAT547_hw06_JasmineLib_files/figure-markdown_github/unnamed-chunk-28-1.png)

``` r
quadraticfit_ggplot = function (countryname = "") {
  gapminder %>% 
    filter(country == countryname) %>% 
    ggplot() + aes(x = year, y = lifeExp) +
    geom_smooth(method = "lm", formula = y~x+I(x^2) )+
    geom_point() + 
    ggtitle("Quadratic Fit of Life Expectancy Over Time")
  
}

quadraticfit_ggplot("Kenya")
```

![](STAT547_hw06_JasmineLib_files/figure-markdown_github/unnamed-chunk-28-2.png)

Conclusion: Here we have simplified the process of making a quick plot to check our functions, by writing another function for linear and quadratic fits in ggplot.

The next step is to test the function: Try to break the function

``` r
#the function should not work if I enter a country that is not in the gapminder dataset.
#linearfit_ggplot("abc")
#quadratic_regression_fit("abc")
#quadraticfit_ggplot("abc")
#linear_regression_fit("abc")

#the function should not work if I enter a non-string argument:
#linearfit_ggplot(1)
#quadratic_regression_fit(2)
#quadraticfit_ggplot(3)
#linear_regression_fit(4)

quadratic_regression_fit = function(countryname = "", offset = 1952) {
  
  if (!is.character(countryname)) {
        stop(paste("expecting input for countryname to be a string corresponding to a country in ggplot. You gave me ",
                   typeof(countryname)))
    }
  gap_data = gapminder %>% 
  filter(country == countryname)
  quadratic_fit = lm(lifeExp~I(year-offset)+I((year-offset)^2), gap_data)
  setNames(coefficients(quadratic_fit), c("intercept", "coefficient x", "coefficient x^2"))
}

#now if we enter a non-character input, we will return an error:
#returns you gave me list:
#quadratic_regression_fit(gapminder)

#returns you gave me double:
#quadratic_regression_fit(2)

#this should work:
quadratic_regression_fit("Kenya")
```

    ##       intercept   coefficient x coefficient x^2 
    ##     40.72898901      0.95927363     -0.01368665

``` r
#applying the same stop if not for the linear regression function:
linear_regression_fit = function(countryname = "", offset = 1952) {
  if (!is.character(countryname)) {
        stop(paste("expecting input for countryname to be a string corresponding to a country in ggplot. You gave me ",
                   typeof(countryname)))
    }
  gap_data = gapminder %>%
    filter(country == countryname)
  linear_fit = lm(lifeExp~I(year-offset), gap_data)
  setNames(coef(linear_fit), c("intercept", "slope"))
} 

#returns you gave me double: 
#linear_regression_fit(2)
```

Now that we have these two nice functions to make linear and quadratic fits for the data, the next logical step is to be able to estimate life expectancy based on this data.

``` r
quadratic_regression_fit_estimate = function(countryname = "", year_estimate = 1999, offset = 1952) {
  #we want the year_estimate to be an integer between 1952 and 2007 (useful for estimating years not provided in gapminder dataset )
   if (!is.double(year_estimate)) {
        stop(paste("expecting input for year_estimate to be an integer between 1952 and 2007. You gave me ",
                   typeof(year_estimate)))
    }
  if (!is.character(countryname)) {
        stop(paste("expecting input for countryname to be a string corresponding to a country in ggplot. You gave me ",
                   typeof(countryname)))
  }
   if (year_estimate > 2007 | year_estimate <1952) {
        stop("expecting input for year_estimate to be an integer between 1952 and 2007.")
    }
  gap_data = gapminder %>% 
  filter(country == countryname)
  quadratic_fit = lm(lifeExp~I(year-offset)+I((year-offset)^2), gap_data)
  setNames(coefficients(quadratic_fit), c("intercept", "coefficient x", "coefficient x^2"))
  x2_coeff = coefficients(quadratic_fit)[3]
  x_coeff = coefficients(quadratic_fit)[2]
  intercept_value = coefficients(quadratic_fit)[1]
  offsetyear = year_estimate-offset
 # x2_coeff
  #x_coeff
  #intercept_value
 life_exp_estimate = intercept_value + x2_coeff*offsetyear^2 + x_coeff*offsetyear
 life_exp_estimate
}
#this returns a value of 55.24 years. 
quadratic_regression_fit_estimate("Kenya", 2000)
```

    ## (Intercept) 
    ##    55.24007

``` r
#checking with our ggplot quadratic fit, does this estimate seem reasonable? 
quadraticfit_ggplot("Kenya")
```

![](STAT547_hw06_JasmineLib_files/figure-markdown_github/unnamed-chunk-30-1.png)

Conclude: Yes, from the output returned and the quadratic fit plot, the estimate of 55 years does seem reasonable.

Now we can repeat the same process to estimate life expectancy for data fitted to a linear model:

``` r
linear_regression_fit_estimate = function(countryname = "", year_estimate = 1999, offset = 1952) {
  #we want the year_estimate to be an integer between 1952 and 2007 (useful for estimating years not provided in gapminder dataset )
   if (!is.double(year_estimate)) {
        stop(paste("expecting input for year_estimate to be an integer between 1952 and 2007. You gave me ",
                   typeof(year_estimate)))
    }
  if (!is.character(countryname)) {
        stop(paste("expecting input for countryname to be a string corresponding to a country in ggplot. You gave me ",
                   typeof(countryname)))
  }
   if (year_estimate > 2007 | year_estimate <1952) {
        stop("expecting input for year_estimate to be an integer between 1952 and 2007.")
   }
  
  gap_data = gapminder %>%
    filter(country == countryname)
  linear_fit = lm(lifeExp~I(year-offset), gap_data)
  setNames(coef(linear_fit), c("intercept", "slope"))
  x_coeff = coefficients(linear_fit)[2]
  intercept_value = coefficients(linear_fit)[1]
  offsetyear = year_estimate-offset
  #x_coeff
  #intercept_value
 life_exp_estimate = intercept_value + x_coeff*offsetyear
 life_exp_estimate
} 

linear_regression_fit_estimate("Canada", 2000)
```

    ## (Intercept) 
    ##    79.38957

``` r
linearfit_ggplot("Canada")
```

![](STAT547_hw06_JasmineLib_files/figure-markdown_github/unnamed-chunk-31-1.png)

Conclude:
Sanity Check: the estimated life expectancy for this year matches with what we can see on the plot of the linear regression.
