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

