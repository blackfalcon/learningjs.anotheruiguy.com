> {{book.title}} v{{book.version}}

# This is an example document

Educational documents are to be created using a simple text format with Markdown enhancement. This allows for greater flexibility in rendering documents as either PDFs, static web docs, or even embedding content into other platforms.

If you are not familiar with Markdown, please review this [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

## Common examples

The following are common examples used for creating a Markdown style document.

#### Code

Probably one of the most useful tools in Markdown is it's easy process for adding code examples to a document and syntax highlighting.

For inline code references, simply wrap the string with the back-tick <code>\` \`</code> or grave accent character.

```
This is an `inline` code example
```

For large blocks of code, using three back-ticks followed by the syntax of the code you want to highlight, and then closing that with three more back-ticks, Markdown will process this to be properly formatted code in the text document.

<pre>
```js
  var thing = 'foo';
```

```css
.thing {
  color: orange;
}
```

```python
import datetime
now = datetime.datetime.now()
```
</pre>

```js
  var thing = 'foo';
```

```css
.thing {
  color: orange;
}
```

```python
import datetime
now = datetime.datetime.now()
```

#### Headers

Headers are an important tool for creating content separation.

```
# H1
## H2
### H3
#### H4
##### H5
###### H6
```

#### Emphasis

Using emphasis can help to draw attention to content in the document.

```
Emphasis, aka italics, with *asterisks* or _underscores_.

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~
```

#### Lists

While there are multiple ways to create lists in Markdown, there is a preference for using a single `1.` per line item and allowing Markdown to create the appropriate numbered list.

```
1. list item thing words
1. list item thing words
1. list item thing words
  1. list item thing words
  1. list item thing words
  1. list item thing words
1. list item thing words
1. list item thing words
```

#### Links

Again, there are a number of ways to create links in Markdown, but for simplicity, the inline-style link is most common.

```
Here is a [link](http://www.example.com/link.html) to a document on the Internet.
```

#### Images

Inserting images into Markdown is a little tricky as you do not have design control over how the images are placed. The syntax is very simple to use.

Inline-style:
```
![alt text](https://github.com/common/images/icon48.png "Logo Title Text 1")
```

Reference-style:
```
![alt text][logo]
[logo]: https://github.com/common/images/icon48.png "Logo Title Text 2"
```

#### Blockquotes

Blockquotes help to draw visual attention to the quited content in your document. To create a blockquote, simply use the `>` greater-than character preceding the content you intend to quote.

```
> This is quoted text that I want to draw attention to in my written document.
```

> This is quoted text that I want to draw attention to in my written document.
