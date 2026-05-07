#include <DHT.h>                            // Librería para manejar sensores DHT
#include <DHT_U.h>                      // Librería unificada para sensores DHT
#include <LiquidCrystal_I2C.h>                // Librería para controlar pantallas LCD I2C


LiquidCrystal_I2C lcd(0x27,16,2);                 // Crea el objeto LCD con dirección 0x27, 16 columnas y 2 filas

int DHTPIN = 8;                  // Pin digital donde está conectado el DHT11
int LM;                // Variable para guardar lectura analógica del LM35
int T_DHT;                // Variable para almacenar temperatura del DHT11
float T_LM;                // Variable flotante para temperatura calculada del LM35
int HUMEDAD;                // Variable para almacenar humedad del DHT11

DHT dht (DHTPIN, DHT11);                 // Crea el objeto DHT indicando pin y tipo de sensor

void setup(){                 // Función de configuración inicial

  Serial.begin(9600);                  // Inicia comunicación serial a 9600 baudios

  dht.begin();                  // Inicializa el sensor DHT11
  lcd.init();                  // Inicializa la pantalla LCD
  lcd.backlight();                  // Enciende la luz de fondo de la LCD
  lcd.clear();                  // Limpia la pantalla LCD
}


void loop(){                // Función principal que se repite continuamente

  T_DHT = dht.readTemperature();                 // Lee la temperatura del DHT11
  HUMEDAD = dht.readHumidity();                   // Lee la humedad del DHT11

  Serial.print("Temperatura DHT11: ");                  // Imprime texto en el monitor serial
  Serial.print(T_DHT);                  // Muestra temperatura del DHT11
  Serial.print(" | Humedad DHT11: ");                  // Imprime separador y texto de humedad
  Serial.println(HUMEDAD);                  // Muestra humedad del DHT11

  LM = analogRead(A0);                  // Lee valor analógico del LM35 en A0
  T_LM = ((LM * 5000.0) / 1023) / 10;                  // Convierte lectura analógica a temperatura en °C
  
  Serial.print("Temperatura LM35: ");                  // Imprime texto de temperatura LM35
  Serial.println(T_LM,1);                  // Muestra temperatura LM35 con 1 decimal
  lcd.setCursor(0,0);                  // Posiciona cursor en columna 0 fila 0
  lcd.print("DHT11: ");                  // Muestra texto en LCD

  lcd.print(T_DHT, 1);                  // Muestra temperatura DHT11
  lcd.print("C");                  // Muestra símbolo de grados simplificado
  lcd.print(" H: ");                  // Muestra etiqueta de humedad
  
  lcd.print(HUMEDAD);                  // Muestra humedad
  lcd.setCursor(2,1);                  // Posiciona cursor en columna 2 fila 1
  lcd.print("LM35 : ");                  // Muestra texto LM35
  lcd.print(T_LM, 1);                  // Muestra temperatura LM35
  lcd.print("C");                  // Muestra símbolo de grados

  delay(1000);                  // Espera 1 segundo antes de repetir

}