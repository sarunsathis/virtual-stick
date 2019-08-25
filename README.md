#include "DHT.h"
#define echo 10
#define trig 11
#define DHTPIN 2 
#define tep 8
#define mod 7
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

long dis=0;
int j=0,s=0;
float C,H;


void setup() {
  pinMode(echo,INPUT);


  Serial.begin(9600);
  dht.begin();

  pinMode(trig,OUTPUT);
  pinMode(12,OUTPUT);

}



void temp()
{
  float c,h;
  c=dht.readTemperature();
  h=dht.readHumidity();

    if(s==0)
    {
      C=c;
      H=h;
      s++;
    }
    if(c!=c && H!=h)
  { Serial.print("temperature :  ");
   Serial.println(c);
   Serial.print("humidty : ");
   Serial.println(h);
   C=c;
   H=h;
  

   if(c>34 && h<60)
   Serial.println("today will be a hot day and with almost no probablity of rain ");
   else if (c<27 && h>60)
   Serial.println("today is a relatively colder day and there are chances of rain ");
  }
}

int i=0;
void speedi()
{ long t1=0,t2=0,s=0,d1=0,d2=0;

  
  digitalWrite(trig,1);
  delayMicroseconds(2);
  digitalWrite(trig,0);
  
  d1=pulseIn(echo,1);
  d1=d1*0.034/2;

  delay(550);

  digitalWrite(trig,1);
  delayMicroseconds(2);
  digitalWrite(trig,0);
  d2=pulseIn(echo,1);
  
  d2=d2*0.034/2;
  
  s=(d1-d2);
  
  if(s<=-75 && i==0)
  {
    Serial.println("obstacle arriving at high speed relatively");
    i=1;
  }
  else
  i=0;
}
void distance()
{
  
  digitalWrite(trig,1);
  delayMicroseconds(2);
  digitalWrite(trig,0);

  dis=pulseIn(echo,1);
  dis=dis*0.034/2;

  if(dis<=50 && j==0)
  {
    Serial.println("obstacle approaching in ");
    Serial.print(dis);
    Serial.println( " centimeters ");
    j=1;
  }
  else
  j=0;

}
int a=0;
void loop() {
  digitalWrite(12,1);

  temp();
 distance();
 speedi();

 delay(750);
 
}
