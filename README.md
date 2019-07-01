<h1>UNK</h1>

<h2>a very small static site generator</h2>

__UNK__ is an experiment in minimalism.
It is a templating static site generator
with an included markup language
that all fits with 1000 bytes.
There are three main scripts:

<ul>
    <li><strong>UNK</strong>, a bash script that applies the template
        to each page and publishes them to the output dir,</li>
    <li><strong>LHT</strong>, an awk script that serves as a (very) basic
        markup language, and</li>
    <li><strong>TM</strong>,
        the default template script for <strong>UNK</strong>.</li>
</ul>

__UNK__ and __LHT__ are 250 bytes each, for a total of 500 bytes.
__TM__ takes up the remaining 500 bytes
of the target 1000 bytes.
You are, of course, free to make the template file as large
and involved as you like.

<h1>DETAILS</h1>

<h2>unk</h2>

__UNK__ takes a set of files in a directory, applies a template to them,
and output them into another directory as HTML files ready for a server.
To keep a very small size, __UNK__ delegates most file processing to __TM__,
the main template.  It delegates by using an idea found in
<a href="https://github.com/zimbatm/shab">shab</a>:
each input file is read as a `heredoc`, which enables
shell interpolation.
So the template, as opposed to the engine,
can do all the heavy-lifting of index generation and navigation and such.

Content goes into the following (hard-coded) directories:

<ul>
    <li><strong>I/</strong>,
        for written (<em><strong>I</strong>nput</em>) content
        (the pages of the site),</li>
    <li><strong>S/</strong>, for <em><strong>S</strong>tatic</em> content
        (css, images, etc.), &amp;</li>
    <li><strong>O/</strong>, for the (<em><strong>O</strong>utput</em>)
        website, ready for <code>rsync</code>ing to a server.</li>
</ul>

If there is no __TM__ in the directory where __UNK__ is run,
one will be created that will simply `cat` the file being processed.

The following variables are made available to __TM__:

<ul>
    <li><strong>FN</strong>: the <em>FileName</em>
        (with directories removed) of the file being processed</li>
    <li><strong>TT</strong>: the <em>TiTle</em>
        (the first line) of the file</li>
    <li><strong>BD</strong>: the <em>BoDy</em>
        (the rest) of the file</li>
</ul>

as well as this function:

<ul>
    <li><strong>X</strong>, for <em>eXpand</em>:
        the <code>shab</code> stand-in.
        It is much simpler than <code>shab</code>,
        and will fail if the template
        (or if it nests templates, one of the nested ones)
        has a <code>ZZ</code> on a line by itself,
        due to its <code>heredoc</code> nature.</li>
</ul>

and these aliases (though they're more an artefact of saving space
in the script, but they can be used in templates):

<ul>
    <li><strong>c</strong>: <code>cat</code></li>
    <li><strong>q</strong>: <code>test</code></li>
    <li><strong>e</strong>: <code>echo</code></li>
</ul>

As mentioned above, templates can be nested.
Simply call another template from __TM__ with the __X__ function.

<h2>lht</h2>

__LHT__ stands for *Less HyperText*,
because that's what you're writing when you're writing it
(though not much less than HTML).
Basically,
blank lines are interpreted as <code>&lt;p&gt;</code> tag breaks,
unless the previous source paragraph started with
<code>&lt;</code> and ended with <code>&gt;</code>.
It also has support for three inline spans:

<ul>
    <li><code>&#42;em&#42;</code>
        as <em>em</em></li>
    <li><code>&#95;&#95;strong&#95;&#95;</code>
        as <strong>strong</strong></li>
    <li><code>&#96;code&#96; as <code>code</code></li>
</ul>

Everything else is just HTML.
This means that a valid `.lht` file is *almost* a valid `.md` file,
except where it nests HTML and Markdown
(so it's not really, but you can run it through Markdown in a pinch
and get the basic idea across.
This file, for example, is both `index.lht` and `README.md`
(they're just symlinked to each other),
so it's got some weirdness to keep things compatible between Markdown and LHT.
But if you're just writing for LHT, it can be much simpler.).

__LHT__ was inspired, in part, by
<a href="http://john.ankarstrom.se/html">Writing HTML in HTML</a>
by John Ankarstrom,
as well as some other articles I can't think of right now.
I liked the idea, but some tags in HTML are just annoying to write
over and over, and take me out of the flow of writing prose.
So I fixed those few tags.
__The inline tags are definitely subject to change.__

<h1>Why?</h1>

I was bored and decided I'd try to write a static site generator
that could fit in a
<a href="https://writing.exchange/web/statuses/102333562361891512">toot</a>
(500 characters).
I
<a href="https://writing.exchange/web/statuses/102334522981990897">wrote</a>
<a href="https://writing.exchange/web/statuses/102334522981990897">a few</a>
<a href="https://writing.exchange/web/statuses/102339851501562648">of them</a>,
making them smaller and smaller each time.
By the end, I was left with a *tiny* script
that delegated almost *all* the work to the template file.
That script became __UNK__ in this repo.

I was feeling pretty high on my horse after writing the tiny SSG,
so I thought,
<em><a href="https://writing.exchange/@acdw/102339290120562386">maybe
I could try for a tootable Markdown converter next</a></em>
boy, was I wrong about that.
Markdown is *way* too complicated to fit in 500 bytes.
So I just wrote the Really Important Parts: <code>&lt;p&gt;</code>
and some inlines.

<h1>LEGAL</h1>

Copyright &copy; 2019 Case Duckworth
<a href="mailto:acdw@acdw.net">&lt;acdw@acdw.net&gt;</a>.

This work is free.
You can redistribute it and/or modify it under the terms of
the Do What The Fuck You Want To Public License, Version 2,
as published by Sam Hocevar.
See the LICENSE file for more details.

<h2>Why this license?</h2>

I was going to go with a stricter license like the GPL,
but realized that

<ol>
    <li>this software isn't so important or time-consuming that I need
        others to credit me or redistribute the project under the same terms,
        and</li>
    <li>the GPL is <em>way</em> too long for a project like this.
        It's over 35 times <em>bigger</em> than the entirety of this project,
        not counting the content or this README.
        It would weigh down the entire undertaking.
        The WTFPL, by contrast, is a trim 443 characters,
        which is right in keeping with the smallness of this project.</li>
</ol>
