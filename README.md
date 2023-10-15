# htu21d
HDU21D sensor data logger



This Arduino code is designed to read temperature and humidity data from an Adafruit_HTU21DF sensor and log the data to an SD card.

### Libraries

#include <Wire.h>
#include <Adafruit_HTU21DF.h>
#include <SD.h>

These are the libraries used in the code:
- `Wire.h`: Used for I2C communication.
- `Adafruit_HTU21DF.h`: Library for the Adafruit HTU21DF sensor.
- `SD.h`: Library for reading and writing to SD cards.

### Pin Configuration

const int chipSelect = 10;

Defines the chip select pin for the SD card module. In this case, it's pin 10.

### Sensor and SD Card Initialization

Adafruit_HTU21DF htu = Adafruit_HTU21DF();

void setup() {
  Serial.begin(9600);

  if (!SD.begin(chipSelect)) {
    Serial.println("SD card initialization failed!");
    while (1);
  }

  if (!htu.begin()) {
    Serial.println("Couldn't find sensor! Check wiring and power. Is the sensor connected?");
    while (1);
  }
}

In the `setup()` function:
- Serial communication is initiated at a baud rate of 9600.
- The SD card is initialized, and if it fails, an error message is printed, and the program enters an infinite loop.
- The HTU21DF sensor is initialized, and if it fails, an error message is printed, and the program enters an infinite loop.

### Main Loop

void loop() {
  float temperature = htu.readTemperature();
  float humidity = htu.readHumidity();

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" °C, Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  logData(temperature, humidity);

  delay(1000);
}

In the `loop()` function:
- Temperature and humidity data are read from the HTU21DF sensor.
- The data is printed to the Serial monitor.
- The `logData` function is called to log the data to the SD card.
- There is a delay of 1000 milliseconds (1 second) before the next iteration.

### Data Logging Function

void logData(float temperature, float humidity) {
  File dataFile = SD.open("test.txt", FILE_WRITE);

  if (dataFile) {
    dataFile.print(millis()); // Timestamp
    dataFile.print(", ");
    dataFile.print(temperature);
    dataFile.print(", ");
    dataFile.print(humidity);
    dataFile.println();
    dataFile.close();
  } else {
    Serial.println("Error opening datalog.txt");
  }
}

The `logData` function:
- Opens the file "test.txt" on the SD card for writing.
- If the file is available, it writes a line of data containing a timestamp, temperature, and humidity.
- Closes the file.
- If there is an error opening the file, it prints an error message to the Serial monitor.

This code essentially reads sensor data, prints it to the Serial monitor, and logs it to an SD card file. Make sure to properly wire the sensor and SD card module according to your hardware setup.
