Introduction
=====================================

`Rcpp <http://www.rcpp.org/>`_ is a huge project aiming at interfacing R to C++ seamlessly.
Originally it was developed to provide an easier and more expressive API on top of R,
compared with the official R API designed for C language. Now after nearly ten years'
improvement, Rcpp has already been greatly enriched, with many useful features like Rcpp sugar,
Rcpp attributes and Rcpp modules. As for now, there are more than 200 packages on
`CRAN <http://cran.r-project.org/>`_ that directly use the functionality provided by Rcpp, and
this number is still increasing.

However, despite the broad use of Rcpp among developers in the R community, presently there isn't
any official function-by-function reference to give a complete description of the Rcpp API.
And what I'm trying to do here is to document the most useful classes and functions
(but **NOT** all of them) inside Rcpp.

Note that this is an **unofficial** reference of Rcpp, so the content here
is not complete nor 100% accurate. To make the documentation more integrated and
articulated, any feedback, suggestion and pull requests are highly appreciated.

