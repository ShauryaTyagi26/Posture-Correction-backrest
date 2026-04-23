#define sensorUpper A0
#define sensorLower A1
#define buzzer 8

// Adjust these after calibration
int thresholdUpper = 520;
int thresholdLower = 500;

// For stable readings
int readSensorAverage(int pin) {
  long sum = 0;
  for (int i = 0; i < 10; i++) {
    sum += analogRead(pin);
    delay(5);
  }
  return sum / 10;
}

void setup() {
  pinMode(buzzer, OUTPUT);
  digitalWrite(buzzer, LOW);
  Serial.begin(9600);
}

void loop() {
  int upperValue = readSensorAverage(sensorUpper);
  int lowerValue = readSensorAverage(sensorLower);

  Serial.print("Upper: ");
  Serial.print(upperValue);
  Serial.print(" | Lower: ");
  Serial.println(lowerValue);

  bool badPosture = false;

  // Check posture
  if (upperValue < thresholdUpper || lowerValue < thresholdLower) {
    badPosture = true;
  }

  // Alert system
  if (badPosture) {
    digitalWrite(buzzer, HIGH);
  } else {
    digitalWrite(buzzer, LOW);
  }

  delay(200);
}
