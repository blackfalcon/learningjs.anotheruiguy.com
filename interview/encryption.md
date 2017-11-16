# Basic encryption

So when interviewing with another engineer there was a question that really grabbed my interest. "_Using only JavaScript, what kinds of things can you do to provide basic security and encryption?_"

Off the top of my head, answers like "_Obfuscation and base 64_" jumped out at me. But what does that really mean? How can we use these tools to thwart off simple hackers? So, to really answer this question I came up with a small project, one that would use these tools to 'encrypt' the code and keep people from simply 'looking' at the code to gain access.

## Restricted Access

The project scope is simple. Create a landing page, that would prompt a user for a password. By entering the correct password the user would be taken to the page with the restricted content. As an added security measure, ensure that if someone has access to the second URL, if they have not passed security, they are to be forced to the landing page with the log in. Last, for a bonus, if they have passed security and return to the landing page, simply move them onto the restricted content page.

On the surface, this all seems pretty simple. But, doing this all with JavaScript, there are additional problems that we need to solve. To make this simple, we will have the password in the code. But, if someone looks at the code, how do we keep them from seeing the password? Same thing goes for the URL a user will be redirected to. How do we keep a user from simply reading the code and finding the secret URL?

## The solution

The solution to this is actually pretty simple, but at the same time, really complex. I will set a password that will challenge the user, and upon success the code will set a cookie and redirect the user to the URL with the restricted content. On the restricted content page, I will have a cookie check to see if the user has passed through the restriction gate, if not, then return the user back to the original landing page.

Like I said, this is not rock-solid security, but doing some simple things will make this really hard for a basic hacker to gain access.

###
