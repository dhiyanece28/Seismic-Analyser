#include <Wire.h>
#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>

Adafruit_MPU6050 mpu;

unsigned long previousMicros = 0;
const int sampleRate = 200;                 // 200 Hz sampling
const int sampleInterval = 1000000 / sampleRate;

void setup() {

  Serial.begin(230400);
  Wire.begin();

  if (!mpu.begin()) {
    Serial.println("MPU6050 not detected");
    while (1);
  }

  // High sensitivity for small vibrations
  mpu.setAccelerometerRange(MPU6050_RANGE_2_G);

  // Noise filtering
  mpu.setFilterBandwidth(MPU6050_BAND_44_HZ);

  Serial.println("Ax,Ay,Az");
}

void loop() {

  if (micros() - previousMicros >= sampleInterval) {

    previousMicros = micros();

    sensors_event_t a, g, temp;
    mpu.getEvent(&a, &g, &temp);

    float Ax = a.acceleration.x / 9.81;
    float Ay = a.acceleration.y / 9.81;
    float Az = a.acceleration.z / 9.81;

    Serial.print(Ax, 3);
    Serial.print(",");
    Serial.print(Ay, 3);
    Serial.print(",");
    Serial.println(Az, 3);
  }
}
