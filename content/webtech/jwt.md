---
draft: true
title: Jwt
categories:
  - Webtech
---
This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA.

HMAC

JSON Web Tokens consist of three parts separated by dots (.), which are:
 •	Header
 •	Payload
 •	Signature

Header

{
  "alg": "HS256",
  "typ": "JWT"
}

{“alg”:”RS256",”kid”:”78b4cf23656dc395364f1b6c02907691f2cdffe1"}

This first part(once parted by the periods) of the JWT is known as the JOSE header . JOSE stands for Javascript Object Signing and Encryption 

- base64URL encoded

payload

signature = HMACSHA256(base64encoded(header).base64encoded(body))

—

 In OpenID Connect the id_token is represented as a JWT. 
 
 A signed JWT is known as a JWS (JSON Web Signature). In fact a JWT does not exist itself — either it has to be a JWS or a JWE (JSON Web Encryption). 
 
 The value of the kid element provides an indication or a hint about the key, which is used to sign the message. Looking at the kid, the recipient of the message should know where and how to lookup for the key and find it.


——

JWS Compact Serialization
This compact string has three main elements separated by periods (.): the JOSE header, the JWS payload and the JWS signature.
