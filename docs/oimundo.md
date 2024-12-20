Oi, mundo!

# Python-Markdown cheatsheet (hd1)

## Python-Markdown (hd2)

### Python-Markdown (hd3)

#### Python-Markdown (hd4)

hr:

---

## Middle-Word Emphasis

    some_long_filename.txt  
    some _long_ filename.txt

some_long_filename.txt  
some _long_ filename.txt

## Indentation/Tab Length & Consecutive Lists

    - list
    *    list
    +    list
    1.   list
        - list
        -    list

- list
* list
+ list
1. list
   - list
   - list

## Blockquote

https://daringfireball.net/projects/markdown/syntax

```
> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
id sem consectetuer libero luctus adipiscing.
```

> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.

```
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.
```

> This is the first level of quoting.
> 
> > This is nested blockquote.
> 
> Back to the first level.

```
> ## This is a header.
> 
> 1.   This is the first list item.
> 2.   This is the second list item.
> 
> Here's some example code:
> 
>     return shell_exec("echo $input | $markdown_script");
```

> ## This is a header.
> 
> 1. This is the first list item.
> 2. This is the second list item.
> 
> Here's some example code:
> 
>     return shell_exec("echo $input | $markdown_script");

---

## Lists

    0.  Bird
    0.  McHale
    0.  Parish

0. Bird
1. McHale
2. Parish

---

List markers typically start at the left margin, but may be indented by up to three spaces.

    *   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
        Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
        viverra nec, fringilla in, laoreet vitae, risus.
    *   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
        Suspendisse id sem consectetuer libero luctus adipiscing.

* Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
  Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
  viverra nec, fringilla in, laoreet vitae, risus.
* Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
  Suspendisse id sem consectetuer libero luctus adipiscing.

---

If list items are separated by blank lines, Markdown will wrap the items in <p> tags in the HTML output.

    *   Bird
    
    *   Magic

* Bird

* Magic

---

List items may consist of multiple paragraphs. Each subsequent paragraph in a list item must be indented by either 4 spaces or one tab:

    1.  This is a list item with two paragraphs. Lorem ipsum dolor
        sit amet, consectetuer adipiscing elit. Aliquam hendrerit
        mi posuere lectus.
    
        Vestibulum enim wisi, viverra nec, fringilla in, laoreet
        vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
        sit amet velit.
    
    2.  Suspendisse id sem consectetuer libero luctus adipiscing.

1. This is a list item with two paragraphs. Lorem ipsum dolor
   sit amet, consectetuer adipiscing elit. Aliquam hendrerit
   mi posuere lectus.
   
   Vestibulum enim wisi, viverra nec, fringilla in, laoreet
   vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
   sit amet velit.

2. Suspendisse id sem consectetuer libero luctus adipiscing.

---

It looks nice if you indent every line of the subsequent paragraphs, but here again, Markdown will allow you to be lazy:

    *   This is a list item with two paragraphs.
    
        This is the second paragraph in the list item. You're
    only required to indent the first line. Lorem ipsum dolor
    sit amet, consectetuer adipiscing elit.
    
    *   Another item in the same list.

* This is a list item with two paragraphs.
  
  This is the second paragraph in the list item. You're
  only required to indent the first line. Lorem ipsum dolor
  sit amet, consectetuer adipiscing elit.

* Another item in the same list.

---

To put a blockquote within a list item, the blockquote’s > delimiters need to be indented:

    *   A list item with a blockquote:
    
        > This is a blockquote
        > inside a list item.

* A list item with a blockquote:
  
  > This is a blockquote
  > inside a list item.

---

To put a code block within a list item, the code block needs to be indented twice — 8 spaces or two tabs:

    *   A list item with a code block:
    
            <code goes here>

* A list item with a code block:
  
      <code goes here>

---

    1986. What a great season.
    
    1986\. What a great season.

1986. What a great season.

1986\. What a great season.

---

## Rules

    * * *
    
    ***
    
    *****
    
    - - -
    
    ---------------------------------------

* * *

***

*****

- - -

---------------------------------------

---

## Span

Markdown supports two style of links: inline and reference.

    This is [an example](http://example.com/ "Title") inline link.
    
    [This link](http://example.net/) has no title attribute.

This is [an example](http://example.com/ "Title") inline link.

[This link](http://example.net/) has no title attribute.

---

If you’re referring to a local resource on the same server, you can use relative paths:

    See my [About](teaching/index.md) page for details.   

See my [About](teaching/index.md) page for details.   

Reference-style links use a second set of square brackets, inside which you place a label of your choosing to identify the link:

    This is [an example][id] reference-style link.

This is [an example][id] reference-style link.

Then, anywhere in the document, you define your link label like this, on a line by itself:

    [id]: http://example.com/longish/path/to/resource/here
        "Optional Title Here"

[id]: http://example.com/longish/path/to/resource/here
    "Optional Title Here"

---

The implicit link name shortcut allows you to omit the name of the link, in which case the link text itself is used as the name. Just use an empty set of square brackets — e.g., to link the word “Google” to the google.com web site, you could simply write:

    [Google][]

[Google][]

And then define the link:

    [Google]: http://google.com/

[Google]: http://google.com/

---

    I get 10 times more traffic from [Google] [1] than from
    [Yahoo] [2] or [MSN] [3].
    
    [1]: http://google.com/        "Google"
    [2]: http://search.yahoo.com/  "Yahoo Search"
    [3]: http://search.msn.com/    "MSN Search"
    
    I get 10 times more traffic from [Google][] than from
    [Yahoo][] or [MSN][].
    
    [google]: http://google.com/        "Google"
    [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
    [msn]:    http://search.msn.com/    "MSN Search"

I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].

  [1]: http://google.com/        "Google"
  [2]: http://search.yahoo.com/  "Yahoo Search"
  [3]: http://search.msn.com/    "MSN Search"

I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].

  [google]: http://google.com/        "Google"
  [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
  [msn]:    http://search.msn.com/    "MSN Search"

---

## Emphasis

    *single asterisks*
    
    _single underscores_
    
    **double asterisks**
    
    __double underscores__
    
    \*this text is surrounded by literal asterisks\*

*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

\*this text is surrounded by literal asterisks\*

---

## Code

    Use the `printf()` function.
    
    ``There is a literal backtick (`) here.``
    
    A single backtick in a code span: `` ` ``
    
    A backtick-delimited string in a code span: `` `foo` ``
    
    Please don't use any `<blink>` tags.

Use the `printf()` function.

``There is a literal backtick (`) here.``

A single backtick in a code span: `` ` ``

A backtick-delimited string in a code span: `` `foo` ``

Please don't use any `<blink>` tags.

---

## Images

    ![Alt text](img/favicon.ico)
    
    ![Alt text](img/favicon.ico "Optional title")

![Alt text](img/favicon.ico)

![Alt text](img/favicon.ico "Optional title")

---

    ![Alt text][id]
    
    [id]: img/favicon.ico  "Optional title attribute"

![Alt text][id]

[id]: img/favicon.ico  "Optional title attribute"

---

## Automatic links

    <http://example.com/>
    
    http://example.com/

<http://example.com/>

http://example.com/

---

## Escapes

    \\   backslash
    \`   backtick
    \*   asterisk
    \_   underscore
    \{\}  curly braces
    \[\]  square brackets
    \(\)  parentheses
    \#   hash mark
    \+   plus sign
    \-   minus sign (hyphen)
    \.   dot
    \!   exclamation mark

\\   backslash  
\`   backtick  
\*   asterisk  
\_   underscore  
\{\}  curly braces  
\[\]  square brackets  
\(\)  parentheses  
\#   hash mark  
\+   plus sign  
\-   minus sign (hyphen)  
\.   dot  
\!   exclamation mark  

---

## Extensions

MkDocs default: meta, toc, tables, and fenced_code

## Fenced Code Blocks

A paragraph before the code block.

    ```
    a one-line code block
    ```

```
a one-line code block
```

A paragraph after the code block.

    ~~~
    a one-line code block
    ~~~

```
a one-line code block
```

To include a set of backticks (or tildes) within a code block, use a different number of backticks for the deliminators.

    ````
    ```
    ````

```
```
```

Unlike indented code blocks, a fenced code block can immediately follow a list item without becoming part of the list.

    * A list item.
    
    ```
    not part of the list
    ```

* A list item.

```
not part of the list
```

---

    ``` { .html }
    <p>HTML Document</p>
    ```

```{
<p>HTML Document</p>
```

---

    ``` { #example }
    A linkable code block
    ```

```{
A linkable code block
```

---

    ```bash
    $ pip install mkdocs
    ```
    ```python
    for _ in range(1): 
        print(oi)
    ```

```bash
$ pip install mkdocs
```

```python
for _ in range(1): 
    print(oi)
```

---

## Tables

Tables are defined using the syntax established in PHP Markdown Extra

https://michelf.ca/projects/php-markdown/extra/#table

    First Header  | Second Header
    ------------- | -------------
    Content Cell  | Content Cell
    Content Cell  | Content Cell

| First Header | Second Header |
| ------------ | ------------- |
| Content Cell | Content Cell  |
| Content Cell | Content Cell  |

---

You can specify alignment for each column by adding colons to separator lines. A colon at the left of the separator line will make the column left-aligned; a colon on the right of the line will make the column right-aligned; colons at both side means the column is center-aligned.

    | Item      | Value |
    | --------- | -----:|
    | Computer  | $1600 |
    | Phone     |   $12 |
    | Pipe      |    $1 |

| Item     | Value |
| -------- | -----:|
| Computer | $1600 |
| Phone    | $12   |
| Pipe     | $1    |

---

You can apply span-level formatting to the content of each cell using regular Markdown syntax:

    | Function name | Description                    |
    | ------------- | ------------------------------ |
    | `help()`      | Display the help window.       |
    | `destroy()`   | **Destroy your computer!**     |

| Function name | Description                |
| ------------- | -------------------------- |
| `help()`      | Display the help window.   |
| `destroy()`   | **Destroy your computer!** |

---

# Admonition

    !!! note
    
        The [`site_name`][site_name] and [`site_url`][site_url] configuration
        options are the only two required options in your configuration file. When
        you create a new project, the `site_url` option is assigned the placeholder
        value: `https://example.com`. If the final location is known, you can change
        the setting now to point to it. Or you may choose to leave it alone for now.
        Just be sure to edit it before you deploy your site to a production server.

!!! note
    The [`site_name`][site_name] and [`site_url`][site_url] configuration
    options are the only two required options in your configuration file. When
    you create a new project, the `site_url` option is assigned the placeholder
    value: `https://example.com`. If the final location is known, you can change
    the setting now to point to it. Or you may choose to leave it alone for now.
    Just be sure to edit it before you deploy your site to a production server.

---

    !!! warning
        Using absolute paths with links is not officially supported. Relative paths
        are adjusted by MkDocs to ensure they are always relative to the page. Absolute
        paths are not modified at all. This means that your links using absolute paths
        might work fine in your local environment but they might break once you deploy
        them to your production server.

!!! warning
    Using absolute paths with links is not officially supported. Relative paths
    are adjusted by MkDocs to ensure they are always relative to the page. Absolute
    paths are not modified at all. This means that your links using absolute paths
    might work fine in your local environment but they might break once you deploy
    them to your production server.

---

# def_list

    Apple
    :   Pomaceous fruit of plants of the genus Malus in
        the family Rosaceae.
    
    Orange
    :   The fruit of an evergreen tree of the genus Citrus.

Apple
: Pomaceous fruit of plants of the genus Malus in
    the family Rosaceae.

Orange
: The fruit of an evergreen tree of the genus Citrus.

---

    `permalink`:
    : Generate permanent links at the end of each header. Default: `False`.
    
        When set to True the paragraph symbol (¶ or `¶`) is used as the
        link text. When set to a string, the provided string is used as the link
        text. For example, to use the hash symbol (`#`) instead, do:
    
            markdown_extensions:
                - toc:
                    permalink: "#"

`permalink`:
: Generate permanent links at the end of each header. Default: `False`.

    When set to True the paragraph symbol (¶ or `¶`) is used as the
    link text. When set to a string, the provided string is used as the link
    text. For example, to use the hash symbol (`#`) instead, do:
    
        markdown_extensions:
            - toc:
                permalink: "#"

---

Katex

    $$
    \cos x=\sum_{k=0}^{\infty}\frac{(-1)^k}{(2k)!}x^{2k}
    $$

$$
\cos x=\sum_{k=0}^{\infty}\frac{(-1)^k}{(2k)!}x^{2k}
$$

    The homomorphism $f$ is injective if and only if its kernel is only the singleton set $e_G$, because otherwise $\exists a,b\in G$ with $a\neq b$ such that $f(a)=f(b)$.

The homomorphism $f$ is injective if and only if its kernel is only the singleton set $e_G$, because otherwise $\exists a,b\in G$ with $a\neq b$ such that $f(a)=f(b)$.

---

Other extensions: <https://www.mkdocs.org/user-guide/writing-your-docs/>
