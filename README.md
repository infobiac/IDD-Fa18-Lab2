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

**Take a picture of your soldered panel and add it here!**

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

## Part C. Using a time-based digital sensor

**Upload a video of your working rotary encoder here.**


## Part D. Make your Arduino sing!

**a. How would you change the code to make the song play twice as fast?**
 
**b. What song is playing?**


## Part E. Make your own timer

**a. Make a short video showing how your timer works, and what happens when time is up!**

**b. Post a link to the completed lab report your class hub GitHub repo.**
