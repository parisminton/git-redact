git-redact
==========

Changelog
---------

**2/21/13**

Redacts multiple files, and confirms changes made to each file separately.





**2/19/13**

1. Patterns that aren\'t found don\'t print to the confirmation report.

2. Instead of appearing many times in the confirmation report, redaction matches are printed only once for all their occurrences. 




**1/8/13**

1. Confirmation reporting is prettier. If a redaction occurs on more than one line, the line numbers are separated by commas before they're printed in the sentence. The last number is separated from the penultimate number by spaces and the word 'and'.

The odd line break is gone.

2. More consistent replacement of word boundaries.



**1/6/13**

As a first step, git-redact tests for the operating system. If it's OS X, it uses that system's ugly word boundary syntax (`[[:<:]]` and `[[:>:]]`).

In the confirmation step, git-redact will print the original word boundary syntax as entered by the user.



**12/28/12**

The test inside `replacePatternWithMatch()` now uses `sed` instead of `grep`.



**12/20/12**

Before redactions are made, any regex word boundary (`\b`) matches in `swap_array` are substituted for the syntax understood by sed in OS X (`[[:<:]]` and `[[:>:]]`). Eventually, I'd like to test for the OS and replace this value with the syntax understood by each.



**12/11/12**

The `operations` array contains actual line numbers and actual values for every pattern matched during a redaction. This array is used to log results.



**12/9/12**

1. Collect all the redact/replace values in an array. Write out two `sed` files -- one to perform the redactions, and another to collect the line numbers of each transformation.

2. Added a summary report. After it runs, git-redact will print all its changes to the console and ask the user to approve them before making them permanent.
