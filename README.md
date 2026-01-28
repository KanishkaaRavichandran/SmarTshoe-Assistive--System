# SmarTshoe-Assistive--System
Smart Shoe is a wearable safety companion that detects obstacles before you do. Powered by embedded systems and sensors, it identifies hazards in real time and alerts the user through vibration or buzzer feedback. Compact, efficient, and innovative, this project highlights the power of smart wearables in everyday mobility and assistive technology.
#define trigPinL 34
#define echoPinL 35
#define trigPinR 32
#define echoPinR 33
#define irPin 23

void setup() {
  Serial.begin(9600);         // Serial Monitor (Laptop)
  Serial2.begin(9600);          // HC-05 Bluetooth
  pinMode(trigPinL, OUTPUT);
  pinMode(echoPinL, INPUT);
  pinMode(trigPinR, OUTPUT);
  pinMode(echoPinR, INPUT);
  pinMode(irPin, INPUT);

  Serial.println("🟢 Smart Shoe Ready");
  Serial2.println("🟢 Smart Shoe Ready"); // Send to phone
}

long getDistance(int trigPin, int echoPin) {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  long duration = pulseIn(echoPin, HIGH, 30000);
  return duration * 0.034 / 2;
}

void loop() {
  long leftDist = getDistance(trigPinL, echoPinL);
  long rightDist = getDistance(trigPinR, echoPinR);
  bool irObstacle = digitalRead(irPin) == LOW;

  Serial.print("L:");
  Serial.print(leftDist);
  Serial.print(" R:");
  Serial.print(rightDist);
  Serial.print(" IR:");
  Serial.println(irObstacle ? "Blocked" : "Clear");

  if (leftDist > 0 && leftDist < 30) {
    Serial2.println("Obstacle on the LEFT");
    Serial.println("📡 Sent to phone: Obstacle on the LEFT");
    delay(1000);
  } else if (rightDist > 0 && rightDist < 30) {
    Serial2.println("Obstacle on the RIGHT");
    Serial.println("📡 Sent to phone: Obstacle on the RIGHT");
    delay(1000);
  } else if (irObstacle) {
    Serial2.println("Obstacle AHEAD");
    Serial.println("📡 Sent to phone: Obstacle AHEAD");
    delay(1000);
  }

  delay(200);  
}
