% Better Commit Messages
% Nick Telford
% 2016-04-26

![https://xkcd.com/1296/](https://imgs.xkcd.com/comics/git_commit.png "xkcd Git Commit")

## Structure of a Commit

    Summary of commit, maximum 50 characters

    Description of commit. This can be of any number of lines, with paragraphs
    and any other formatting you like. The one restriction is that the maximum 
    line-length is about 72 characters.
    
    Closes FTA-239

# The Summary

## Purpose

To provide a concise *summary* of the commit's changes.

## Project History

    $ git log --oneline --no-merges

    a188091 Remove debug log messages that leaked token info
    100455c Add initial integration test suite
    6044db8 Major refactoring of tokauth to new specifications
    cf628a4 Tell jDBI how to return an Option of results
    ebb9188 Provide a default mapper for JacksonDeserializer
    ac9df88 Fix not correctly initialising Kafka Deserializers
    c145740 Fix multi-threaded access in Kafka Consumer

## Pull Request Title

![Creating a Pull Request from a single commit](img/pr-from-commit.png)\ 

## Rules

- First line of commit message
- One line, no wrapping
- **50 characters** maximum
- Don't end the line with a full-stop
- Capitalise first letter
- Use "commands" rather than "descriptions"

. . .

  ----------------------------------------- -------- -------- ---------------------------------------
  <span style="color:green">&#x2713;</span> Add          Adds <span style="color:red">&#x2715;</span>
  <span style="color:green">&#x2713;</span> Update    Updated <span style="color:red">&#x2715;</span>
  <span style="color:green">&#x2713;</span> Removing Removing <span style="color:red">&#x2715;</span>
  ----------------------------------------- -------- -------- ---------------------------------------

# The Description

## Purpose

To provide *an explanation* of the changes made.

> - The diff communicates **what** changed
> - The description communicates:
>     + _**why** the change was made_
>     + _**how** the change works_
>     + _**what** impact the change has_
> - Include references to tickets _at the end_

## Pull Request Description

![Creating a Pull Request from a single commit](img/pr-from-commit.png)\ 

## Rules

- **72 characters** per-line maximum
- Don't repeat the summary line
- Use of paragraphs and lists encouraged
- Use Markdown syntax for:
    + Punctuation (sparingly)
    + Code-blocks

# Writing Good Commit Messages

## The `git` Command

> - Never use `git commit -m 'message'`:
>     + Overly terse
>     + No description
>     + Discourages you from _thinking_ about your commit
> - Use `git commit`:
>     + Uses your default `EDITOR`, usually VI
>     + Cancel commit by quitting editor without saving or saving a blank 
        message.

## The Editor (VIM)

![VIM, with the blank commit message template](img/vim-template.png)\ 

## The Editor (VIM)

![VIM, demonstrating commit message highlighting](img/vim-highlighting.png)\ 

## The Message

The complexity of the commit message should reflect the complexity of the 
change:

. . .

- Single-line messages are fine for _trivial_ changes
    + Fixing a typo
    + Making whitespace changes
    + Tidying up mundane code (comments, imports etc.)

## One Change, One Commit

> - Use `git diff` and `git add` to select files to stage
> - Use `git add --patch` (aka. `git add -p`)
>     + Interactively stages chunks of changed files
> - Use a local WIP branch for more complex work
>     + Rebase and squash your commits before pushing
>     + Ensure squashed commit message is well-written
>     + Use `git commit --amend` to edit message

# Real-world examples

## Bad

    Major overhaul

. . .

    298 additions and 609 deletions.

---

![Excerpt from "The Scream", by Edvard Munch](img/scream.jpg)\ 

## Good

    Refactor namespace parsing in rapidjson parser

    Currently, while recursively parsing interaction objects, a `namespaces`
    stack is maintained with the fully qualified name of each namespace.

    Example:

    ```json
    {"a": { "b": { "c": { "d": 123 } } } }
    ```

    For the above JSON, the following elements will be pushed on to the
    namespaces stack:

    ```
    a
    a.b
    a.b.c
    a.b.c.d
    ```

    Only leaf nodes in the object tree are valid targets (`a.b.c.d` in our
    example), the other elements serving only as a prefix to the next.

    Constructing these intermediary namespaces is expensive, because the
    string appends create a bunch of temporaries, and this occurs for every
    branch node, not just the leaves.

    This refactoring instead places just the node itself on the stack, when
    a leaf node is reached the fully-qualified target is materialized by
    iterating the stack and joining the elements with the separator.

    For our above example, the following stack would be maintained:

    ```
    a
    b
    c
    d
    ```

    At which point, the `build_target` method will be used to walk the stack
    and generate `a.b.c.d` as the target name for the leaf node.

    Note: `build_target` is a custom implementation similar to
    `boost::algorithm::join`, this permits us to pre-allocate an estimate
    for the size of the target before joining, substantially reducing
    re-allocation overhead.

## I'm a Bad Person

     Merge branch 'develop' into fix/refactor-namespace-parsing

                            much change.
        so confuse.
                                wow.

---

## References

Pro Git
:   Scott Chacon and Ben Straub
:   https://git-scm.com/book/ch5-2.html

How to Write a Git Commit Message
:   Chris Beams
:   http://chris.beams.io/posts/git-commit/

