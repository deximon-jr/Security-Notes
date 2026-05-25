# Broken Access Control (BAC)

> A practical beginner-friendly guide to understanding Broken Access Control, Privilege Escalation, common attack scenarios, and how these vulnerabilities appear in real web applications.

---

# Table of Contents

- What is Access Control?
- Why Do We Need Access Control?
- What is Broken Access Control?
- Why Does Broken Access Control Happen?
- Common Types of Broken Access Control
- What is Privilege Escalation?
- Types of Privilege Escalation
- Difference Between BAC and Privilege Escalation
- Real-World Examples
- Prevention

---

# What is Access Control?

Access Control is the process of deciding:

- Who can access a resource
- What actions they can perform
- What data they can view or modify

Every application has resources that should not be available to everyone.

For example:

| User | Allowed Action |
|--------|--------|
| Customer | View own profile |
| Customer | Edit own profile |
| Admin | View all users |
| Admin | Delete users |

The application must verify permissions before performing any action.

---

# Why Do We Need Access Control?

Without Access Control, every user could access anything.

Imagine a banking application.

A normal user should only see:

```http
GET /account/12345
```

their own account information.

If the application does not enforce access control properly, the user may be able to access:

```http
GET /account/54321
```

and view someone else's account.

This is where problems begin.

---

# What is Broken Access Control?

Broken Access Control occurs when an application fails to properly enforce authorization rules.

In simple words:

> The application trusts a user to do something they should not be allowed to do.

This may allow an attacker to:

- Access other users' data
- Modify information
- Delete resources
- Perform administrative actions
- Take control of accounts

Broken Access Control is one of the most common and dangerous web vulnerabilities.

---

# Why Does Broken Access Control Happen?

Developers sometimes make mistakes such as:

- Checking permissions only in the frontend
- Trusting user-supplied parameters
- Exposing hidden endpoints
- Missing authorization checks
- Using predictable object identifiers

For example:

```http
GET /profile?id=1001
```

A user changes the ID:

```http
GET /profile?id=1002
```

If the application returns another user's profile, access control is broken.

---

# Common Types of Broken Access Control

## 1. Insecure Direct Object Reference (IDOR)

IDOR happens when an application exposes internal object identifiers and does not verify ownership.

Example:

```http
GET /invoice/123
```

Changing it to:

```http
GET /invoice/124
```

may return another user's invoice.

The application should verify:

```text
Does this invoice belong to the current user?
```

If not, access should be denied.

---

## 2. Horizontal Access Control

A user accesses another user with the same privilege level.

Example:

User A:

```http
GET /profile/100
```

User B:

```http
GET /profile/101
```

User A changes the request:

```http
GET /profile/101
```

and successfully views User B's profile.

Both users have the same role.

This is called Horizontal Privilege Escalation.

---

## 3. Vertical Access Control

A low-privileged user gains access to higher-privileged functionality.

Example:

A normal user discovers:

```http
/admin/delete-user
```

and accesses it successfully.

A regular user should never have access to administrative functions.

---

## 4. Forced Browsing

Some applications hide pages from the interface but do not protect them.

Example:

The Admin button is hidden.

However:

```http
GET /admin
```

still works.

Security must be enforced on the server, not in the user interface.

---

## 5. Method-Based Access Control Bypass

Permissions may only be checked for certain HTTP methods.

Example:

Blocked request:

```http
POST /admin/delete-user
```

Allowed request:

```http
GET /admin/delete-user
```

or the opposite.

Changing the request method bypasses security controls.

---

# What is Privilege Escalation?

Privilege Escalation occurs when a user gains permissions they should not have.

The attacker moves from:

```text
Low Privilege
```

to

```text
Higher Privilege
```

This allows access to restricted resources or functionality.

---

# Types of Privilege Escalation

## Horizontal Privilege Escalation

Accessing another user's resources without increasing role level.

Example:

```text
User A -> User B's Data
```

Same privilege level.

Different account.

---

## Vertical Privilege Escalation

Moving from a lower role to a higher role.

Example:

```text
User -> Admin
```

This is usually more dangerous because administrative functionality becomes available.

---

## Context-Dependent Privilege Escalation

Permissions work correctly in some situations but fail in others.

Example:

An application checks permissions during Step 1:

```text
Verify User
```

but skips checks during Step 2:

```text
Approve Payment
```

An attacker directly accesses Step 2.

---

# Difference Between BAC and Privilege Escalation

Many beginners confuse these terms.

## Broken Access Control

Broken Access Control is the vulnerability itself.

Example:

```text
The application does not properly enforce permissions.
```

---

## Privilege Escalation

Privilege Escalation is often the result of exploiting Broken Access Control.

Example:

```text
BAC -> User accesses admin functionality
```

The escalation happened because access control was broken.

Think of it like this:

```text
Broken Access Control = Cause

Privilege Escalation = Result
```

---

# Real-World Examples

## Example 1: Changing User IDs

Request:

```http
GET /api/orders/100
```

Modified request:

```http
GET /api/orders/101
```

If another user's order is returned:

```text
IDOR
Horizontal Privilege Escalation
Broken Access Control
```

---

## Example 2: Accessing Hidden Admin Pages

Request:

```http
GET /admin
```

Application response:

```http
200 OK
```

for a normal user.

Result:

```text
Vertical Privilege Escalation
Broken Access Control
```

---

## Example 3: Changing Role Parameters

Original request:

```http
POST /update-profile

role=user
```

Modified request:

```http
POST /update-profile

role=admin
```

If the application trusts the parameter:

```text
Vertical Privilege Escalation
Broken Access Control
```

---

# Prevention

Developers can reduce the risk of BAC by:

- Enforcing authorization on the server side
- Never trusting client-side controls
- Validating ownership of resources
- Following the principle of least privilege
- Using secure role-based access control
- Performing authorization checks on every request
- Logging and monitoring sensitive actions

---

# Key Takeaways

- Access Control decides who can do what.
- Broken Access Control happens when authorization rules fail.
- IDOR is one of the most common BAC vulnerabilities.
- Privilege Escalation is often the result of exploiting BAC.
- Horizontal escalation targets users at the same privilege level.
- Vertical escalation targets higher privilege levels such as administrators.
- Security must always be enforced on the server side.

---

> One of the most important lessons in web security is simple:
>
> **Never trust the client. Always verify permissions on the server.**
