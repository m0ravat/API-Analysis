# API-Analysis

[Rest APIs](RestAPI.md)
[GraphQL APIs](graphQL.md)
[gRPC APIs](gRPC.md)
[SOAP APIs](soap.md)
[WebSockets](websockets.md)


## Respective use cases
Alot of the APIs I've looked at here have different use cases so to summarise:     
- REST APIs are by far the easiest to learn and solid for beginners, however requires further optimisation from HTTP to HTTPS.
- GraphQL is useful for when handling complex queries and can get precisely the data we need. This would be useful for intuitive UIs like dashboards and when handling nested data.
- SOAP APIs are the most portable and secure so it would be useful for information sensitive applications, particularly enterprise since it's also very portable.
- gRPC based APIs would be used mainly on services with microservices where we want communication and fast responses across multiple backends which may be written in different languages.
- Websockets would be used for real time communication between a frontend/backend.

## Real Life Example
It's also worth of note to mention that I assume alot of complicated applications would merge multiple APIs together somehow, an example being Spotify. They may use Django or another python oritented framework to handle analytics/machine learning for suggesting music, they may have a more faster/optimised backend to quickly get data for the UI, a frontend framework for obvious reasons, another backend that may handle the music playing logic and a mobile implementation. Since each API has their own use cases the common assumption is they implement different APIs for different use cases, such as using websockets for frontend/backend communication (displaying messages like "Playing next song..."), REST APIs to fetch artist/music details quickly and gRPC to send data from the analytics/ML backend in python to another backend like SpringBoot written in Java. This could be reflected in the chain below: 

SpringBoot custom backend (send music/artist details) --> Frontend (React/Angular/Mobile alternative like Flutter/React Native)    
SpringBoot backend API (send request to another API to preload the next song) <-(listens for if current song playing is about to finish)-> Frontend (show message like "next song playing..." at the bottom of the screen)      
SpringBoot backend API (Sends request for music suggestions) <-(gRPC API)-> Django (sends data for suggested songs based off user's favourites)     
Frontend payment request is made --> SOAP API request --> Backend that uses Stripe API to handle payment --> SOAP API success/failure --> Frontend gets response. (with Spotify also adding age verification security is a concern which only enhances the need for a SOAP API)
## Credits 
[GraphQL Explanation by PedroTech](https://www.youtube.com/watch?v=Zg4XIpnLWQg)       
[Postman blog on different APIs](https://blog.postman.com/different-types-of-apis/)         
[IBM Video on gRPC](https://www.youtube.com/watch?v=hVrwuMnCtok)     
