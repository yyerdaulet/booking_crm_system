# AUTH ENTITIES

```
┌────────────────────────────┐
│         USER ENTITY        │
└────────────────────────────┘

id

email
passwordHash
emailVerified
verificationToken

fullName
birthDate
city
gender
role(CLIENT/MANAGER/ADMIN)

createdAt
updatedAt
```



# FIELD NOTES

```
email
- unique
- normalized
- required

passwordHash
- hashed with argon2/bcrypt
- never store plain password

emailVerified
- boolean
- default = false

verificationToken
- used for email verification
- nullable
- regenerated when resending verification email
- invalidated after successful verification
```