# SQL injection

## Detection:

1) ``'`` this might trigger some errors
2) Boolean conditions such as ``OR 1=1`` ans ``OR 1=2`` may trigger specific responce
3) Time based SQLI payloads and look at the time diffrence for each request.
4) OAST(Out-of-bands Application Security Testing) 
    1. [Testing out OAST](https://chanukyapl.medium.com/my-way-of-testing-sql-injection-part-2-oast-testing-5c8167fd3199) 
    2. [What is OAST](https://portswigger.net/blog/oast-out-of-band-application-security-testing)
    3. [How OAST works](https://portswigger.net/burp/application-security-testing/oast)


## Possible Cause

Most SQL injection vulnerabilities occur within the ``WHERE`` clause of a ``SELECT`` query. Experienced testers are familiar with this type of SQL injection.

## Retrieving hidden data

Consider the following example:

On a online store when the user clicks on the **gifts** category

``https://insecure-website.com/products?category=Gifts``

Suppose this makes the application to make such a query:
>SELECT * FROM products WHERE category = 'Gifts' AND released = 1

Suppose we manipulate the query in such a way that it becomes as follows:

``SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1``

Here ``--`` is a commentor thus the ``AND`` part of the query is ignored and thus dumping all the products.

Simillar type of attack:

``SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1``

Here with OR 1=1 boolean expression teh result is alwasy true. Thus dumping all the containts

### **NOTE:**
> This OR 1=1' may look harmless in this context.
>
> But if this every reaches ``UPDATE`` or ``DELETE`` this may cause in data loss or maybe deleating of the entire database.

## Exploiting Application Logic

Imagine a query uses the following logic to authenticate the user

>SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'

Now we can exploit this with the following

``Username: admin'--``

``Password: [anythingiwant]``

This results in 
>SELECT * FROM users WHERE username = 'administrator'--' AND password = ''

Inshort authenticating us as the admin. 

## SQL injection UNION attacks

``UNION`` keyword is used to retreive data from other tables within the same database.

This eables you to use one or more additional ``SELECT`` queries and append the result to the original query.

eg:
>SELECT a, b FROM table1 UNION SELECT c, d FROM table2

Thus returning ``a`` and ``b`` from ``table1`` and ``c`` and ``d`` from ``table2``

## Requirements for UNION attack to work:

1. The individual queries must return the same number of columns.
2. The data types in each column must be compatible between the individual queries.

>How many columns are being returned from the original query.
>
>Which columns returned from the original query are of a suitable data type to hold the results from the injected query.

