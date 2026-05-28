# AUTH FLOW

```
┌──────────────────────┐
│   POST /register     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Create User          │
│ Email Verified=false │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Send Verify Email    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ POST /email/verify   │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Email Verified=true  │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│    POST /login       │
└──────────┬───────────┘
           │
           ▼
┌───────────────────────────────┐
│ Generate Tokens               │
│                               │
│ Access Token  (15 min)        │
│ Refresh Token (30 days)       │
└──────────┬────────────────────┘
           │
           ▼
     ┌───────────────┐
     │ Client Stores │
     └──────┬────────┘
            │
            ├──────────────────────┐
            │                      │
            ▼                      ▼

┌───────────────────┐     ┌────────────────────┐
│ Access Token      │     │ Refresh Token      │
│ Short-lived JWT   │     │ Long-lived Session │
└─────────┬─────────┘     └─────────┬──────────┘
          │                         │
          ▼                         │
┌──────────────────────┐            │
│ API Requests         │            │
│ Authorization Bearer │            │
└──────────┬───────────┘            │
           │                        │
           ▼                        │
┌──────────────────────┐            │
│ Verify JWT Signature │            │
│ Extract User ID      │            │
└──────────┬───────────┘            │
           │                        │
           ▼                        │
┌──────────────────────┐            │
│ Protected Resource   │            │
│ /appointments        │            │
└──────────────────────┘            │
                                    │
                                    ▼
                        ┌──────────────────────┐
                        │ Access Token Expired │
                        └──────────┬───────────┘
                                   │
                                   ▼
                        ┌──────────────────────┐
                        │ POST /auth/refresh   │
                        └──────────┬───────────┘
                                   │
                                   ▼
                        ┌──────────────────────┐
                        │ Validate Refresh     │
                        │ Token & Session      │
                        └──────────┬───────────┘
                                   │
                                   ▼
                        ┌──────────────────────┐
                        │ Generate NEW Tokens  │
                        │ Rotate Refresh Token │
                        └──────────┬───────────┘
                                   │
                                   ▼
                        ┌──────────────────────┐
                        │ Continue Using API   │
                        └──────────────────────┘



                LOGOUT FLOW

┌──────────────────────┐
│ POST /auth/logout    │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Revoke Refresh Token │
│ Delete Session       │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ User Logged Out      │
└──────────────────────┘



             PASSWORD RESET FLOW

┌────────────────────────────┐
│ POST /auth/password/forgot │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Generate Reset Token       │
│ Send Reset Email           │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ User Opens Reset Link      │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ POST /auth/password/reset  │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Validate Reset Token       │
│ Hash New Password          │
│ Invalidate Old Sessions    │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Password Updated           │
│ User Must Login Again      │
└────────────────────────────┘
```

# EMAIL VERIFICATION + RESEND FLOW

```
┌────────────────────────────┐
│     POST /auth/register    │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Create User                │
│ emailVerified = false      │
│ generate verificationToken │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Send Verification Email    │
│ (token included)           │
└──────────────┬─────────────┘
               │
               ▼
        USER IGNORES EMAIL
               │
               ▼

┌────────────────────────────┐
│ POST /auth/email/resend    │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────────────┐
│ Check user by email               │
│ If already verified → STOP        │
│ Generate NEW verificationToken    │
│ Replace old token                 │
└──────────────┬─────────────────────┘
               │
               ▼
┌────────────────────────────┐
│ Send NEW verification email │
└──────────────┬─────────────┘
               │
               ▼

┌────────────────────────────┐
│ POST /auth/email/verify    │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────────────┐
│ Validate verificationToken         │
│ Match user                         │
└──────────────┬─────────────────────┘
               │
               ▼
┌────────────────────────────┐
│ emailVerified = true       │
│ verificationToken = null    │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ EMAIL VERIFIED SUCCESS     │
└────────────────────────────┘
```


# PASSWORD CHANGE FLOW (AUTHENTICATED USER)

```text
┌──────────────────────────────┐
│ POST /auth/login             │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ User authenticated           │
│ Access Token issued          │
└──────────────┬───────────────┘
               │
               ▼
        USER IS LOGGED IN
               │
               ▼

┌──────────────────────────────┐
│ POST /auth/password/change   │
│ Authorization: Bearer TOKEN  │
└──────────────┬───────────────┘
               │
               ▼
┌────────────────────────────────────────┐
│ Validate access token                  │
│ Extract userId                        │
│ Verify current password               │
└──────────────┬─────────────────────────┘
               │
               ▼
┌────────────────────────────────────────┐
│ Validate new password rules           │
│ (length, complexity, etc.)            │
└──────────────┬─────────────────────────┘
               │
               ▼
┌────────────────────────────────────────┐
│ Hash new password                     │
│ Update passwordHash in DB            │
└──────────────┬─────────────────────────┘
               │
               ▼
┌────────────────────────────────────────┐
│ Invalidate all refresh tokens        │
│ (force re-login on all devices)      │
└──────────────┬─────────────────────────┘
               │
               ▼
┌────────────────────────────────────────┐
│ Optional: send security email         │
│ "Password changed successfully"       │
└──────────────┬─────────────────────────┘
               │
               ▼
┌────────────────────────────────────────┐
│ RESPONSE: SUCCESS                     │
└────────────────────────────────────────┘
```
