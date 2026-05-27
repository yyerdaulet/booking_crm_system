# CRM Endpoints For Backend 
___

## Security Endpoints: 

```
POST /register 
POST /login 
POST /verify 
GET  /check 
GET  /me
```
___

## Business Logic Endpoints

### ROLE : Client

```
GET /locations
GET /locations/{locationId}
```

```
GET /services
GET /services/{serviceId}
GET /service?locationId=1
```

```
GET /masters
GET /masters/{masterId}
GET /master?locationId=1&serviceId=2
```