# Rest API

## Format
The common way of fetching data is by sending a HTTP request to the server, alongside a URL which specifies the data and a resource identifier. An example would be:    
GET /user/:id where you would fill the :id with an id (string or number) and it gets that specific user, you could remove the /:id portion and it would typically fetch all users instead. This can get very messy, turning into something like the following for a social media post: GET /user/12/post/32/comments to fetch all comments. You can also add path parameters, so we could extend on the social media comment post by doing a GET /user/12/post/32/comments?likes_gt=30. The _gt stands for greater than to imply we want to fetch all comments with over 30 likes. 
## HTTP Request types
POST - Inputting/creating data.       
PUT - Updating data by replacing all fields.       
PATCH - Partially update a resource.      
HEAD - Retrieving headers for a resource like HTTP Status codes, Content-Type, Content-Length, Last-Modified etc.      
OPTIONS - Check what methods are supported.      
DELETE - Deleting data      
GET - Fetching data (can be used in frameworks like express to also tell the server to render a web page with said data).      
## Return Type
It often returns data in JSON format, but can also return HTML,CSV, XML making it very portable. 

## Pros
- Typically uses conventional notation so it's very readable, easy to figure out the purpose behind API routes.
- The HTTP requests are very broad and cover every aspect of CRUD making it compatible with many services.
- Flexible data formatting works well for APIs where you may want data in multiple formats.
- Supports caching
- Makes code more modular since the frontend only refers to the API endpoint when sending a HTTP request which is handled by the backend.

## Cons
- For complex/tedious queries it can become very long as seen in the above example where we try to fetch a number of comments under a social media post. We are referring to 3 different resources and using one parameter which becomes very lengthy. We could use a custom endpoint like GET /socialMediaPostsGT/30 or abbreviate it but risk readability as a result. 
- Since there is no defined science to defining API endpoints we can define them however we want which can be a con since it can lead to many producing messy code. NextJS for example handles routing automatically now so all you need to do is follow the folder/file naming conventions giving it a steeper learning curve but makes it easier on the user in the long run as opposed to using React Router.
- To interact with the backend you have to send a request, increasing the need for websockets as it can't automatically keep up with live changes.