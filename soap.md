# SOAP API
Unlike RESTful APIs which are an architectural style SOAP is an API that follows the SOAP protocol.

## Format
Data is sent and received via XML format and has 4 layers:    
- Envelope: Defining the structure of the message.
- Encoding: Rules for expressing the type of data.
- Requests: How each SOAP API request is structured.
- Responses: How each SOAP API response is structured.

## Example 
This is an example of a request followed by a successful/failed response:    
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

<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">  
  <soap:Body>  
    <ns:CheckAttendanceResponse xmlns:ns="http://school.example.com/student">  
      <ns:StudentID>12345</ns:StudentID>  
      <ns:Date>2025-08-18</ns:Date>  
      <ns:Present>true</ns:Present>  
    </ns:CheckAttendanceResponse>  
  </soap:Body>  
</soap:Envelope>  

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
- Can be sent across HTTP but other other networking protocols making it very portable which is useful when different languages/operating systems/platforms are at play.    
- Very secure which is useful for corporate systems are at play like finance.    
- Very good error handling.     
- Maintained for decades so strong community + support. 

### Cons
- Steeper learning curve.    
- Much less adaptable, follows a strict structure (can also be a pro).    
- Usually slower and doesn't allow caching. 
- Declining popularity, new APIs solve issues SOAP has so it's used within its niche especially with increasing performance demands. 