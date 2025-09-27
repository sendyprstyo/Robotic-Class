#define IR_KIRI 21 // IR R1
#define IR_TENGAH 19 // IR R2
#define IR_KANAN 18 // IR R3

#define IN1 13  // Motor kiri pin 1
#define IN2 12  // Motor kiri pin 2
#define IN3 14  // Motor kanan pin 1
#define IN4 27  // Motor kanan pin 2

int count = 0;
int prevKiri = 0, prevTengah = 0, prevKanan = 0; // Menyimpan keadaan sebelumnya

void setup() {
  Serial.begin(115200);
  
  pinMode(IR_KIRI, INPUT);
  pinMode(IR_TENGAH, INPUT);
  pinMode(IR_KANAN, INPUT);
  
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
}

void loop() {
  int kiri = !digitalRead(IR_KIRI);
  int tengah = !digitalRead(IR_TENGAH);
  int kanan = !digitalRead(IR_KANAN);
  
  Serial.print(" Count: ");
  Serial.println(count);
  
  if ((kiri == 0 && tengah == 0 && kanan == 0) || (kiri == 1 && tengah == 0 && kanan == 1) || (kiri == 0 && tengah == 1 && kanan == 0)) {
    // MAJU
    maju();
  } else if ((kiri == 1 && tengah == 0 && kanan == 0) || (kiri == 1 && tengah == 1 && kanan == 0)) {
    // BELOK KIRI: kiri mundur, kanan maju
    belokKIRI_TAJAM();
  } else if ((kiri == 0 && tengah == 0 && kanan == 1) || (kiri == 0 && tengah == 1 && kanan == 1)) {
    // BELOK KANAN: kiri maju, kanan mundur
    belokKANAN_TAJAM();
   
  } else if (kiri == 1 && tengah == 1 && kanan == 1) {
    // Kondisi 111 - cek keadaan sebelumnya
    if ((prevKiri == 1 && prevTengah == 1 && prevKanan == 0) || (prevKiri == 1 && prevTengah == 0 && prevKanan == 0)) {
      // Jika sebelumnya 001 atau 011 -> belok kiri
      belokKIRI_TAJAM();
      count += 1;
    } else if ((prevKiri == 0 && prevTengah == 1 && prevKanan == 1) || (prevKiri == 0 && prevTengah == 0 && prevKanan == 1)) {
      // Jika sebelumnya 100 atau 110 -> belok kanan
      belokKANAN_TAJAM();
      count += 1;
    } else if (prevKiri == 0 && prevTengah == 0 && prevKanan == 0) {
      // Jika sebelumnya 000 -> stop
      stopMotor();
    } else {
      // Default (seperti program asli)
      belokKANAN_TAJAM();
      count += 1;
    }
  } else {
    stopMotor();
  }
  
  // Simpan keadaan saat ini sebagai keadaan sebelumnya (kecuali saat 111)
  if (!(kiri == 1 && tengah == 1 && kanan == 1)) {
    prevKiri = kiri;
    prevTengah = tengah;
    prevKanan = kanan;
  }
  
  delay(30);
}

// === Fungsi Motor ===
void maju() {
  // Motor kiri maju
  digitalWrite(IN1, HIGH);  // Motor kiri pin 1: HIGH
  digitalWrite(IN2, LOW);   // Motor kiri pin 2: LOW
  
  // Motor kanan maju
  digitalWrite(IN3, HIGH);  // Motor kanan pin 1: HIGH
  digitalWrite(IN4, LOW);   // Motor kanan pin 2: LOW
}

void belokKIRI_TAJAM() {
  // Motor kiri mundur
  digitalWrite(IN1, LOW);   // Motor kiri pin 1: LOW
  digitalWrite(IN2, HIGH);  // Motor kiri pin 2: HIGH
  
  // Motor kanan maju
  digitalWrite(IN3, HIGH);  // Motor kanan pin 1: HIGH
  digitalWrite(IN4, LOW);   // Motor kanan pin 2: LOW
}

void belokKANAN_TAJAM() {
  // Motor kiri maju
  digitalWrite(IN1, HIGH);  // Motor kiri pin 1: HIGH
  digitalWrite(IN2, LOW);   // Motor kiri pin 2: LOW
  
  // Motor kanan mundur
  digitalWrite(IN3, LOW);   // Motor kanan pin 1: LOW
  digitalWrite(IN4, HIGH);  // Motor kanan pin 2: HIGH
}

void stopMotor() {
  // Matikan semua motor
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
