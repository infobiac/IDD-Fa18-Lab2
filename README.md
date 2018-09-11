# Make a Digital Timer!
 
## Overview
For this assignment, you are going to 

A) [Solder your LCD panel](#part-a-solder-your-lcd-panel)

B) [Write text to an LCD Panel](#part-b-writing-to-the-lcd) 

c) [Using a time-based digital sensor!](#part-c-using-a-time-based-digital-sensor)

D) [Make your Arduino sing!](#part-d-make-your-arduino-sing)

E) [Make your own timer](#part-e-make-your-own-timer) 
 
## In The Report
Include your responses to the bold questions on your own fork of [this lab report template](https://github.com/FAR-Lab/IDD-Fa18-Lab2). Include snippets of code that explain what you did. Deliverables are due next Tuesday. Post your lab reports as README.md pages on your GitHub, and post a link to that on your main class hub page.

## Part A. Solder your LCD panel

Picture [here](https://github.com/infobiac/IDD-Fa18-Lab2/blob/master/data/soldered.JPG). I didn't take a picture until after hello world, unfortunately.
## Part B. Writing to the LCD
 
**a. What voltage level do you need to power your display?** The display voltage is 5v (we can tell that it's 5v by unplugging the 5v power source, at which point the backlight remains on, but the display panics).

**b. What voltage level do you need to power the display backlight?** The display voltage is 3.3v (we can tell by unplugging the 3.3v power source, at which point the backlight turns off but the display remains on).
   
**c. What was one mistake you made when wiring up the display? How did you fix it?** I did not plug RS into IO 12 the first time and plugged E into IO12 rather than IO11, which caused the display to light up but nothing printed. Once I identified this issue and rectified it, it ended up working.

**d. What line of code do you need to change to make it flash your name instead of "Hello World"?** By changing the text in   lcd.print("Hello World"); within the setup function to my name, I could print my name instead.

 
**e. Include a copy of your Lowly Multimeter code in your lab write-up.**
```

// include the library code:
#include <LiquidCrystal.h>

// initialize the library by associating any needed LCD interface pin
// with the arduino pin number it is connected to
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

void setup() {
  // set up the LCD's number of columns and rows:
  lcd.begin(16, 2);
}

void loop() {
  // Turn off the display:
  lcd.clear();
  int res=analogRead(A5);
  lcd.print(res);
  delay(100);
}
```

Multimeter video [here](https://github.com/infobiac/IDD-Fa18-Lab2/blob/master/data/potentiometer.MOV).

## Part C. Using a time-based digital sensor

Rotary encoder video [here](https://github.com/infobiac/IDD-Fa18-Lab2/blob/master/data/rotaryencoder.MOV).

## Part D. Make your Arduino sing!

Video [here](https://github.com/infobiac/IDD-Fa18-Lab2/blob/master/data/sound1.MOV).
**a. How would you change the code to make the song play twice as fast?**

We can halve the value of pauseBetweenNotes. For me, I did the following:
```  int pauseBetweenNotes = noteDuration * 1.30/2;```

**b. What song is playing?**
The star wars theme song!

Video [here](https://github.com/infobiac/IDD-Fa18-Lab2/blob/master/data/starwars.MOV).

## Part E. Make your own timer

**a. Make a short video showing how your timer works, and what happens when time is up!**
The timer works as a study break timer. You set the amount of time you want the timer to run for via the rotary encoder. You then close your laptop to signify the start of a study break, which presses the button. In an ideal world, the button is  directly onto the laptop screen, but I couldn't figure out how to attach it, so I used a chopstick instead. The display updates to show the remaining amount of time in seconds, continuously updating. When the timer goes off, the alarm goes off, and the display updates to show time's up. The alarm will not stop chiming until the button is pressed again, forcing the user to actually press the button and get back to work!

Link [here](https://github.com/infobiac/IDD-Fa18-Lab2/blob/master/data/final.mov).

Code here:
```
#include <LiquidCrystal.h>


#define ENC_A 6 //these need to be digital input pins
#define ENC_B 7

const int buttonPin = 9;     // the number of the pushbutton pin

const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

int buttonState = 0;         // variable for reading the pushbutton status

int in_timer_loop = 0;
int total_time;
int start;
int finished = 1;

void setup()
{
  /* Setup encoder pins as inputs */
  pinMode(buttonPin, INPUT);
  pinMode(ENC_A, INPUT_PULLUP);
  pinMode(ENC_B, INPUT_PULLUP);
 
  lcd.begin(16, 2);
  lcd.print("Study break (s)");    


}
 
void loop()
{
  if(!in_timer_loop){
    static unsigned int counter4x = 0;      //the SparkFun encoders jump by 4 states from detent to detent
    static unsigned int counter = 0;
    static unsigned int prevCounter = 0;
    int tmpdata;
    tmpdata = read_encoder();
    if( tmpdata) {
      counter4x += tmpdata;
      counter = counter4x/4;
      if (prevCounter != counter){
          lcd.setCursor(0, 0);
          lcd.clear();
          lcd.print(counter);
  
    
      }
      prevCounter = counter;
  
    }
    
    buttonState = digitalRead(buttonPin);
    if (buttonState == HIGH && finished == 1) {
      
      in_timer_loop = 1;
      start = millis();
      total_time = start/1000 + counter;
      lcd.clear();
    }
  }

  if(in_timer_loop){
    lcd.setCursor(0,0);
    lcd.print(total_time - millis()/1000);
  }
  if (millis()/1000 > total_time && in_timer_loop){
    lcd.setCursor(0, 0);
    lcd.clear();
    lcd.print("Time's up!!!");
    int button_pressed = 0;
    finished = 0;
    while(!button_pressed){
      tone(8, 262, 250);
      delay(500);
      if(digitalRead(buttonPin) == HIGH){
        button_pressed = 1;
      }
    }
    in_timer_loop = 0;
  }
} 
 
/* returns change in encoder state (-1,0,1) */
int read_encoder()
{
  static int enc_states[] = {
    0,-1,1,0,1,0,0,-1,-1,0,0,1,0,1,-1,0  };
  static byte ABab = 0;
  ABab *= 4;                   //shift the old values over 2 bits
  ABab = ABab%16;      //keeps only bits 0-3
  ABab += 2*digitalRead(ENC_A)+digitalRead(ENC_B); //adds enc_a and enc_b values to bits 1 and 0
  return ( enc_states[ABab]);
 
 
}
```

**b. Post a link to the completed lab report your class hub GitHub repo.**
