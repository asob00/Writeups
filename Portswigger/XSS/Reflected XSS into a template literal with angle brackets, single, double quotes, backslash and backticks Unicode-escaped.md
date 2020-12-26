
## Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped

## What do we know?
- site contains reflected XSS in search functionality
- angle brackets, single, double quotes, backslash and backticks are unicode-escaped

## What do we have to do?
- call alert when author name is clicked

## Solution:
Our task tells us that we need to perform XSS into a template, first thing, that came to my mind is this graph:
![](https://portswigger.net/cms/images/migration/blog/screen-shot-2015-07-20-at-09-21-56.png)

Source: https://portswigger.net/research/server-side-template-injection

I started checking these payloads and after typing `${"z".join("ab")}` I got empty response message.
After lurking into script I realized that my double quotes got escaped (forgot about escaping).

Finally I decided to go back to first step (`${7*7}`) and mess with it a little bit. Solution was very easy, it was enough to put just `alert(1)` into the expression like `${alert(1)}`. 

## Final payload looks like this:
- `${alert(1)}`
