## What do we know?
- site contains stored XSS in comments section
- angle brackets and double quotes are HTML-encoded
- single quotes and backslashes are escaped

## What do we have to do?
- call alert when author name is clicked

## Solution:

Firstly I tried filling inputs with random data to see how website interacts with user data. 

**I found some requirements:**
- email field needs to be filled with some-text@some-text
- website field has to begin with http://

I didn't notice any further requirements.

Now it's time to look into source of the website. My focus lays mostly on onclick event of `<a>` parameter, where my website address is stored, because alert is supposed to be called from there.
Onclick event of `<a>` parameter contains some tracking code. Inside the code my website address is stored between colons. My first thought was to escape string, run alert and comment out the rest.
I tried to insert some "forbidden" characters not really hoping for anything and as expected nothing really happened.
Next I tried using different encodings. If encoded string enters javascript in unencoded form I could bypass escaping colons or backslashes. 

After some time of trial and error i found that UTF-8 encoded string will work. 
Inserted payload looked like `http://&#x27 aaa`.

In Firefox inspector `&#x27` showed up as colon so I was close to solving the challenge. 
Next thing to do was obviously inserting alert into the code.

**Javascript code:**
`var tracker={track(){}};tracker.track('my-url' );`

To insert alert al I had to do was close string which contained my url, close parentheses, insert semicolon and my alert(1), then comment out the rest :)

Final payload looks like this:
**Comment:** a
**Name:** a
**Email:** a@a
**Website:** http://&#x27); alert(1); //
