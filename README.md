# CAPSTONE - LOW COST SENSOR MODULE FOR AQUACULTURE 
- **We are developing and designing a low cost sensor module that has its application in water quality monitoring for providing a better aquatic life.** 
## pH Sensor
- First CCP source code was used to obtain raw values.
- pH sensor was calibrated physically by using the adjusting srcew on IC 
- Then CPP source code was used extract pH values of different liquids.
```
#define SensorPin 0          //pH meter Analog output to Arduino Analog Input 0
unsigned long int avgValue;  //Store the average value of the sensor feedback
float b;
int buf[10],temp;

void setup()
{
  pinMode(13,OUTPUT);  
  Serial.begin(9600);  
  Serial.println("Ready");    //Test the serial monitor
}
void loop()
{
  for(int i=0;i<10;i++)       //Get 10 sample value from the sensor for smooth the value
  { 
    buf[i]=analogRead(SensorPin);
    delay(10);
  }
  for(int i=0;i<9;i++)        //sort the analog from small to large
  {
    for(int j=i+1;j<10;j++)
    {
      if(buf[i]>buf[j])
      {
        temp=buf[i];
        buf[i]=buf[j];
        buf[j]=temp;
      }
    }
  }
  avgValue=0;
  for(int i=2;i<8;i++)                      //take the average value of 6 center sample
    avgValue+=buf[i];
  float phValue=(float)avgValue*5.0/1024/6; //convert the analog into millivolt
  phValue=3.5*phValue;                      //convert the millivolt into pH value
  Serial.print("    pH:");  
  Serial.print(phValue,2);
  Serial.println(" ");
  digitalWrite(13, HIGH);       
  delay(800);
  digitalWrite(13, LOW); 

}

```
- Obtained values were compared with the actual ones.
- Then python code was used to obtain similar values.
```
import pyfirmata
import time

ser = pyfirmata.Arduino('/dev/ttyACM0', 9800, timeout=1)
time.sleep(2)

for i in range(50):
    line = ser.readline()   # read a byte
    if line:
        string = line.decode()  # convert the byte string to a unicode string
        num = int(string) # convert the unicode string to an int
        num = num*5.0*3.5/1024
        print(num)
ser.close() 
```
![ph Sensor Image](https://github.com/Sensor-Module-Capstone/-/blob/main/Screenshot%20from%202022-04-25%2003-05-38.png)
