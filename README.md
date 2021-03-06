# Welcome to Webstep Fokus ASP.NET Core Fagdag!
This readme contains information for the ASP.NET Core WebApi hands-on exercise. 

### Your mission should you choose to accept it
The goal of this hands-on exercise is to implement a REST-based API for the booking department of the Forkus Hotel. 
This document will provide a specification of API calls with expected results and it will be up to you to implement them. 
You can then use tests to test if your implementation follows the spec. 

### Testing
The solution includes a test-project for the API. The test suite is not complete for all the assignments.
Run this either through the *Visual Studio Test Explorer* or from the command-line using:
``` cmd
dotnet test
```
Note: current directory must be in the integration-tests project folder for the command to work.

### Postman testing
There is a postman collection "postman_collection.json" at the root of the repo that has pre-defined requests for the same scenarios as the integration tests.

### Suggested solution
The "ForkusHotel.Api.Solution"-folder contains suggested solutions for the first part of the assignment. It is not complete. Tests and postman-collection should work as specified with the solution project.

## Specification
What follows is the specification for the API. 
To keep it simple don't worry about JSON serialization details (like dates). These are up to your implementation

### Retrieve all room types
#### Request
* Route: /api/booking/roomtypes
* Method: Get

#### Response
* Status: 200 - Ok
* JSON Body: 
``` json
{ "roomTypes" : [ 
    "Single", "Double", "Twin", 
    "DeluxeDouble", "JuniorSuite", "Suite", 
    "ForkusSuite"
]}
```


### Book a room
##### Request
* Route: /api/booking/bookings
* Method: Post
* JSON Body(example):
``` json
{ 
    "roomType" : "ForkusSuite",
    "startDate" : "2016-10-21T13:28:06.419Z",
    "numberOfNights" : "3",
    "guestName" : "Kjell Lj0stad"
}
```

##### Response on success: 
* Status: 201 - Created
* Location header: /api/booking/bookings/\{newBookingId\}
* JSON Body(example):
``` json
{ "bookingId" : "{newBookingId}" }
```

##### Response if roomtype is unknown
* Status: 400 - Bad request
* JSON Body:
``` json
{ "error" : "Unknown room-type specified" }
```

##### Response if time period is invalid
* Status: 400 - Bad request
* JSON Body:
``` json
{ "error" : "Specified time period is invalid" }
```

##### Response if specified roomtype is not available for time period
* Staus: 409 - Conflict


### Retrieve list of all bookings
* Route: /api/booking/bookings
* Method: Get

##### Response on success: 
* Status: 200 - Ok
* JSON Body(example):
``` json
{ 
    "bookings" : [ 
    {
        "bookingId" : "2B121B48-FD3D-4E14-89E9-11EC009F57C7",
        "roomType" : "ForkusSuite",
        "startDate" : "2016-10-21T13:28:06.419Z",
        "numberOfNights" : "3",
        "guestName" : "Rupert Giles"
    },
    {
        "bookingId" : "02841435-1A78-4EFD-8581-1BB8A0B098A9",
        "roomType" : "JuniorSuite",
        "startDate" : "2016-10-21T13:28:06.419Z",
        "numberOfNights" : "2",
        "guestName" : "Jean-Luc Picard" 
    }]
}
```


### Retrieve details for a booking
* Route: /api/booking/bookings/{bookingId}
* Method: Get

##### Response on success: 
* Status: 200 - Ok
* JSON Body(example):
``` json
{ 
    "roomType" : "ForkusSuite",
    "startDate" : "2016-10-21T13:28:06.419Z",
    "numberOfNights" : "3",
    "guestName" : "Kjell Lj0stad",
    "paymentConfirmed" : false,
    "checkedIn" : true,
    "checkedOut" : false
}
```

##### Response if bookingId is unknown
* Status: 404 - Not found


### Cancel a booking
* Route: /api/booking/bookings/\{bookingId\}
* Method: Delete

##### Response on success: 
* Status: 204 - No Content

##### Response if bookingId is unknown
* Status: 404 - Not found


### Change the roomtype of a booking
* Route: /api/booking/bookings/\{bookingId\}/roomtype
* Method: Put
* JSON Body(example):
``` json
{ "newRoomType" : "ForkusSuite" }
```
##### Response on success: 
* Status: 204 - No content

##### Response if bookingId is unknown
* Status: 404 - Not found

##### Response if roomtype is unknown
* Status: 404 - Not found
* JSON Body:
``` json
{ "error" : "Unknown room-type" }
```

### Checking in
* Route: /api/booking/bookings/\{bookingId\}/checkin
* Method: Put

##### Response on success: 
* Status: 204 - No content 

##### Response if bookingId is unknown
* Status: 404 - Not found

##### Response if already checked in 
* Status: 404 - Not found
* JSON Body:
``` json
{ "error" : "Already checked in" }
```

### Checking out
* Route: /api/booking/bookings/\{bookingId\}/checkout
* Method: Put

##### Response on success: 
* Status: 204 - No content 

##### Response if bookingId is unknown 
* Status: 404 - Not found

##### Response if already checked out 
* Status: 404 - Not found
* JSON Body:
``` json
{ "error" : "Already checked out" }
```

Happy coding!
