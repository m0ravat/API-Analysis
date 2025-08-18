# API-Analysis

## Rest API
### Format
The common way of fetching data is by sending a HTTP request to the server, alongside a URL which specifies the data and a resource identifier. An example would be:    
GET /user/:id where you would fill the :id with an id (string or number) and it gets that specific user, you could remove the /:id portion and it would typically fetch all users instead. This can get very messy, turning into something like the following for a social media post: GET /user/12/post/32/comments to fetch all comments. You can also add path parameters, so we could extend on the social media comment post by doing a GET /user/12/post/32/comments?likes_gt=30. The _gt stands for greater than to imply we want to fetch all comments with over 30 likes. 
### HTTP Request types
POST - Inputting/creating data.       
PUT - Updating data by replacing all fields.       
PATCH - Partially update a resource.      
HEAD - Retrieving headers for a resource like HTTP Status codes, Content-Type, Content-Length, Last-Modified etc.      
OPTIONS - Check what methods are supported.      
DELETE - Deleting data      
GET - Fetching data (can be used in frameworks like express to also tell the server to render a web page with said data).      
### Return Type
It often returns data in JSON format, but can also return HTML,CSV, XML making it very portable. 

### Pros
- Typically uses conventional notation so it's very readable, easy to figure out the purpose behind API routes.
- The HTTP requests are very broad and cover every aspect of CRUD making it compatible with many services.
- Flexible data formatting works well for APIs where you may want data in multiple formats.
- Supports caching
- Makes code more modular since the frontend only refers to the API endpoint when sending a HTTP request which is handled by the backend.

### Cons
- For complex/tedious queries it can become very long as seen in the above example where we try to fetch a number of comments under a social media post. We are referring to 3 different resources and using one parameter which becomes very lengthy. We could use a custom endpoint like GET /socialMediaPostsGT/30 or abbreviate it but risk readability as a result. 
- Since there is no defined science to defining API endpoints we can define them however we want which can be a con since it can lead to many producing messy code. NextJS for example handles routing automatically now so all you need to do is follow the folder/file naming conventions giving it a steeper learning curve but makes it easier on the user in the long run as opposed to using React Router.
- To interact with the backend you have to send a request, increasing the need for websockets as it can't automatically keep up with live changes.

## GraphQL

### Format 
As opposed to REST which sends a HTTP request the format of a GraphQL API is very similar to a database query, you define a schema, mutation and query. A query involves reading/fetching data while a mutation involves updating, creating or deleting data.  An example of these 3 being used in GraphQL SDL is:     
type User {    
   id: ID! (Note - The Exclamation mark is to say this data is mandatory)    
   name: String!   
   email: String!   
   doB: Date    
   job: Job!   
}    
input createUserInput {    
  name: String!    
  email: String!    
  doB: Date    
  job: Job!    
}    
type Job {    
   id: ID!     
   name: String!    
   desc: String      
}     
schema {     
   query: Query,    
   mutation: Mutation     
}        
type Query {   
   getAllUsers: [User]! (this is the return type - an array of users)    
   getUser(id: ID!) (this passes a parameter)     
}    
type Mutation {    
  createUser(input: createUserInput): User     
}      

In this, the schema tells us what queries and mutations can be made and we use types to ensure data integrity like in TypeScript.

### Pros
- You can fetch what data you need without overfetching or underfetching - Querying multiple tables for a select few attributes.
- Uses one endpoint like /graphql, posting the query we want to use.
- Supports real time updates through subscriptions.

### Cons
- Fields can be very long and tedious to wrap around with many inputs and types as seen above.
- With rest api frontend devs only worry about which endpoint to use, now they also need to worry about query/mutation structure. 
- Developers without a foundation in it's formatting may require more time to come to grips with this as opposed to a REST API.
- Requires more custom optimization.
- 
## SOAP API
Unlike RESTful APIs which are an architectural style SOAP is an API that follows the SOAP protocol.

### Format
Data is sent and received via XML format and has 4 layers:    
- Envelope: Defining the structure of the message.
- Encoding: Rules for expressing the type of data.
- Requests: How each SOAP API request is structured.
- Responses: How each SOAP API response is structured.

### Example 
A request may be given like this: 

POST /StudentService HTTP/1.1
Host: school.example.com
Content-Type: text/xml; charset=utf-8
SOAPAction: "CheckAttendance"

<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Header>
    <auth:Authentication xmlns:auth="http://school.example.com/auth">
      <auth:Username>admin</auth:Username>
      <auth:Password>secret123</auth:Password>
    </auth:Authentication>
  </soap:Header>
  <soap:Body>
    <ns:CheckAttendance xmlns:ns="http://school.example.com/student">
      <ns:StudentID>12345</ns:StudentID>
      <ns:Date>2025-08-18</ns:Date>
    </ns:CheckAttendance>
  </soap:Body>
</soap:Envelope>

The success response might look like this:    
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <ns:CheckAttendanceResponse xmlns:ns="http://school.example.com/student">
      <ns:StudentID>12345</ns:StudentID>
      <ns:Date>2025-08-18</ns:Date>
      <ns:Present>true</ns:Present>
    </ns:CheckAttendanceResponse>
  </soap:Body>
</soap:Envelope>

And the error may look like this:     
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <soap:Fault>
      <faultcode>soap:Client</faultcode>
      <faultstring>Invalid StudentID</faultstring>
      <detail>
        <ns:ErrorDetails xmlns:ns="http://school.example.com/errors">
          Student ID 99999 does not exist.
        </ns:ErrorDetails>
      </detail>
    </soap:Fault>
  </soap:Body>
</soap:Envelope>

### Pros
Can be sent across HTTP but other other networking protocols making it very portable which is useful when different languages/operating systems/platforms are at play.    
Very secure which is useful for corporate systems are at play like finance.    
Very good error handling.     
Maintained for decades so strong community + support. 

### Cons
Steeper learning curve.    
Much less adaptable, follows a strict structure (can also be a pro).    
Usually slower and doesn't allow caching. 
Declining popularity, new APIs solve issues SOAP has so it's used within its niche especially with increasing performance demands. 

## gRPC  
Oriented towards communication between different microservices on the backend (a python analytics backend communicating with a java backend) with a .proto file. 

### Format
An example of a protocol shared between two backends could be:   
syntax = "proto3";

service StudentService {
  rpc GetStudent (StudentRequest) returns (StudentResponse);
}

message StudentRequest {
  int32 id = 1;
}

message StudentResponse {
  int32 id = 1;
  string name = 2;
  bool enrolled = 3;
}
Here we define the service and what type of data we expect to recieve and send. On one backend we would implement the service logic and the function GetStudent, then we could send the data to another backend that listens for the result. This is similar to websockets where you listen on a frontend/backend for changes made on the server but both have different use cases. 

### Pros 
Since it utilises HTTP/2 it's very fast and well maintained.    
Since it's protocol buffers send/recieve responses in binary it's alot faster than sending JSON data which isn't as space efficient.   
It's very convenient for sharing data across multiple backends which makes it essential in microservice oriented applications.    
Its protocol buffers handle alot of the logic for us so we can focus more on implementation.   

### Cons
Not very compatible with frontend/browser where websockets would be a better choice.     
Lot of maintenance since you'd need to learn gRPC related libraries on multiple languages alongside the proto schema.    
Harder to test since you need to use extra tools to decode the binary message/request into a readable format whereas JSON can easily be debugged. 

## WebSockets
Used for real time changes/communication like chat apps and games, particularly prominent amongst frontend/backend communication. 
### Format
Based off my [MyNotesToDo](https://github.com/m0ravat/MyNotesToDo-Node) project I already used websockets to allow for real time changes to be seen when multiple people are on a task. When a task is triggered from the frontend like creating a new card it updates the database then can send a message to both the backend/frontend via websockets. The frontend that listens may trigger a UI change like showing a message that a certain user has created a new card and the backend might console.log() the relevant information for future debugging purposes. 
### Pros
Real time, efficient bi-directional communication. 
### Cons
Can use alot of server resources on large applications, less strict than gRPC and it isn't cache friendly. 

## Respective use cases
Alot of the APIs I've looked at here have different use cases so to summarise:     
- REST APIs are by far the easiest to learn and solid for beginners, however requires further optimisation from HTTP to HTTPS.
- GraphQL is useful for when handling complex queries and can get precisely the data we need. This would be useful for intuitive UIs like dashboards and when handling nested data.
- SOAP APIs are the most portable and secure so it would be useful for information sensitive applications, particularly enterprise since it's also very portable.
- gRPC based APIs would be used mainly on services with microservices where we want communication and fast responses across multiple backends which may be written in different languages.
- Websockets would be used for real time communication between a frontend/backend.

It's also worth of note to mention that I assume alot of complicated applications would merge multiple APIs together somehow, an example being Spotify. They may use Django or another python oritented framework to handle analytics/machine learning for suggesting music, they may have a more faster/optimised backend to quickly get data for the UI, a frontend framework for obvious reasons, another backend that may handle the music playing logic and a mobile implementation. Since each API has their own use cases the common assumption is they implement different APIs for different use cases, such as using websockets for frontend/backend communication (displaying messages like "Playing next song..."), REST APIs to fetch artist/music details quickly and gRPC to send data from the analytics/ML backend in python to another backend like SpringBoot written in Java. This could be reflected in the chain below: 

SpringBoot backend API (send music/artist details) --> Frontend (React/Angular/Mobile alternative like Flutter/React Native)    
SpringBoot backend API (send request to another API to preload the next song) <-(listens for if current song playing is about to finish)-> Frontend (show message like next song playing... at the bottom of the screen)      
SpringBoot backend API (Sends request for music suggestions) <-(gRPC API)-> Django (sends data for suggested songs based off user's favourites)     
Frontend payment request is made --> SOAP API request --> Backend that uses Stripe API to handle payment --> SOAP API success/failure --> Frontend gets response. (with them implementing age verification and the rising concerns for security this could also be something to consider with security)
## Credits 
[GraphQL Explanation by PedroTech](https://www.youtube.com/watch?v=Zg4XIpnLWQg)      
[Postman blog on different APIs](https://blog.postman.com/different-types-of-apis/)     
[IBM Video on gRPC](https://www.youtube.com/watch?v=hVrwuMnCtok) 
