git-redact
==========

Changelog
---------

**12/9/12**

1. Collect all the redact/replace values in an array. Write out two `sed` files -- one to perform the redactions, and another to collect the line numbers of each transformation.

2. Added a summary report. After it runs, git-redact will print all its changes to the console and ask the user to approve them before making them permanent.
