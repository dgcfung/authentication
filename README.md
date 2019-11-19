# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)  SOFTWARE ENGINEERING IMMERSIVE

# Authentication

![](user-friendly.gif)

## Prerequisites
- Express & React CRUD

## Learning Objectives
By the end of this, students should be able to:
- Understand & Implement Authentication

## Framing (5min)
Turn & Talk with your neighbor and answer the following:
- What is Authentication?
- What is Authorization

**Bonus**: What is the difference between hashing vs encrypting?


## Introduction

### Authentication

What is Authentication? Well it's the hardest topic in our curriculum. Ok joke aside, what is Auth?

We have learned how to create full-stack applications. There is a problem though. Often we want to limit what a user has access to. So how do we handle this? Welcome.. Authentication.

Authentication proves that the user is who he or she says they are. And with that information we can "authorize" the user to access whatever resources we would like to allow them to access.

The concept is a user can sign up and sign in aka authenticate. Then for specific resources we can check if they're authenticated - if they are, then allow them to interact with the resource otherwise, tell them they do not have access.

Cool so what does this look like?

## Topics

### Authentication
Authentication is the ability to give users an identity that we can track.
- Authentication is who the user is
- We can use a JWT Token as a means of authentication
### Authorization
When we authenticate a user, we then either authorize them to access everything in our web application or only certain parts. For example, an Admin, typically, is authorized to have access to all resources. Authorization is giving users priviledges to whatever resources we define.
- Authorization is (now that we know who the user is - Authentication) what the user can do

A JWT Token can be a "claim" of who the user is (Authentication) and what the user can do (Authorization)
JWT Tokens should be "signed" so we can can verify the authenticity of the JWT Token.
### Hashing
Hashing is a one-way function that uses an algorithm to scramble text into a unique digest. It is common practice to hash passwords and store them in a database, rather than store plaintext passwords in databases, that way, if the database is compromised a hacker would gain access to hashed passwords and not the plaintext password (which in all likelihood is used as a password for other websites!)
### Encrypting
Encryption is a two-way function that can encrypt or decrypt via a unique key.
### Salt Rounds
A Salt Round is a random character or characters that is added to the hashing process to increase the complexity of the hash and make it virtually impossible to crack.
### [JSON Web Token or JWT (pronounced JOT)](https://jwt.io/introduction)
JWT is what we use to verify that the user is who they say they are.

JWT is URL safe encoded data that can be used to verify authenticity. It's a key - we define what we can "open" with that key.
### JWT Token Key or Secret
The JWT Secret is what we use to sign the JWT Token and make it unique. The idea is that we generate a unique JWT Token with the JWT Secret, we then send that token to a client as a way to give that client some identity (that identity may come with some privileges). The client includes the JWT Token with every request as "proof" as to who they are. The server checks if that JWT Token is a valid token by checking it against the JWT Secret.
### Payload
The Payload is whatever data we choose to send within the JWT Token. Keep in mind this data can be seen by anyone who obtains the JWT Token so refrain from adding sensitive information e.g. passwords.
> The Payload is typically where we set the "cliams" for the user, or in other words, who the user is and what they're allowed to do
### JWT Signing
JWT Signing is when we construct the JWT Token. It is when we take the JWT Header and Payload and generate the Signature.
> Typically, we sign the Header, Payload, and JWT Secret using an HMAC SH-256 algorithm

### JWT Verify
When we verify the authenticity of the JWT Token we typically check for three things: 
- It was created by you (by verifying the signature, using the secret signing key)
- It hasn't been modified (e.g. some claims were maliciously added)
- It hasn't expired
- It is active

## Sign Up

![](signUp.png)

1. User fills out sign up form on the react app
2. User clicks submit, an axios POST request with a user object stored in the body of the request is created and sent to the `/signup` endpoint on the express server
3. We de-structure the request body - pulling out `username`, `email`, and `password`. We now create a `password_digest` by using `bcrypt`'s `hash` method to **encrypt** the user's `password` with a `SALT_ROUND` of 11
4. We can now create the user with the password digest and store that user in our database
5. Using a `TOKEN_KEY` we create a [JWT library](https://jwt.io) Token with a `payload` set to the user's credentials (`id`, `username`, `email`)
6. The Express server responds with the newly created `token` and `user` object
7. Our React app takes the `token` and stores it in [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) so the user can remain logged in even when we reload the webpage


## Sign In

![](signIn.png)

1. The user fills out the sign in form on the react app
2. User clicks submit, an axios POST request with a user object stored in the body of the request is created and sent to the `/signin` endpoint on the express server
3. We de-structure the request body - pulling out `username` and `password`. We use the `username` to find that specific user in our database to grab the `password_digest` for that user
4. We user `bcrypt.compare` method to `hash` the `password` and compare it with the `password_digest` - if they match, then that user is who they say they are
5. We create a JWT token and send it back along wuth the user as a response

## Accessing a Protected Resource

![](protected.png)

1. The user fills out the update form to edit a specific item
2. In our React app we check if there is a token in localStorage
3. We grab the token in localStorage and construct an axios PUT request with the token in the `header` of the request and the `item` object (with the modifications) in the body of the request
4. On our Express server we parse the `header` to grabe the token. We verify if the token is the original token using the `jwt.verify` method and the unique `TOKEN_KEY`
5. If the token is legit, we make the update in the database
6. We respond with the updated item

## Conclusion

Authentication is found in nearly every application we interact with. In this lesson we learned how to handroll our own authentication system. This has given us a thorough understanding of how authentication works. However, because authentication is what protects our application from unauthorized access it is crucial we use highly secure authentication system. We highly recommend, in real-world applications to use industry vetted third-party authentication libraries.

✊ **Fist to Five**

-- Happy Coding :)

## Feedback

> [Take a minute to give us feedback on this lesson so we can improve it!](https://forms.gle/vgUoXbzxPWf4oPCX6)
