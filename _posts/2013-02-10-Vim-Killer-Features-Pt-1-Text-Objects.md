---
layout: post
title: "Vim Killer Features Part 1: Text Objects"
---

Vim has a notorious learning curve which often leads beginners to give up and
return to a more traditional editor or IDE. I tried to learn Vim many times
before I was able to fully embrace it, each time spending mountains of effort
simply trying to get to parity with my productivity in other editors. While
learning Vim will always require some frustration, a key insight is to find the
killer features that no other editor will provide you.  This is part one in a
series of posts about features in Vim I couldn't live without.

Most editors require navigating by mouse or keyboard to the correct location and
backspacing over some text character by character. I find this to be a very
inefficient process. Vim instead works by applying operators to blocks of text.
We can supply a block of text to an operator using our motions.  For instance we
can use `dw` to delete text up to the start of the next word, or `yfx` to copy
text up to the next occurrence of the character 'x'. However, this method really
provides little benefit over other editors, since we still need to navigate to
the start and end of any region of text we wish to operate on. To compound
problem, Vim movements are extremely unintuitive to the novice. Navigating to
the start and end of text blocks is not easy for Vim beginners, and even feels
clumsy to all but Vim experts

Luckily, Vim allows us to specify a block of text in a more intuitive way. Plain
text is naturally broken up into words, sentences, and paragraphs. When
programming, pieces of text are often delimited by punctuation, like
`array[indices]`, `"strings"`, or `<html>tags</html>`. We can reference text
that is grouped into logical chunks like this using Vim's *text objects*. Any
operator that takes a motion will take a text object instead. This lets us avoid
having to navigate to the start and end of regions, we simply need to move the
cursor *anywhere* within the object.

All text objects come in two forms, normal and inner (prefixed by `a` and `i`,
respectively). As a general rule, normal text objects include the characters
that delimit the object, while the inner form will leave the delimiters intact.
The normal form is often used when deleting an object and the inner form is
often useful for changing an object. For example, `da"` will delete a double
quoted string (perhaps to replace it with a variable), while `ci"` will leave
the quotes intact, and only delete the content inside the string, ready to be
altered.

Here's a list of built-in text objects:

- Word by punctuation: `aw`/`iw`
- Word by whitespace: `aW`/`iW` (see `:help WORD`)
- Sentence: `as`/`is`
- Paragraph: `ap`/`ip`
- Quotes: `a'`/`i'` or `a"`/`i"`
- Parentheses: `a)`/`i)`
- Brackets: `a]`/`i]`
- Braces: `a}`/`i}`
- Angle Brackets: `a>`/`i>`
- Tags (e.g. `<html>inner</html>`): `at`/`it`

There are also a number of useful plugins which extend Vim's built-in pool of
text objects to include some very useful objects.

- [ArgTextObj](https://github.com/vim-scripts/argtextobj.vim)

  Adds an object for arguments lists of the form `(arg1, arg2, arg3)`,
  where `ia` will target an argument and `aa` will include the comma if
  present.

- [vim-textobj-entire](https://github.com/kana/vim-textobj-entire)

  Gives us an object to refer to the entire buffer. This is useful to
  replace the `gg<command>G` idiom. For example, to format the entire buffer
  use `=ae`, or to start fresh use `dae`

- [vim-textobj-indent](http://www.vim.org/scripts/script.php?script_id=2484)

  Allows us to select lines based on their indentation. Useful in languages
  with syntactically significant whitespace like python or coffeescript.
  `ii` targets all contigious lines with the same or deeper indentation, while `ai` does the
  same while skipping over whitespace. The `aI` and `iI` variants are similar,
  but only select lines with the *same* level of indentation.

- [vim-textobj-url](https://github.com/mattn/vim-textobj-url)

  Adds support for URLs with the `au`/`iu` text object.

- [vim-textobj-comment](https://github.com/thinca/vim-textobj-comment)

  With this plugin, we can target text inside multi-line and single line
  comments, with `ac` including the comment tokens (`//` or `/* */`) and `ic`
  operating inside.
