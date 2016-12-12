//# Final-Poject
//Final for Programming &amp; Electronics: The modern creation of Adam


uint32_t hsb2rgb(uint16_t index, uint8_t sat, uint8_t bright);

#include <Servo.h>

Servo myservo;  // hand // create servo object to control a servo // twelve servo objects can be created on most boards
Servo myservo2; // LIGHTs // this is where you declare how many servos you need. (next one would have a 3 on it)

int pos = 0;    // variable to store the servo position (tells where the final pos # is)

#include <Adafruit_NeoPixel.h>
#define PIN 11
#define NUMPIXELS 10

Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  myservo.attach(9);   // hand
  // myservo2.attach(10); //fabric 1
  // myservo3.attach(11); //fabric 2

  Serial.begin(9600);
  pixels.begin();
}

void loop() {

  //HAND:


  int distance = analogRead(A0); // reads the value of the potentiometer (value between 0 and 1023)
  Serial.println(distance);

  if (distance < 300) {
    myservo.write(180);
    for (int i = 0; i < NUMPIXELS; i += 1) {
      uint32_t color = hsb2rgb(random(768), 255, 80); // 80 is the "open" brightness... adjust to 
      pixels.setPixelColor(i, color);
    }
  } else {
    myservo.write(map(distance, 300, 700, 180, 0));
    for (int i = 0; i < NUMPIXELS; i += 1) {
      uint32_t color = hsb2rgb(random(768), 255, map(distance, 300, 700, 80, 255)); // 80-255 is the "nearby" brightness
      pixels.setPixelColor(i, color);
    }
  }
  pixels.show();


  //  distance = map(distance, 0, 1023, 0, 175); // scale it to use it with the servo (value between 0 and 180)
  //  //for (pos = 10; pos <= 170; pos += 10) {
  //  myservo.write(distance); // sets the servo position according to the scaled value
  delay(150);  // waits for the servo to get there



  //LIGHT:

  // int distance = analogRead(A0);
  // int hsb2rgb;

  //  if (distance < 0) {
  //    for (int i = 0; i < NUMPIXELS; i += 1) {
  //      uint32_t color = hsb2rgb(0, 0, 00);
  //      pixels.setPixelColor(i, color);
  //    }
  //  }
  //  else {
  //    for (int i = 0; i < NUMPIXELS; i += 1) {
  //      uint32_t color = hsb2rgb(random(768), 255, distance / 4);
  //      pixels.setPixelColor(i, color);
  //    }
  //  }


  //fabric 1:

  //for (pos = 0; pos <= 180; pos += 1) {
  // myservo2.write(pos);
  // delay(200);
  //}

  //fabric 2:

  //for (pos = 180; pos >= 0; pos -= 1) {
  //myservo3.write(pos);
  //delay(500);
  //}
  //Serial.println(distance);

}






//LIGHTS (hsb) :

//******************************************************************************
 * accepts hue, saturation and brightness values and outputs three 8-bit color
 * values in an array (color[])
 *
 * saturation (sat) and brightness (bright) are 8-bit values.
 *
 * hue (index) is a value between 0 and 767. hue values out of range are
 * rendered as 0.
******

uint32_t hsb2rgb(uint16_t index, uint8_t sat, uint8_t bright)
{
  uint16_t r_temp, g_temp, b_temp;
  uint8_t index_mod;
  uint8_t inverse_sat = (sat ^ 255);

  index = index % 768;
  index_mod = index % 256;

  if (index < 256)
  {
    r_temp = index_mod ^ 255;
    g_temp = index_mod;
    b_temp = 0;
  }

  else if (index < 512)
  {
    r_temp = 0;
    g_temp = index_mod ^ 255;
    b_temp = index_mod;
  }

  else if ( index < 768)
  {
    r_temp = index_mod;
    g_temp = 0;
    b_temp = index_mod ^ 255;
  }

  else
  {
    r_temp = 0;
    g_temp = 0;
    b_temp = 0;
  }

  r_temp = ((r_temp * sat) / 255) + inverse_sat;
  g_temp = ((g_temp * sat) / 255) + inverse_sat;
  b_temp = ((b_temp * sat) / 255) + inverse_sat;

  r_temp = (r_temp * bright) / 255;
  g_temp = (g_temp * bright) / 255;
  b_temp = (b_temp * bright) / 255;

  return ((uint32_t)r_temp << 16) | ((uint32_t)g_temp << 8) | b_temp;
}

******/

