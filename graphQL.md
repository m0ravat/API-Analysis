# GraphQL

## Format 
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