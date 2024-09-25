const int PhotoresistorPin = A0;
const int LedPin = 6;
const int Threshold = 700;

unsigned long startTimeLight = 0;
unsigned long startTimeDark = 0;
unsigned long durationLight = 0;
unsigned long durationDark = 0;
bool isLightOn = false;
bool isDarkOn = false;
String morseBuffer = "";

void setup() {
  pinMode(LedPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int lightLevel = analogRead(PhotoresistorPin);
  delay(5);



  if (lightLevel > Threshold) {
    if (!isLightOn) {
      startTimeLight = millis();
      isLightOn = true;
    }
    if (isDarkOn) {
      durationDark = millis() - startTimeDark;
      if (durationDark >= 150 && durationDark <= 250) {
        // Dot
      } else if (durationDark >= 500 && durationDark <= 1000) {
        // Dash
        morseBuffer += "|";
      } else if (durationDark >= 1300 && durationDark <= 1900) {
        // Space between words
        morseBuffer += "/";
      }
      isDarkOn = false;
    }
  } else {
    if (isLightOn) {
      durationLight = millis() - startTimeLight;


      if (durationLight >= 150 && durationLight <= 250) {
        // Dot
        morseBuffer += ".";
      } else if (durationLight >= 500 && durationLight <= 700) {
        // Dash
        morseBuffer += "-";
      }
      isLightOn = false;
    }
    if (!isDarkOn) {
      startTimeDark = millis();
      isDarkOn = true;
    }
  }

  // Check for overtime and decode Morse buffer
  if (millis() - startTimeDark > 2500 && morseBuffer.length() > 0) {
    Serial.print("Morse buffer: ");
    Serial.println(morseBuffer);
    decodeAndPrint(morseBuffer);
    morseBuffer = "";  // Reset buffer after decoding
  }
}

void decodeAndPrint(String morseCode) {
  if (morseCode.length() != 0) {
    String decodedMessage = "";
    String currentSymbol = "";
    for (int i = 0; i < morseCode.length(); i++) {
      if (morseCode.charAt(i) == '|') {
        // Decode and add letter to decoded message
        decodedMessage += decodeMorse(currentSymbol);
        currentSymbol = "";
      } else if (morseCode.charAt(i) == '/') {
        // Add spacing for words
        decodedMessage += decodeMorse(currentSymbol);
        decodedMessage += " ";
        currentSymbol = "";
      } else if (i == morseCode.length() - 1) {
        currentSymbol += morseCode.charAt(i);
        decodedMessage += decodeMorse(currentSymbol);

      } else {
        currentSymbol += morseCode.charAt(i);
      }
    }
    // Print the decoded message
    Serial.println("Decoded Morse: " + decodedMessage);
  }
}

char decodeMorse(String symbol) {
  // Define Morse code lookup table
  String morse[] = { ".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.." };
  char letters[] = { 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z' };
  for (int i = 0; i < 26; i++) {
    if (symbol.equals(morse[i])) {
      return letters[i];
    }
  }
  return '?';  // Return '?' for unknown symbols
}
