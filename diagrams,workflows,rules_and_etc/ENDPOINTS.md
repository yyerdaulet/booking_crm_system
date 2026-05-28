# CRM ENDPOINTS FOR BACKEND
___

## SECURITY ENDPOINTS

```
POST /auth/register 
POST /auth/login 
POST /auth/email/verify
POST /auth/email/resend
POST /auth/refresh
POST /auth/logout
POST /auth/password/forgot

GET  /auth/email/verified
GET  /auth/me
```
___

## BUSINESS LOGIC ENDPOINTS

### ROLE : CLIENT

```
GET /locations
GET /locations/{locationId}
```

```
GET /services
GET /services/{serviceId}
GET /services?locationId=1
```

```
GET /masters
GET /masters/{masterId}
GET /masters?locationId=1&serviceId=2
```

```
GET /slots?masterId=1&date=2026-05-30
```

```
GET /appointments/{appointmentId}
POST /appointments
```

### ROLE : MANAGER

```
GET /locations
GET /locations/{locationId}

POST /locations

PUT /locations/{locationId}

DELETE /locations/{locationId}
```

```
GET /services
GET /services?locationId=1
GET /services/{serviceId}

POST /services

PUT /services/{serviceId}

DELETE /services/{serviceId}
```

```
GET /masters
GET /masters/{masterId}
GET /masters?locationId=1&serviceId=2

POST /masters

PUT /masters/{masterId}

DELETE /masters/{masterId}
```

```
GET /slots?masterId=1&date=2026-05-30
GET /slots
GET /slots/{slotId}

PATCH /slots/{slotId}
```

### ROLE : ADMIN

```

```

--- 

## NOTIFICATION SERVICE ENDPOINTS

