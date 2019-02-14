# x2modbusrtu
This project converts protocols such as snmp,opc,modbus tcp... to modbus rtu.
It has two simple interfaces for user application.The first interface is sendModbusRequest,which receives an modbus rtu request from the user application, and may convert it to specific underlaying protocol frame, such as an snmp request, or a series of requests,and then sends it(them) to the device which the underlaying protocol is running on it.After that,the user application call receiveModbusResponse to wait for a modbus rtu response.The modbus response is made in this way:the underlaying protocol waits for a response from the device which a request was just sent to,after the response was received, the underlaying protocol converts it to a modbus rtu response, then sends it back to user application which is waiting for it via receiveModbusResponse.
The user application code is something like this:

int communication( void ){

  int ret;
  
  char *modbusRequest;
  
  int requestLength;
  
  char modbusResponse[256];//255 is the max length of modbus rtu frame.
  
  int responseLength;

  //Here, do something to make the modbus request.
  
  ret = sendModbusRequest( modbusRequest, requestLength );
  
  //If there is some error.
  
  if( ret == -1 ){
  
    return;
    
  }
  
  //Or,waiting for response.
  responseLength = receiveModbusResponse( modbusResponse );
  
  if( responseLength == -1 ){
  
    //Do something to handle the error
    
    return;
    
  }
  
  //Or, go
  
}

The project is written in C,it can run on any linux device.
