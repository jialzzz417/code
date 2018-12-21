# code

byte blinks[10];
byte player[10]; 

byte cpblinks = true;
byte playerInput = 0; 
byte level = 3; 

const int hold = 500;

const byte buzzer = A5; 

void setPlayer(byte _guess) {
  player[playerInput] = _guess;
  digitalWrite(_guess,HIGH);
  delay(300);
  digitalWrite(_guess,LOW);
  playerInput++; 
}

void setup() {
  pinMode(9, OUTPUT); 
  pinMode(10, OUTPUT);
  pinMode(11, OUTPUT);

  pinMode(buzzer, OUTPUT);

  //Buttons
  pinMode(6, INPUT_PULLUP); //corresponds to pin 9 LED
  pinMode(8, INPUT_PULLUP); //corresponds to pin 10 LED
  pinMode(7, INPUT_PULLUP); //corresponds to pin 11 LED
}

void loop() {

  if (cpblinks == true) {
    for (int i = 0; i < level; i++) {
      blinks[i] = random(9, 12);
    }
  }
  
  for (int i = 0; i < level; i++) {
    digitalWrite(blinks[i], HIGH);
    delay(hold);
    digitalWrite(blinks[i], LOW);
    delay(hold);
  }

  cpblinks = false; 

  
  while (playerInput < level) {
    if (digitalRead(7) == LOW) {
      setPlayer(9);
    }
    if (digitalRead(8) == LOW) {
      setPlayer(10);
    }
    if (digitalRead(6) == LOW) {
      setPlayer(11);
    }
  }

  //Check 
  for (int i = 0; i < level; i++) {
    if (player[i] == blinks[i]) {
      tone(buzzer, 950);
      delay(50);
      noTone(buzzer);

    } else {   
      tone(buzzer, 150, 1000);
      level = 2;
    }
  }
  delay(2000);
  cpblinks = true;
  playerInput = 0; 
  level++; 

  if (level > 10) {
    for (int i = 0; i < 80; i++) {
      tone(buzzer, 950);
      digitalWrite(10, HIGH);
      digitalWrite(11, LOW);
      delay(20);
      tone(buzzer, 550);
      digitalWrite(11, HIGH);
      digitalWrite(10, LOW);
      delay(20);
      noTone(buzzer);
    }
    level = 3;
  }
}
