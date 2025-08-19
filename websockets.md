## WebSockets
Used for real time changes/communication like chat apps and games, particularly prominent amongst frontend/backend communication. 
### Format
Based off my [MyNotesToDo](https://github.com/m0ravat/MyNotesToDo-Node) project I already used websockets to allow for real time changes to be seen when multiple people are on a task. When a task is triggered from the frontend like creating a new card it updates the database then can send a message to both the backend/frontend via websockets. The frontend that listens may trigger a UI change like showing a message that a certain user has created a new card and the backend might console.log() the relevant information for future debugging purposes. 
### Pros
Real time, efficient bi-directional communication. 
### Cons
Can use alot of server resources on large applications, less strict than gRPC and it isn't cache friendly. 