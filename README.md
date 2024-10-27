# Codigo-proyecto
#include <Servo.h>

// Definición de pines del sensor ultrasónico
const int trigPin = 10;
const int echoPin = 11;
const int buzzer = 9;

// Variables para la duración y distancia
long duration;
int distance;
int max = 9; // Distancia máxima
int min = 1;
;  // Distancia mínima

Servo myServo; // Creas una variable servo

void setup() {
  pinMode(trigPin, OUTPUT); // Configura trigPin como salida
  pinMode(echoPin, INPUT);  // Configura echoPin como entrada
  pinMode(buzzer, OUTPUT);  // Configura buzzer como salida

  Serial.begin(9600);
  myServo.attach(12); // Conecta el servo en el pin 12
}

void loop() {  
  // Rotación del servo de 15 a 180 grados
  for (int i = 1; i <= 180; i++) {  
    myServo.write(i);
    delay(30);

    distance = calculateDistance(); // Calcular la distancia
    
    Serial.print(i); // Envía el ángulo actual por el puerto serial
    Serial.print(","); // Envía una coma para separar valores
    Serial.print(distance); // Envía la distancia calculada por el puerto serial
    Serial.print("."); // Punto para finalizar los datos seriales

    // Verificar si la distancia está dentro de los límites
    if (distance >= min && distance <= max) {
      digitalWrite(buzzer, HIGH); // Enciende el buzzer si la distancia está dentro del rango
    } else {
      digitalWrite(buzzer, LOW);  // Apaga el buzzer si la distancia está fuera del rango
    }
  }

  // Rotación inversa del servo de 180 a 15 grados
  for (int i = 180; i > 1; i--) {  
    myServo.write(i);
    delay(30);

    distance = calculateDistance(); // Calcular la distancia
    
    Serial.print(i); // Envía el ángulo actual por el puerto serial
    Serial.print(","); // Envía una coma para separar valores
    Serial.print(distance); // Envía la distancia calculada por el puerto serial
    Serial.print("."); // Punto para finalizar los datos seriales

    // Verificar si la distancia está dentro de los límites
    if (distance >= min && distance <= max) {
      digitalWrite(buzzer, HIGH); // Enciende el buzzer si la distancia está dentro del rango
    } else {
      digitalWrite(buzzer, LOW);  // Apaga el buzzer si la distancia está fuera del rango
    }
  }
}

// Función para calcular la distancia usando el sensor ultrasónico
int calculateDistance() {
  // Envía un pulso al pin trig para activar el sensor
  digitalWrite(trigPin, LOW); 
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Lee la duración del pulso en el pin echo
  duration = pulseIn(echoPin, HIGH);
  
  // Calcula la distancia en cm (velocidad del sonido = 0.034 cm/us)
  distance = duration * 0.034 / 2;

  return distance;
}
