1. In Session-based Authentication the Server does all the heavy lifting server-side. Broadly speaking a client authenticates with its credentials and receives a session\_id (which can be stored in a cookie) and attaches this to every subsequent outgoing request. So this could be considered a "token" as it is the equivalent of a set of credentials. There is however nothing fancy about this session\_id-String. It is just an identifier and the server does everything else. It is stateful. It associates the identifier with a user account (e.g. in memory or in a database). It can restrict or limit this session to certain operations or a certain time period and can invalidate it if there are security concern and more importantly it can do and change all of this on the fly. Furthermore it can log the users every move on the website(s). Possible disadvantages are bad scale-ability (especially over more than one server farm) and extensive memory usage.
2. In Token-based Authentication no session is persisted server-side (stateless). The initial steps are the same. Credentials are exchanged against a token which is then attached to every subsequent request (It can also be stored in a cookie). However for the purpose of decreasing memory usage, easy scale-ability and total flexibility (tokens can be exchanged with another client) a string with all the necessary information is issued (the token) which is checked after each request made by the client to the server. There are a number of ways to use/ create tokens:
a) using a hash mechanism e.g. HMAC-SHA1

token = user\_id|expiry\_date|HMAC(user\_id|expiry\_date, k)
--id and expiry\_id are sent in plaintext with the resulting hash attached (k is only know to the server)

b) encrypting the token symmetrically e.g. with AES

token = AES(user\_id|expiry\_date, x)
--x represents the en-/decryption key

c) encrypting it asymmetrically e.g. with RSA

token = RSA(user\_id|expiry\_date, private key)
Productive systems are usually more complex than those two archetypes. Amazon for example uses both mechanisms on its website. Also hybrids can be used to issue tokens as described in 2 and also associate a user session with it for user tracking or possible revocation and still retain the client flexibility of classic tokens. Also OAuth 2.0 uses short-lived and specific bearer-tokens and longer-lived refresh tokens e.g. to get bearer-tokens.


from https://security.stackexchange.com/questions/81756/session-authentication-vs-token-authentication
