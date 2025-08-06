# RC Car Controller - Sistem Kontrol Mobil RC Berbasis ESP32-CAM

Aplikasi web modern untuk mengontrol mobil RC berbasis ESP32-CAM melalui internet menggunakan protokol MQTT. Sistem ini menyediakan interface yang intuitif untuk mengontrol pergerakan mobil, melihat live stream kamera, dan memantau status sistem secara real-time dengan fokus pada pengaturan jarak minimum yang sederhana namun efektif.

## 🚗 Fitur Utama

### **Kontrol Real-time**
- **Kontrol Keyboard**: Gunakan tombol WASD untuk mengontrol pergerakan mobil
- **Kontrol UI**: Interface tombol yang responsif untuk kontrol manual
- **Kontrol Kecepatan**: Slider untuk mengatur kecepatan motor (0-100%)
- **Kontrol Pencahayaan**: Slider untuk mengatur intensitas LED flash (0-100%)

### **Live Camera Stream**
- **Streaming Video Langsung**: Real-time video dari ESP32-CAM
- **Rotasi Gambar**: Kemampuan memutar gambar 90° untuk orientasi yang tepat
- **Indikator LIVE**: Visual feedback saat streaming aktif
- **Optimasi Bandwidth**: Kompresi JPEG yang dioptimalkan untuk streaming smooth

### **Mode Autonomous**
- **Navigasi Otomatis**: Sistem navigasi mandiri dengan sensor jarak
- **Pengaturan Jarak Minimum**: Konfigurasi threshold jarak untuk stop/mundur
- **Obstacle Avoidance**: Algoritma penghindaran halangan yang cerdas
- **State Machine**: Sistem state yang terstruktur untuk navigasi yang predictable

### **System Monitoring**
- **Real-time Logging**: Panel log sistem dengan database persistence
- **Sensor Monitoring**: Pembacaan sensor jarak proximity secara real-time
- **Status Monitoring**: Monitoring uptime, memory, dan koneksi
- **Connection Status**: Indikator status koneksi MQTT real-time

### **Pengaturan Jarak Minimum**
- **Interface Sederhana**: Satu input field untuk jarak minimum (5-300cm)
- **Validasi Real-time**: Validasi input dengan feedback visual
- **Persistent Storage**: Penyimpanan di browser localStorage dan Arduino EEPROM
- **Aplikasi Langsung**: Perubahan diterapkan langsung tanpa restart

### **Autentikasi & Keamanan**
- **Supabase Authentication**: Sistem login yang aman
- **Row Level Security**: Database security yang ketat
- **Secure WebSocket**: Koneksi MQTT melalui WSS

## 🛠 Teknologi yang Digunakan

### **Frontend Stack**
- **React 18**: Library UI modern dengan hooks
- **TypeScript**: Type safety dan developer experience yang lebih baik
- **Vite**: Build tool yang cepat dan modern
- **Tailwind CSS**: Utility-first CSS framework
- **Lucide React**: Icon library yang konsisten

### **Backend & Services**
- **Supabase**: Backend-as-a-Service untuk authentication dan database
- **MQTT over WebSocket**: Real-time communication protocol
- **HiveMQ Cloud**: Managed MQTT broker service

### **Hardware**
- **ESP32-CAM**: Microcontroller dengan built-in camera
- **HC-SR04**: Ultrasonic distance sensor
- **L298N**: Motor driver untuk kontrol pergerakan
- **LED Flash**: Pencahayaan yang dapat dikontrol

## 📋 Prasyarat

### **Software Requirements**
- **Node.js**: Versi 16 atau lebih baru
- **npm atau yarn**: Package manager
- **Browser Modern**: Chrome, Firefox, Safari, atau Edge terbaru

### **Hardware Requirements**
- **ESP32-CAM Module**: Dengan antenna WiFi yang baik
- **Ultrasonic Sensor**: HC-SR04 atau kompatibel
- **Motor Driver**: L298N atau kompatibel
- **DC Motors**: 2 buah untuk pergerakan
- **Power Supply**: 7.4V Li-Po battery atau power adapter
- **Chassis**: Frame mobil RC yang sesuai

### **Service Requirements**
- **Akun Supabase**: Untuk authentication dan database
- **MQTT Broker**: HiveMQ Cloud atau broker MQTT lainnya
- **WiFi Network**: Koneksi internet yang stabil

## 🚀 Instalasi dan Setup

### **1. Clone Repository**
```bash
git clone <repository-url>
cd rc-car-controller
```

### **2. Install Dependencies**
```bash
# Menggunakan npm
npm install

# Atau menggunakan yarn
yarn install
```

### **3. Setup Environment Variables**
Buat file `.env` di root directory:
```env
VITE_SUPABASE_URL=your_supabase_project_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
```

**Cara mendapatkan Supabase credentials:**
1. Buat akun di [supabase.com](https://supabase.com)
2. Buat project baru
3. Pergi ke Settings > API
4. Copy URL dan anon key ke file `.env`

### **4. Setup Supabase Database**
Jalankan migration untuk membuat tabel yang diperlukan:
```bash
# Jika menggunakan Supabase CLI
supabase db push

# Atau import manual melalui Supabase Dashboard
# Import file: supabase/migrations/20250628170401_old_bar.sql
```

### **5. Konfigurasi MQTT**
Edit file `src/App.tsx` dan sesuaikan konfigurasi MQTT:
```typescript
const mqttConfig = {
  brokerUrl: 'wss://your-mqtt-broker-url:8884/mqtt',
  options: {
    clientId: getStableClientId(),
    username: 'your-mqtt-username',
    password: 'your-mqtt-password',
    clean: true,
    connectTimeout: 4000,
  }
};
```

**Setup HiveMQ Cloud:**
1. Daftar di [hivemq.com](https://www.hivemq.com/mqtt-cloud-broker/)
2. Buat cluster baru
3. Buat credentials untuk WebSocket
4. Copy URL dan credentials ke konfigurasi

### **6. Upload Arduino Code**
1. Install Arduino IDE dan library yang diperlukan:
   - ESP32 Board Package
   - ArduinoJson
   - PubSubClient
   - WiFiClientSecure
2. Copy kode dari `CODE_ARDUINO_ENHANCED.txt`
3. Sesuaikan konfigurasi WiFi dan MQTT
4. Upload ke ESP32-CAM

### **7. Jalankan Development Server**
```bash
npm run dev
```
Aplikasi akan berjalan di `http://localhost:5173`

## 📁 Struktur Project

```
rc-car-controller/
├── src/
│   ├── components/          # Komponen UI reusable
│   │   ├── DistanceSettingsPanel.tsx  # Panel pengaturan jarak
│   │   ├── LoadingSpinner.tsx         # Loading indicator
│   │   ├── LoginForm.tsx              # Form autentikasi
│   │   ├── LogPanel.tsx               # Panel monitoring logs
│   │   ├── MQTTStatus.tsx             # Status koneksi MQTT
│   │   └── UserProfile.tsx            # Profile pengguna
│   ├── contexts/            # React Context untuk state management
│   │   └── AuthContext.tsx            # Context autentikasi
│   ├── hooks/               # Custom React hooks
│   │   ├── useMQTT.ts                 # Hook untuk MQTT communication
│   │   └── useDistanceSettings.ts     # Hook untuk pengaturan jarak
│   ├── lib/                 # Konfigurasi library eksternal
│   │   └── supabase.ts                # Konfigurasi Supabase client
│   ├── types/               # TypeScript type definitions
│   │   └── settings.ts                # Types untuk pengaturan jarak
│   ├── utils/               # Utility functions
│   │   └── distanceValidation.ts      # Validasi pengaturan jarak
│   ├── App.tsx              # Komponen utama aplikasi
│   ├── main.tsx             # Entry point aplikasi
│   └── index.css            # Global styles dan Tailwind
├── supabase/
│   └── migrations/          # Database migrations
├── public/                  # Static assets
├── CODE_ARDUINO_ENHANCED.txt # Kode Arduino lengkap
├── package.json             # Dependencies dan scripts
├── tailwind.config.js       # Konfigurasi Tailwind CSS
├── tsconfig.json           # Konfigurasi TypeScript
└── vite.config.ts          # Konfigurasi Vite
```

## 🎮 Cara Penggunaan

### **Login dan Autentikasi**
1. **Buka aplikasi** di browser
2. **Masukkan email dan password** yang telah terdaftar di Supabase
3. **Klik "Sign In"** untuk masuk ke dashboard

### **Kontrol Manual Mobil RC**

#### **Kontrol Keyboard**
- **W**: Maju (Forward)
- **S**: Mundur (Backward)
- **A**: Belok kiri (Turn Left)
- **D**: Belok kanan (Turn Right)
- **Lepas tombol**: Berhenti otomatis

#### **Kontrol UI**
- **Tombol panah**: Kontrol arah pergerakan
- **Tombol merah (kotak)**: Stop darurat
- **Slider Speed**: Mengatur kecepatan motor (0-100%)
- **Slider Flash**: Mengatur intensitas LED (0-100%)

### **Mode Autonomous**
1. **Klik "Start Autonomous"** untuk mengaktifkan mode otomatis
2. **Mobil akan bergerak otomatis** berdasarkan sensor jarak
3. **Sistem akan menghindari halangan** secara otomatis
4. **Klik "Stop Autonomous"** untuk kembali ke mode manual

#### **Algoritma Autonomous Navigation:**
- **Forward**: Bergerak maju jika jarak > threshold minimum
- **Obstacle Detection**: Deteksi halangan saat jarak ≤ threshold
- **Stop & Assess**: Berhenti dan evaluasi situasi
- **Backup**: Mundur jika jarak terlalu dekat
- **Turn Left**: Coba belok kiri untuk mencari jalur
- **Turn Right**: Jika kiri terblokir, coba belok kanan
- **Resume**: Lanjutkan maju jika jalur bebas

### **Pengaturan Jarak Minimum**
1. **Buka panel "Pengaturan Jarak"** di sidebar kiri
2. **Klik untuk expand** panel pengaturan
3. **Atur jarak minimum** (5-300cm) sesuai kebutuhan
4. **Validasi real-time** akan menampilkan error jika input tidak valid
5. **Klik "Simpan Pengaturan"** untuk menerapkan perubahan
6. **Pengaturan tersimpan** di browser dan Arduino EEPROM

#### **Panduan Pengaturan Jarak:**
- **5-10cm**: Untuk navigasi presisi tinggi (indoor, area sempit)
- **15-20cm**: Pengaturan standar untuk navigasi umum
- **25-30cm**: Untuk kecepatan tinggi atau area outdoor
- **50cm+**: Untuk deteksi dini halangan besar

### **Monitoring dan Logging**
- **Live Camera**: Monitor streaming video real-time dari ESP32-CAM
- **System Logs**: Pantau pesan log sistem di panel kanan
- **Sensor Data**: Lihat pembacaan sensor jarak dalam cm
- **Connection Status**: Status koneksi MQTT di header
- **System Status**: Uptime, memory usage, dan signal strength

## 📡 MQTT Communication Protocol

### **Topics yang Digunakan**

| Topic | Direction | Deskripsi | Payload Format |
|-------|-----------|-----------|----------------|
| `esp32/car/control/move` | Web → ESP32 | Kontrol pergerakan | `forward`, `backward`, `left`, `right`, `stop` |
| `esp32/car/control/speed` | Web → ESP32 | Kontrol kecepatan | Integer `100-255` |
| `esp32/car/control/flash` | Web → ESP32 | Kontrol LED flash | Integer `0-255` |
| `esp32/car/config/distance` | Web → ESP32 | Pengaturan jarak minimum | JSON: `{"minDistance": 15}` |
| `esp32/car/command/autonomous` | Web → ESP32 | Mode autonomous | `on`, `off` |
| `esp32/car/status` | ESP32 → Web | Status sistem | JSON status object |
| `esp32/cam/stream` | ESP32 → Web | Stream kamera | Binary JPEG data |
| `esp32/car/log` | ESP32 → Web | Log messages | String message |
| `esp32/car/sensor/distance` | ESP32 → Web | Data sensor jarak | Float distance in cm |

### **Message Formats**

#### **Distance Configuration**
```json
{
  "minDistance": 15,
  "timestamp": "2025-01-27T10:30:00.000Z"
}
```

#### **System Status**
```json
{
  "uptime": 3600,
  "rssi": -45,
  "heap": 234567,
  "auto": true,
  "dist": 25,
  "state": 0,
  "obstacle": false,
  "settings": {
    "min": 15
  }
}
```

## 🔧 Konfigurasi Hardware ESP32

### **Pin Configuration**
```cpp
// Motor Control
#define MOTOR_EN 2    // PWM untuk kecepatan motor
#define IN1 14        // Motor A direction 1
#define IN2 15        // Motor A direction 2
#define IN3 12        // Motor B direction 1
#define IN4 13        // Motor B direction 2

// Sensor dan Aktuator
#define FLASH_PIN 4   // LED Flash control
#define TRIG_PIN 0    // Ultrasonic trigger
#define ECHO_PIN 3    // Ultrasonic echo

// Camera Pins (AI Thinker ESP32-CAM)
#define PWDN_GPIO_NUM 32
#define RESET_GPIO_NUM -1
#define XCLK_GPIO_NUM 0
// ... (pin kamera lainnya sesuai kode Arduino)
```

### **Wiring Diagram**

#### **Motor Driver (L298N)**
```
ESP32-CAM    L298N
GPIO2    →   ENA (PWM)
GPIO14   →   IN1
GPIO15   →   IN2
GPIO12   →   IN3
GPIO13   →   IN4
GND      →   GND
5V       →   VCC
```

#### **Ultrasonic Sensor (HC-SR04)**
```
ESP32-CAM    HC-SR04
GPIO0    →   Trig
GPIO3    →   Echo
5V       →   VCC
GND      →   GND
```

#### **LED Flash**
```
ESP32-CAM    LED
GPIO4    →   LED+ (melalui resistor 220Ω)
GND      →   LED-
```

### **Power Requirements**
- **ESP32-CAM**: 3.3V/5V, ~200mA
- **Motors**: 6-12V, 1-2A per motor
- **Sensor**: 5V, ~15mA
- **LED**: 3.3V, ~20mA
- **Total**: Minimum 7.4V 3A power supply

## 🔒 Keamanan dan Best Practices

### **Authentication Security**
- **Supabase Auth**: Industry-standard authentication
- **Row Level Security**: Database-level access control
- **Secure Sessions**: JWT token management
- **Password Policies**: Strong password requirements

### **Network Security**
- **WSS Protocol**: Encrypted WebSocket connections
- **MQTT Credentials**: Username/password authentication
- **Environment Variables**: Sensitive data protection
- **CORS Configuration**: Proper cross-origin policies

### **Hardware Security**
- **Input Validation**: All sensor inputs validated
- **Safe Defaults**: Fallback to safe values on error
- **Emergency Stop**: Multiple stop mechanisms
- **Timeout Protection**: Auto-stop on connection loss

### **Data Privacy**
- **Local Storage**: Settings stored locally when possible
- **Minimal Data**: Only necessary data transmitted
- **Log Rotation**: Automatic cleanup of old logs
- **No Personal Data**: No personal information stored

## 🚀 Deployment

### **Build untuk Production**
```bash
# Build aplikasi
npm run build

# Preview build
npm run preview
```

### **Deploy ke Netlify**
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --prod --dir=dist
```

### **Deploy ke Vercel**
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel --prod
```

### **Environment Variables untuk Production**
```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

## 🐛 Troubleshooting

### **Masalah Koneksi MQTT**

#### **Gejala**: Status "Disconnected" atau "Connection Error"
**Solusi**:
1. Periksa URL broker MQTT dan port (biasanya 8884 untuk WSS)
2. Verifikasi username dan password MQTT
3. Pastikan firewall tidak memblokir WebSocket connections
4. Cek quota dan limits di HiveMQ Cloud dashboard

#### **Gejala**: Koneksi terputus-putus
**Solusi**:
1. Periksa kualitas koneksi internet
2. Sesuaikan `connectTimeout` dan `keepAlive` settings
3. Monitor network latency ke broker

### **Masalah Autentikasi Supabase**

#### **Gejala**: Login gagal atau error "Invalid credentials"
**Solusi**:
1. Verifikasi email dan password di Supabase Dashboard
2. Periksa konfigurasi RLS policies
3. Pastikan environment variables benar
4. Cek Supabase project status

#### **Gejala**: Session expired atau auto-logout
**Solusi**:
1. Periksa JWT expiration settings di Supabase
2. Implement token refresh mechanism
3. Verifikasi browser localStorage permissions

### **Masalah Camera Stream**

#### **Gejala**: "Menunggu Stream..." terus menerus
**Solusi**:
1. Periksa koneksi ESP32-CAM ke WiFi
2. Verifikasi konfigurasi kamera di Arduino code
3. Cek topic MQTT untuk camera stream
4. Monitor memory usage di ESP32

#### **Gejala**: Stream lag atau frame drops
**Solusi**:
1. Kurangi `jpeg_quality` di konfigurasi kamera
2. Sesuaikan `frameInterval` untuk frame rate
3. Periksa bandwidth network
4. Optimize MQTT buffer size

### **Masalah Hardware ESP32**

#### **Gejala**: ESP32 restart terus-menerus
**Solusi**:
1. Periksa power supply (minimum 5V 2A)
2. Verifikasi wiring connections
3. Monitor serial output untuk error messages
4. Cek overheating pada ESP32

#### **Gejala**: Sensor jarak tidak akurat
**Solusi**:
1. Periksa wiring HC-SR04
2. Kalibrasi formula `duration / 2 / 29.1`
3. Pastikan tidak ada interferensi
4. Cek mounting sensor (hindari getaran)

#### **Gejala**: Motor tidak bergerak
**Solusi**:
1. Verifikasi wiring L298N motor driver
2. Cek power supply untuk motor (6-12V)
3. Test PWM output dengan multimeter
4. Periksa kondisi motor dan gearbox

### **Masalah Pengaturan Jarak**

#### **Gejala**: Pengaturan tidak tersimpan
**Solusi**:
1. Periksa browser localStorage permissions
2. Verifikasi koneksi MQTT untuk kirim ke ESP32
3. Cek EEPROM initialization di Arduino
4. Monitor log untuk error messages

#### **Gejala**: Validasi input error
**Solusi**:
1. Pastikan input dalam range 5-300cm
2. Gunakan angka positif saja
3. Hindari karakter non-numerik
4. Refresh browser jika validation stuck

## 📊 Performance Optimization

### **Frontend Optimization**
- **Code Splitting**: Lazy loading untuk komponen besar
- **Image Optimization**: Compress dan optimize assets
- **Bundle Analysis**: Monitor bundle size dengan `npm run build`
- **Caching Strategy**: Implement service worker untuk offline support

### **MQTT Optimization**
- **QoS Settings**: Gunakan QoS 0 untuk real-time data
- **Buffer Size**: Sesuaikan buffer size berdasarkan payload
- **Connection Pooling**: Reuse connections untuk efficiency
- **Message Compression**: Compress large payloads jika perlu

### **Arduino Optimization**
- **Memory Management**: Monitor heap usage dan optimize
- **Task Scheduling**: Gunakan FreeRTOS tasks untuk multitasking
- **Power Management**: Implement sleep modes untuk battery life
- **Watchdog Timer**: Auto-recovery dari hang conditions

## 🤝 Contributing

### **Development Workflow**
1. Fork repository ini
2. Buat branch fitur baru (`git checkout -b feature/amazing-feature`)
3. Commit perubahan (`git commit -m 'Add amazing feature'`)
4. Push ke branch (`git push origin feature/amazing-feature`)
5. Buat Pull Request

### **Code Standards**
- **TypeScript**: Gunakan strict mode dan proper typing
- **ESLint**: Follow linting rules yang sudah dikonfigurasi
- **Prettier**: Format code secara konsisten
- **Commit Messages**: Gunakan conventional commit format

### **Testing Guidelines**
- **Unit Tests**: Test individual functions dan components
- **Integration Tests**: Test MQTT communication dan API calls
- **Hardware Tests**: Test dengan hardware setup yang sebenarnya
- **Performance Tests**: Monitor memory usage dan response time

## 📄 Lisensi

Project ini menggunakan lisensi MIT. Lihat file `LICENSE` untuk detail lengkap.

## 📞 Support dan Komunitas

### **Dokumentasi**
- **GitHub Wiki**: Dokumentasi teknis lengkap
- **API Reference**: Dokumentasi MQTT topics dan payloads
- **Hardware Guide**: Panduan wiring dan assembly

### **Community Support**
- **GitHub Issues**: Report bugs dan request features
- **Discussions**: Tanya jawab dan sharing pengalaman
- **Discord Server**: Real-time chat dengan komunitas

### **Professional Support**
- **Email Support**: support@rccarcontroller.com
- **Consulting**: Custom development dan integration
- **Training**: Workshop dan training sessions

---

**Dibuat dengan ❤️ menggunakan React + TypeScript + ESP32-CAM**

*Terakhir diupdate: Januari 2025*