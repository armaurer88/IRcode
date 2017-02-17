# IRcode
code for arduino NANO to read IR signals
 /* Example program for from IRLib – an Arduino library for infrared encoding and decoding
 * Version 1.3   January 2014
 * Copyright 2014 by Chris Young http://cyborg5.com
 * Based on original example sketch for IRremuote library 
 * Version 0.11 September, 2009
 * Copyright 2009 Ken Shirriff
 * http://www.righto.com/
 */
 /*
 * IRLib: IRrecvDump - dump details of IR codes with IRrecv
 * An IR detector/demodulator must be connected to the input RECV_PIN.
 */

#include <IRLib.h>

//hace RECV_PIN el pin #11
int RECV_PIN = 11;

//inicializa IRrecv e asigna a My_Receiver el valor 11
IRrecv My_Receiver(RECV_PIN);

//why IRdecode is not in red??
IRdecode My_Decoder;
unsigned int Buffer[RAWBUF];

void setup()
{
  //inicia serial a baudrate de 9600
  //espera 2s y mientras serial sea dif a cero 
  //activa la recepción de codgo y usa el ""buffer asignado 
  Serial.begin(9600);
  delay(2000);while(!Serial);//delay for Arduino
  My_Receiver.enableIRIn(); // Start the receiver
  My_Decoder.UseExtnBuf(Buffer);
}
  //si My_Receiver llama a GetResults y es true, ejecuta lo siguiente...
  // en My_Receiver ejecuta resume (continua recibiendo codigos ir)
  //en My_Decoder ejecuta decode
  //porque DumpResults no esta en naranja??
void loop() {
  if (My_Receiver.GetResults(&My_Decoder)) {
    //Restart the receiver so it can be capturing another code
    //while we are working on decoding this one. hayy perro
    My_Receiver.resume(); 
    My_Decoder.decode();
    My_Decoder.value &=0xf7ff;
    My_Decoder.value &=0xfeffff;
    My_Decoder.DumpResults();
  }
}
