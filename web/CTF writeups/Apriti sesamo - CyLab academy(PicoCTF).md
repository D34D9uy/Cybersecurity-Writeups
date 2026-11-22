# Apriti sesamo

- **Platform:** CyLab academy(PicoCTF)
- **Category:** Web
- **Difficulty:** Medium

## Challenge

I found a web app that claims to be impossible to hack!

## Solution

1.After accessing the website, we can see a login page, after trying the usual sql injection payloads, we were met with little success. After some searching, we can see in the backup of the page(adding `~` to the end of the url) is the logic of verifying the credentials. It takes in the user name and password and it requires that`SHA1(username)===SHA1(password) && username !== password`.

2. PHP fun fact: When`SHA1()` takes in an array, it will always return `false` with a warning. We can exploit this feature by passing in two arrays with different values to satisfy the requirements. So we can use the burp suite repeater to modify our request:
`username=b&pwd=a`
to:
`username[]=b&pwd[]=a`
and we can get the flag in our response. 

## Flag

`academy{w3Ll_d3sErV3d_Ch4mp_4b876558}`

## Key Takeaway

This challenge demonstrates the importance of input type validation in PHP. Applications should validate that user-supplied values have the expected types before processing them.

In this case, the application expected strings but accepted arrays, allowing PHP's handling of invalid arguments to bypass the authentication logic. Authentication should also use appropriate password-hashing mechanisms, such as PHP's password_hash() and password_verify(), rather than comparing SHA-1 hashes.

## References

- [https://learn.cylabacademy.org/library/467]
