# air-pollution-monitoring-system-using-IoT

#include <LiquidCrystal.h>

LiquidCrystal lcd(12,11,5,4,3,2);

int pin8 = 8;
int redled = 13;
int orgled = 10;
int grnled = 9;
int gasPin = A0;
int tempPin = A1;
int gasValue = 0;
int tempValue = 0;

void setup()
{
  
  pinMode(gasPin,INPUT);
  pinMode(A1,INPUT);
  pinMode(pin8,OUTPUT);
  pinMode(redled,OUTPUT);
  pinMode(orgled,OUTPUT);
  pinMode(grnled,OUTPUT);
  lcd.begin(16,2);
  lcd.print("What is the air ");
  lcd.print("Quality today?");
  Serial.begin(9600);
  lcd.display();
  
}

void loop()
{
  gasValue = analogRead(gasPin);
  Serial.print("Air quality in PPM: ");
  Serial.println(gasValue);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Air Quality: ");
  lcd.print(gasValue);
  
  
  tempValue = map(((analogRead(tempPin) - 20) * 3.04), 0, 1023, -40, 125);
  Serial.print("Temperature: ");
  Serial.print(tempValue);
  Serial.println(" degree C");
  delay(1000);
  
if(gasValue<=350)
  {
    Serial.print("Fresh Air");
    Serial.print("\r\n");
    lcd.setCursor(0,1);
    lcd.print("Fresh Air");
    digitalWrite(grnled,HIGH);
    delay(1000);
  }
  else if(gasValue>350 && gasValue<600)
  {
    Serial.print("Poor Air");
    Serial.print("\r\n");
    lcd.setCursor(0,1);
    lcd.print("Poor Air");
    digitalWrite(orgled,HIGH);
    delay(1000);
  }
  else if(gasValue>=600)
  {
    Serial.print("Very Poor Air");
    Serial.print("\r\n");
    lcd.setCursor(0,1);
    lcd.print("Very Poor Air");
    delay(1000);
  }
  
  if(gasValue>=600){
    digitalWrite(pin8,HIGH);
    digitalWrite(redled,HIGH);
  }
  else
  {
    digitalWrite(pin8,LOW);
    digitalWrite(redled,LOW);
    digitalWrite(orgled,LOW);
    digitalWrite(grnled,LOW);
  }
}
