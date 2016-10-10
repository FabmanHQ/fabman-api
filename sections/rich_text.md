# Rich text

Many resources such as Members, Equipment and Packages have attributes that can contain rich text as HTML. Rich text attributes may contain lists, block quotes, links and simple text formatting.

## Working with rich text HTML
If your application reads Basecamp rich text content, it must be able to process HTML. Use a web view component to render rich text content unmodified. Or, if your application needs a plain-text representation of rich text content, first decode any HTML entities, then replace `<br>` tags with line breaks, and finally strip the remaining HTML tags.

Similarly, if your application writes rich text content, it must be able to generate well-formed HTML. At a minimum, this means properly encoding HTML entities and replacing line breaks with `<br>` tags.

Applications that modify existing rich text content should take special care not to discard any formatting or attachments during processing. Consider using a full HTML parser to manipulate rich text content. Libraries such as [Nokogiri](http://www.nokogiri.org) (Ruby) and [Cheerio](https://github.com/cheeriojs/cheerio) (Node.js) are good fits for this scenario.


## Allowed HTML tags
You may use the following standard HTML tags in rich text content: `div`, `br`, `strong`, `em`, `strike`, `a` (with a `href` attribute), `pre`, `ol`, `ul`, `li`, and `blockquote`. 

