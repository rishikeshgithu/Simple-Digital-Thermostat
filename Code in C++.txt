  #include <dht11.h>
  #include <Wire.h>
  #include <LiquidCrystal_I2C.h>

  #define DHT11PIN 4
  int outpin = 5;

  dht11 DHT11;
  LiquidCrystal_I2C lcd(0x27, 16, 2);  // Use the correct I2C address for your LCD

  void setup() {
    Serial.begin(9600);
    pinMode(outpin, OUTPUT); // Set the outpin (pin 5) as an output

    Wire.begin();
    lcd.init();        // Initialize the LCD
    lcd.backlight();   // Turn on the backlight
  }

  void loop() {
    int chk = DHT11.read(DHT11PIN);

    Serial.println();

    Serial.print("Humidity (%): ");
    Serial.println((float)DHT11.humidity, 2);

    Serial.print("Temperature (C): ");
    Serial.println((float)DHT11.temperature, 2);

    lcd.setCursor(0, 0); // Set cursor to the first line
    lcd.print("Humidity: ");
    lcd.print((float)DHT11.humidity, 2); // Display humidity

    lcd.setCursor(0, 1); // Set cursor to the second line
    lcd.print("Temp: ");
    lcd.print((float)DHT11.temperature, 2); // Display temperature

    if (DHT11.humidity < 70) {
      digitalWrite(outpin, LOW); // Set pin 5 high if humidity is less than 70
    } 
    else {
      digitalWrite(outpin, HIGH); // Otherwise, set it low
    }

    delay(1000);
  }
