# Cross-Site Scripting (XSS)

> A beginner-friendly guide to understanding XSS, its types, impact, prevention methods, and the role of Web Application Firewalls (WAFs).

---

## Table of Contents

- What is XSS?
- How XSS Happens
- Types of XSS
  - Reflected XSS
  - Stored XSS
  - DOM-Based XSS
- Common XSS Contexts
- Impact of XSS
- Prevention Techniques
- What is a WAF?
- Types of WAFs
- WAF Limitations
- Conclusion

---

# What is XSS?

Cross-Site Scripting (XSS) is a web security vulnerability that occurs when an application includes untrusted data in a web page without properly handling it.

As a result, a browser may interpret user-supplied content as active code instead of plain text.

---

# How XSS Happens

The vulnerability typically follows this flow:

```text
User Input
    ↓
Application Processes Input
    ↓
Input Returned to Browser
    ↓
Browser Interprets Content
```

If the application fails to properly handle user input, unintended code execution may occur.

---

# Types of XSS

## 1. Reflected XSS

Reflected XSS occurs when input is immediately returned in the application's response.

### Example Scenario

A search page displays the search term back to the user.

```html
<p>You searched for: USER_INPUT</p>
```

If the application does not properly handle the input, the browser may interpret it in an unsafe manner.

### Characteristics

- Requires user interaction
- Usually delivered through URLs
- Common in search pages

---

## 2. Stored XSS

Stored XSS occurs when user input is saved and later displayed to other users.

### Example Scenario

```text
User Comment
    ↓
Database
    ↓
Website Visitors
```

Examples include:

- Blog comments
- Forum posts
- User profiles

### Characteristics

- Persistent
- Affects multiple users
- Usually higher impact

---

## 3. DOM-Based XSS

DOM XSS occurs when client-side JavaScript processes untrusted data and inserts it into the page in an unsafe way.

### Example Scenario

```javascript
const value = location.search;
```

JavaScript reads data directly from the URL and later displays it inside the page.

### Characteristics

- Client-side vulnerability
- Often related to JavaScript code
- Server may never process the input

---

# Common XSS Contexts

Understanding the context is critical.

## HTML Context

```html
<div>USER_INPUT</div>
```

## Attribute Context

```html
<input value="USER_INPUT">
```

## URL Context

```html
<a href="USER_INPUT">Link</a>
```

## JavaScript Context

```javascript
var data = "USER_INPUT";
```

Different contexts require different protection mechanisms.

---

# Impact of XSS

Potential consequences include:

- Session theft
- Account compromise
- Phishing attacks
- Content manipulation
- Unauthorized actions

---

# Prevention Techniques

## Input Validation

Validate incoming data whenever possible.

## Output Encoding

Encode output based on the rendering context.

## Safe DOM APIs

Prefer safer APIs when updating page content.

## Content Security Policy (CSP)

Use CSP to reduce the impact of injection vulnerabilities.

## Security Testing

Perform regular:

- Code reviews
- Penetration tests
- Security assessments

---

# What is a WAF?

A Web Application Firewall (WAF) is a security layer that filters and monitors HTTP traffic before it reaches the application.

```text
User
  ↓
WAF
  ↓
Web Application
```

---

# Types of WAF

## Network-Based WAF

Hardware or infrastructure-based protection.

## Host-Based WAF

Runs directly on the application server.

## Cloud-Based WAF

Managed by cloud providers.

Examples:

- Cloudflare
- AWS WAF
- Imperva

---

# WAF Limitations

A WAF improves security but does not eliminate vulnerabilities.

Organizations should still implement:

- Secure coding practices
- Proper encoding
- Input validation
- Security testing

A WAF should be considered an additional layer of defense rather than a replacement for secure development.

---

# Conclusion

XSS remains one of the most common web vulnerabilities.

Understanding:
- How it occurs
- The different types
- Common contexts
- Prevention techniques

is essential for anyone learning Web Security.

In addition, WAFs can provide valuable protection, but secure development practices remain the primary defense against XSS vulnerabilities.

---

# References

- PortSwigger Web Security Academy
- OWASP XSS Prevention Cheat Sheet
- MDN Web Docs
