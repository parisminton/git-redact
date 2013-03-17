git-redact
==========

**git-redact** is a utility to automate the removal of stuff you don't want in your repo before publishing, like `console.log()`. Or, you know, passwords.

[For now (1)][1], you'll run this utility before `git add`, stripping out the unwanted values before you commit them. You set these values beforehand in a *.gitredact* file, in the same set-it-and-forget-it way you'd use *.gitignore*.


How it works
------------

You supply **git-redact** with a list of terms to remove or replace. Using this list, it writes a sed file and instructions, which it runs on each file that's been modified since the last commit. All the deletions and replacements are done with sed.

Your original files are copied to an ignored backup directory and renamed. The copies, which have now been redacted, are what Git will see when it's time to stage files for committing.

At this point, **git-redact** prints to the terminal a list of all the changes it's made and asks if you approve. Hit 'y' and it prints a confirmation message and exits 0. Hit 'n' and it deletes the redacted copies, moves the originals back into the directory under their old names, prints a confirmation and exits 1.


How to use it
-------------

### Set up ###

1. Move the *git-redact* file (not to be confused with *.gitredact*; note the hyphen and leading dot in each) somehwere your `$PATH` environment variable will see it, like /usr/local. Modify `$PATH` if necessary.

2. Switch to the Git repo in which you'll be doing automated redactions. Here, you'll need a *.gitredact* file and a directory named *.redacted_originals* (note the leading dot.) If you don't make these yourself, **git-redact** will create them if it sees they're missing.

3. Add patterns to *.gitredact*, [as explained below][2].

4. Work as usual until you're ready to commit.

### Run ###

1. At the prompt: `git redact`.

2. **git-redact** will do its thing, show you a list of all the changes it's made in each file and ask you to approve. Based on your yea or nay, it finishes by telling you whether the original or redacted files are ready for staging.

### Restore ###

1. Run `git redact` on a repo that's already been redacted. If it sees files in the *redacted_originals* folder that match filenames in the root, it will replace and rename the redacted ones with the originals.

2. When prompted, decline the changes.

Yeah. I'm working on a better workflow for this.

<a name="gitredactfile" />
The *.gitredact* file
-------------------

This file is simply a list of patterns **git-redact** uses to search for terms to remove and replace. Each uncommented line of *.gitredact* should contain one of the following:

- A pair of values separated by an equals sign. The pattern used for removal should be on the left of the equals sign, with the value replacing it on the right.

`my_secret_password = password_placeholder`

- One pattern on a line by itself. The value matching this pattern will be removed and not replaced.

`# Does this cummerbund make me look fat?`

The *.gitredact* file needs to contain at least one pattern for **git-redact** to work. If *.gitredact*  is empty or doesn't contain any patterns, **git-redact** outputs an alert message and exits 1 without attempting anything. 

If *.gitredact* doesn't exist, **git-redact** creates it, outputs a confirmation message and exits 1. It won't redact until the user adds patterns to the *.gitredact* file, and it will print a reminder each time it's run with no patterns available.

### On patterns ###

Because **git-redact** relies on sed, the values it tests for are patterns, not literal strings. It supports sed's regular expression syntax, so `console.log(.*)` will match any instance of `console.log(` followed by any combination of characters followed by a closing `)`.

Bookend a pattern with `\b` to match it standing alone in the file.


Footnotes
---------

<a name="githooks" />
1. I've given some thought to firing these processes on the pre-commit hook, so you wouldn't have to type any extra commands to start them. Is this in line with Git's usage philosophy, though?


[1]: #githooks "I've given some thought to firing these processes on the pre-commit hook."
[2]: #gitredactfile "How to set up the .gitredact file."
