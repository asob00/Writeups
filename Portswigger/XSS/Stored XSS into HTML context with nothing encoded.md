
## Stored XSS into HTML context with nothing encoded

## What do we know?
- site contains stored XSS in comments functionality
- nothing is encoded 

## What do we have to do?
- call alert when comments section is viewed

## Solution:
At first glance task looks pretty straight forward. We need find out how site stores our comments, close specific HTML tag and inject script tag inside. 

I start by putting random data into input fields, so i can see how this data is stored in HTML code.

**I find additional requirements:**
- Email has to have some-text@some-text format
- Website has to begin with http://

**My data:**
- Comment: a
- Name: aa
- Email: a@a
- Website: http://a

After sending that request I can see that my name is stored inside `<a>` and my comment is stored inside `<p>` tag:

`<p>a</p>`
`<a id="author" href="http://a">aa</a>`.

After seeing that all i have to do is break out of **a** or **p**tag and inject `<script>alert(1)</script>`, I decided to break out of **p** just by closing it with `</p>`.


## Final payload looks like this:
- Comment: `</p><script>alert(1)</script>`
- Name: a
- Email: a@a
- Website: http://
