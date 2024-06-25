
Tags: #jwt #unverifiedsignature #jwtunverifiedsignature

JWT contains three parts.
Header, Payload & Signature.

When the server does not verify the signature we can modify the jwt payload and impersonate any user.

<mark style="background: #FF5582A6;">Reason</mark>: developer uses only decode() functions thinking this alone verifies as well.  developer should use verify() function to verify the signature and decode() to read the contents.

![[Pasted image 20240623172103.png]]
we have logged in as wiener:peter user.
we can see the jwt token in Cookie.
let's throw this in jwt debugger like [token.dev](https://token.dev) 
![[Pasted image 20240623172254.png]]

we can see the payload content. 
we can try to change the **sub** key value from wiener to administrator.
let's modify this
![[Pasted image 20240623172347.png]]
we have modified the payload.
let's copy this jwt token and put it in our session cookie.
![[Pasted image 20240623172429.png]]
hit the save icon and request myaccount
![[Pasted image 20240623172456.png]]
we got access to admin panel.
![[Pasted image 20240623172510.png]]



