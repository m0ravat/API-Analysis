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
- Since it utilises HTTP/2 it's very fast and well maintained.    
- Since it's protocol buffers send/recieve responses in binary it's alot faster than sending JSON data which isn't as space efficient.   
- It's very convenient for sharing data across multiple backends which makes it essential in microservice oriented applications.    
- Its protocol buffers handle alot of the logic for us so we can focus more on implementation.   

### Cons
- Not very compatible with frontend/browser where websockets would be a better choice.     
- Lot of maintenance since you'd need to learn gRPC related libraries on multiple languages alongside the proto schema.    
- Harder to test since you need to use extra tools to decode the binary message/request into a readable format whereas JSON can easily be debugged. 