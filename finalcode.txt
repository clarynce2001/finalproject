//CAPAROS, CLARYNCE
//IMPANG, ANGELICA
//CALAPUTOC, SHERILYN
//LUCENO, EDWIN

#include <SPI.h>  
#include <MFRC522.h> 
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
#include <Servo.h>
LiquidCrystal_I2C lcd(0x27,16,2);
#define RST_PIN 9
#define SS_PIN 10
MFRC522 mfrc522(SS_PIN, RST_PIN);   
Servo myServo;

int *aux;
int card1[4];
int flag = 0;
int led = 13;
int cnt =0;

void setup() {
        pinMode(led, OUTPUT);
        
        lcd.init();
        lcd.backlight();
    SPI.begin();        
    mfrc522.PCD_Init();   
        lcd.print("     SWIPE");
        lcd.setCursor(0,1);
        lcd.print("     HERE  ");

        myServo.attach(3); 
        myServo.write(0);
}

void loop() {
   
    if ( ! mfrc522.PICC_IsNewCardPresent()) {
        return;
    }

   
    if ( ! mfrc522.PICC_ReadCardSerial()) {
        return;
    }

   
    
    for (byte i = 0; i < mfrc522.uid.size; i++) {
            aux[i]= mfrc522.uid.uidByte[i];
    } 
           if(flag == 0)
           {
             lcd.clear();
             lcd.print("Your card UID is: ");
             lcd.setCursor(0,1);
             for (byte i = 0; i < mfrc522.uid.size; i++) {
               card1[i] = aux[i];
             lcd.print( card1[i], DEC);
             lcd.print( " ");
             flag = 1;
            }
           delay(3000);
           lcd.clear();
           lcd.print(" CAPAROS, IMPANG");
           lcd.setCursor(0,1);
           lcd.print("  LUCENO, CALAPUTOC   ");
           } 

           else{
            
           
             for (byte i = 0; i < mfrc522.uid.size; i++) {
               if(aux[i] == card1[i])
                cnt++;
             }
                            
            if(cnt == mfrc522.uid.size-1)
            {
              lcd.clear();
              lcd.print("   ACCESS     ");
              lcd.setCursor(0,1);
              lcd.print("   PERMITTED    ");
              delay(400);

               myServo.write(180);
               delay(5000);
               myServo.write(0);
             }
             else
             {
              lcd.clear();
              lcd.print("    ACCESS     ");
              lcd.setCursor(0,1);
              lcd.print("    DENIED     ");
              delay(2000);
             }
             
           }
           
           lcd.clear();
           lcd.print("    TOLL TAX ");
           lcd.setCursor(0,1);
           lcd.print("     SYSTEM   ");
  cnt=0;
}