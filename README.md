#include "mbed.h"
#include "MMA8451Q.h"

// Define I2C pins and address for the accelerometer
#define MMA8451_I2C_ADDRESS (0x1d<<1)
#define SDA_PIN p28
#define SCL_PIN p27

// Initialize I2C and accelerometer
I2C i2c(SDA_PIN, SCL_PIN);
MMA8451Q accelerometer(SDA_PIN, SCL_PIN, MMA8451_I2C_ADDRESS);

// Initialize PWM outputs for RGB LED
PwmOut red_led(p23);
PwmOut green_led(p24);
PwmOut blue_led(p25);

int main() {
    // Set PWM frequency and initial duty cycle
    red_led.period(0.001f); // 1 kHz
    green_led.period(0.001f); // 1 kHz
    blue_led.period(0.001f); // 1 kHz

    while (1) {
        // Read accelerometer values
        float x = accelerometer.getAccX();
        float y = accelerometer.getAccY();
        float z = accelerometer.getAccZ();

        // Convert accelerometer values to PWM duty cycle (0.0 - 1.0)
        red_led = fabs(x);
        green_led = fabs(y);
        blue_led = fabs(z);

        // Add a short delay
        wait(0.1);
    }
}
