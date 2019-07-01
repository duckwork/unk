# UNK

## a very small static site generator

**UNK** is an experiment in minimalism.
It is a templating static site generator
with an included markup language
that all fits with 1000 bytes.
There are three main scripts:

- **UNK**, a bash script that applies the template
  to each page and publishes them to the output dir, and
- **LHT**, an awk script that serves as a (very) basic
  markup language.
- **TM**, the default template file for **UNK**.

Both scripts are 250 bytes each, for a total of 500 bytes.
The default template file takes up the remaining 500 bytes
of the target 1000 bytes.
You are, of course, free to make the template file as large
and involved as you like.

# DETAILS

## UNK

**UNK** takes a set of files in a directory, applies a template to them,
and output them into another directory as HTML files ready for a server.
To keep a very small size, **UNK** delegates most file processing to **TM**,
the main template.  It delegates by using an idea found in
[shab](https://github.com/zimbatm/shab):
each input file is read as a `heredoc`, which enables
shell interpolation.
So the template, as opposed to the engine,
can do all the heavy-lifting of index generation and navigation and such.
That means all the

Content goes into the following (hard-coded) directories:

- **I/**, for written (*__i__nput*) content (the pages of the site),
- **S/**, for ***s***tatic content (css, images, etc.), &
- **O/**, for the (*__o__utput*) website, ready for `rsync`ing
          to a server.

If there is no **TM** in the directory where **UNK** is run,
one will be created that will simply echo the file being processed.

The following variables are made available to **TM**:

- **FN**: the *FileName* (with directories removed)
  of the file being processed
- **TT**: the *TiTle* (the first line) of the file
- **BD**: the *BoDy* (the rest) of the file

as well as this function:

- **X**, for *eXpand*: the `shab` stand-in.
  It is much simpler than `shab`, and will fail if the template
  (or if it nests templates, one of the nested ones)
  has a `ZZ` on a line by itself, due to its `heredoc` nature.

and these aliases (though they're more an artefact of saving space
in the script, but they can be used in templates):

- **c**: `cat`
- **q**: `test`
- **e**: `echo`

As mentioned above, templates can be nested.
Simply call another template from **TM** with the **X** function.

## LHT

**LHT** stands for *Less HyperText*,
because that's what you're writing when you're writing it
(though not much less than HTML).
Basically,
blank lines are interpreted as `<p>` tag breaks,
unless the previous source paragraph started with `<` and ended with `>`.
It also has support for three inline spans:

- `*em*` or `_em_` as *em*
- `**strong**` or `__strong__` as **strong**
- `\`code\`` as `code`.

Everything else is just HTML.

**LHT** was inspired, in part, by
[Writing HTML in HTML](http://john.ankarstrom.se/html) by John Ankarstrom,
as well as some other articles I can't think of right now.
I liked the idea, but some tags in HTML are just annoying to write
over and over, and take me out of the flow of writing prose.
So I fixed those few tags.
**The inline tags are definitely subject to change.**

# Why?

I was bored and decided I'd try to write a static site generator
that could fit in a [toot] (500 characters).
I [wrote][1] [a few][2] [of them][3],
making them smaller and smaller each time.
By the end, I was left with a *tiny* script
that delegated almost *all* the work to the template file.
That script became **UNK** in this repo.

[toot]: https://writing.exchange/web/statuses/102333562361891512
[1]: https://writing.exchange/web/statuses/102334522981990897
[2]: https://writing.exchange/web/statuses/102334522981990897
[3]: https://writing.exchange/web/statuses/102339851501562648

I was feeling pretty high on my horse after writing the tiny SSG,
so I thought,
*[maybe I could try for a tootable Markdown converter next][4]* --
boy, was I wrong about that.
Markdown is *way* too complicated to fit in 500 bytes.
So I just wrote the Really Important Parts: `<p>` and some inlines.

[4]: https://writing.exchange/@acdw/102339290120562386

# LEGAL

Copyright &copy; 2019 Case Duckworth <acdw@acdw.net>
This work is free.
You can redistribute it and/or modify it under the terms of
the Do What The Fuck You Want To Public License, Version 2,
as published by Sam Hocevar.
See the LICENSE file for more details.

## Why this license?

I was going to go with a stricter license like the GPL,
but realized that

1. this software isn't so important or time-consuming that I need
   others to credit me or redistribute the project under the same terms, and
2. the GPL is *way* too long for a project like this.
   It's over 35 times *bigger* than the entirety of this project,
   not counting the content or this README.
   It would weigh down the entire undertaking.
   The WTFPL, by contrast, is a trim 443 characters,
   which is right in keeping with the smallness of this project.
