# Radiomaster-ER8-PPM-Frist-prototype
```
#include <Servo.h>
#include <PPMReader.h>

// Initialize a PPMReader on digital pin 3 with 6 expected channels.
byte interruptPin = 3;
byte channelAmount = 5;
PPMReader ppm(interruptPin, channelAmount);

// Define ESC pins
#define ESC1_PIN 5
#define ESC2_PIN 6
#define ESC3_PIN 9
#define ESC4_PIN 10

Servo esc1, esc2, esc3, esc4;

void setup() {
    Serial.begin(115200); // Initialize serial communication
    esc1.attach(ESC1_PIN);
    esc2.attach(ESC2_PIN);
    esc3.attach(ESC3_PIN);
    esc4.attach(ESC4_PIN);
    
    // Initialize ESCs
    esc1.writeMicroseconds(1000);
    esc2.writeMicroseconds(1000);
    esc3.writeMicroseconds(1000);
    esc4.writeMicroseconds(1000);
    
    delay(2000); // Wait for ESCs to initialize
}

void loop() {
    // Print latest valid values from all channels
    for (byte channel = 1; channel <= channelAmount; ++channel) {
        unsigned value = ppm.latestValidChannelValue(channel, 0);
        Serial.print(String(value) + "\t");
        
        // Control the ESCs based on the throttle channel (usually channel 1)
        if (channel == 1) {
            // Map throttle value to ESC values
            int escThrottle = map(value, 1000, 2000, 1000, 2000);
            esc1.writeMicroseconds(escThrottle);
            esc2.writeMicroseconds(escThrottle);
            esc3.writeMicroseconds(escThrottle);
            esc4.writeMicroseconds(escThrottle);
        }
    }
    Serial.println();
    delay(20);
}
