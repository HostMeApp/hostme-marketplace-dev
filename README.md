# Building a custom application for Hostme marketplace

Hostme supports two types of custom build applications. 
1.  Private app for one or more locations developer team has acceess to. This is good for developers who want to build an app for specific restaurant or restaurant's group
2.  Publicly available application published to Hostme marketplace. This application could be free of charge or fixed monthly fee or pay per use

It's recommended to start byulding an app as a private app and when it's ready and reviewed by the Hostme team pack and publish it as a public app.

## Starting with sandbox environment

1. Please follow this link to setup new sandbox account: [https://hostme-panel-qa.azurewebsites.net/signup](https://hostme-panel-qa.azurewebsites.net/signup)
2. Install Custom Application app from marketplace
3. Open Custom Application settings and set webhook URL if you want to receive events
4. Copy and save application connection identifier (AppConnectionId) - you need it to request app token in net step. You can find it in the URL when app settings are open. [https://hostme-panel-qa.azurewebsites.net/admin/#/restaurant/[RestaurantId]/integrations/marketplace/**[AppConnectionId]**/settings]

#### Get vendor app token for a private app:
```
POST https://hostme-services-qa.azurewebsites.net/token
type:  x-www-form-urlencoded
grant_type:client_credentials
client_secret:{AppConnectionId}
client_id:customapp  (public app will have its own unique client_id)
```

# Hostme Vendor API Swagger

Here is a link to the full API swagger: https://hostme-services-qa.azurewebsites.net/swagger/index.html

## Some request examples

### List restaurant guest book
```
GET https://hostme-services-qa.azurewebsites.net/api/loyalty/admin/restaurants/6591/members?$top=50&$skip=10
Authorization: Bearer Token

Query Params:
$top - number of records in each page
$skip - number of records to skip
```

### Get restaurant availability
```
GET https://hostme-services-qa.azurewebsites.net/api/rsv/admin/restaurants/6591/availability?date=2024-06-21T12:00:00-07:00&areas=outside,inside&groupSize=4&rangeInMinutes=720&asOnlineGuest=true

Query Params
date: Date and time for requested availability
areas: comma separated list of seating areas to include
groupSize: number of guests
rangeInMinutes: time window in minutes around [date] for availability lookup. 
asOnlineGuest: specifies that online availability settings should be used.
```
### Create new booking
```
POST https://hostme-services-qa.azurewebsites.net/api/rsv/admin/restaurants/6591/reservations
Body:

{
  "reservationTime": "2024-06-21T12:00:00-07:00",
  "groupSize": 4,
  "areas": "outside,inside",
  "tableNumber": "1",
  "tablesSizes": 4,
  "customerName": "evgeny popov",
  "additionals": {
    "babyChair": true,
    "stroller": true,
    "eventTypes": [ " graduation" ]
  },
  "phone": "+1 703-303-7416",
  "note": "",
  "stroller": false,
  "babyChair": false,
  "aboutGuestNotes": "IMPORTANT!: [this is dine-in pre-order for group at table D-24 ] ",
  "customerProfile": {
    "allergy": [ " seafood", " strawberry" ],
    "vegetarian": true,
    "vip": false,
    "handicapAccessible": false,
    "companyName": "1",
    "tablePreferences": "table preferences"
  },
  "email": "evgeny.popov@gmail.com",
  "additionalInfo": "",
  "internalNotes": "Internal booking note",
  "specialRequests": "guest reservation note",
  "restaurantId": "6591",
  "id": "e934acd4d4484bf884fe3b62a66ae6c8"
}

```
