# IDOR Testing

## Public or Private?

Likely public:

```
GET /api/products/<product_id>  
GET /api/reviews/<product_id>
```

Likely Private:

```
GET /api/users/<user_id>/creditcard/details
GET /api/users/<user_id>/mailingaddress/details
```

## Find patterns in API route naming to discover new endpoints

Examples:

```
GET /api/albums/<album_id>/photos/<photo_id>
GET /api/posts/<post_id>/comment/…
GET /api/users/<user_id>/details/… 
GET /service/v1/users/<user_id>
```

Helpful tools to enumerate and/or fuzz routes:
- Burp Suite Intruder
- FFUF
- CeWL

## Add IDs to requests that don’t have them

Example:

`GET /api/MyPictureList` → `/api/MyPictureList?user_id=<other_user_id>`

## Try replacing parameter names

Example:

`GET /api/albums?album_id=<album id>` -> `GET /api/albums?account_id=<account id>`

Helpful tools to enumerate parameters in use by application:
- Burp extension: Paramalyzer 
- Burp extension: Param-miner 

## Supply multiple values for the same parameter

Example:

`GET /api/account?id=<your account id>` → `/api/account?id=<your account id>&id=<admin's account id>`

## Try changing the HTTP request method when testing for IDORs

Try switching POST and PUT and see if you can upload something to another user’s profile. For RESTful services, try changing GET to POST/PUT/DELETE to discover create/update/delete actions.

## Try changing the request’s content type

```
POST /api/chat/join/123
[…]
Content-type: application/xml → Content-type: application/json

<user>test</user>             → {“user”: “test”}
```

## Are credit card details being saved on a website?

When making a second purchase, the client may just be sending a credit card **ID number** instead of the actual card details themselves. Try looking for and changing this credit card ID number.

## Adding someone to a chat?

Alarm bells! Pay close attention to these types of actions as they are often where you can find IDORs. Can add yourself to another chat simply by changing values?

## CRUD

Similarly, if there is a copy, move or delete function on the web app then this is a good place to try looking for IDORs! Try testing all CRUD functions (Create, Read, Update, Delete) on every endpoint.

## Try changing the requested file type

This one is simple; it is not very common but sometimes requesting a different file type or extension may be enough to bypass the access control. Experiment by appending different file extensions (e.g. _.json, .xml, .config_) to the end of requests that reference a document.

## Does the app ask for non-numeric IDs? Use numeric IDs instead

Example:

```
username=user1 → username=1234
account_id=7541A92F-0101-4D1E-BBB0-EB5032FE1686 → account_id=5678
album_id=MyPictures → album_id=12
```

## Try using an array

Example:

`{“id”:19}` → `{“id”:[19]}`

## Wildcard ID

Example:

`GET /api/users/<user_id>/` → `GET /api/users/*`

## Error message?

Do not despair. Sometimes applications will throw an error message regardless of if the action you attempted was successful or not. Even if you get an error message, check manually to see if the action worked.

## Continuity

Make sure if you are replacing one ID with another that you replace it in **every part** of the request, not just in one instance. This is an easy mistake to make and could cause you to overlook potential vulnerabilities.

## Random or encrypted ID?

These can be tricky and daunting at first. The best option is to try to decode it by looking at a few different IDs. This may reveal that only a small part of the ID is changing in each instance. Knowing this may help you narrow down the link between the decrypted input and the encrypted output.

## Try other object IDs if you receive access errors

If you receive an “access denied” response from the server such as a 401 Unauthorized, it’s worth trying other object identifiers to make sure the access controls are uniformly applied. You can use something like [Burp Suite Intruder](https://portswigger.net/burp/documentation/desktop/tools/intruder/using) to quickly enumerate identifiers.


Source: <https://www.aon.com/cyber-solutions/aon_cyber_labs/finding-more-idors-tips-and-tricks/>