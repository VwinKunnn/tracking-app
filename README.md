# Langkah 1: Persiapan Awal di CMD
1. Buka Command Prompt.
2. Periksa Instalasi Node.js:
```bash
node -v
npm -v
```
### Jika belum terinstal, unduh dan instal dari situs resmi Node.js >>> https://nodejs.org/en/download

# Buat Folder Proyeknya
```
mkdir tracking-app
cd tracking-app
```
# Inisialisasi Proyek NestJS:
```
npm i -g @nestjs/cli
nest new .
```

# Langkah 2: Instalasi Dependensi
Install semua Dependensi yang ada

```
npm install @nestjs/jwt @nestjs/passport passport passport-jwt @nestjs/websockets socket.io mongoose redis pg typeorm bcrypt dotenv
```
# Langkah 3: Setup Konfigurasi Tanpa File `.env`
```typescript
echo // src/config/config.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class ConfigService {
  get(key: string): string {
    const envs = {
      DATABASE_URL: 'mongodb://localhost:27017/tracking',
      JWT_SECRET: 'rahasia_dev',
      REDIS_URL: 'redis://localhost:6379',
      PORT: '3000'
    };
    return envs[key];
  }
} > src/config/config.service.ts
```
# Langkah 4: Buat Module Utama
Buat Module untuk autentikasi dan tracking:
```
:: Buat auth module
nest generate module auth
nest generate service auth
nest generate controller auth

:: Buat tracking module
nest generate module tracking
nest generate service tracking
nest generate controller tracking
```
# Langkah 5: Implementasi Kode
Contoh implementasi untuk `auth.service.ts`:
```typescript
echo // src/auth/auth.service.ts
import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { ConfigService } from '../config/config.service';

@Injectable()
export class AuthService {
  constructor(
    private jwtService: JwtService,
    private config: ConfigService
  ) {}

  async validateUser (username: string, pass: string): Promise<any> {
    // Mock user data
    const mockUser  = { id: 1, username: 'admin', password: 'admin' };
    if (username === mockUser .username && pass === mockUser .password) {
      return mockUser ;
    }
    return null;
  }

  async login(user: any) {
    const payload = { username: user.username, sub: user.id };
    return {
      access_token: this.jwtService.sign(payload, {
        secret: this.config.get('JWT_SECRET')
      }),
    };
  }
} > src/auth/auth.service.ts
```
# Langkah 6: Jalankan Aplikasi
Perlu Diingat, Pastikan kalian sudah menginstall Mongodb
### 1. Start MongoDB
```
Mongod
```
### 2. Jalankan Aplikasi:
```
npm run start:dev
```
# Langkah 7: Test API via CMD (menggunakan curl)
### 1. Test Login
```
curl -X POST http://localhost:3000/auth/login -H "Content-Type: application/json" -d "{\"username\":\"admin\",\"password\":\"admin\"}"
```
### 1. Test Tracking Endpoint
```
curl -X POST http://localhost:3000/tracking -H "Authorization: Bearer YOUR_TOKEN" -H "Content-Type: application/json" -d "{\"lat\":-6.2,\"lng\":106.8}"
```
# Langkah 8: Push ke GitHub dari CMD
```
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/repo.git
git push -u origin main
```
Semua langkah diatas sudah saya kerjakan kecuali bagian api. Kurangnya Pengetahuan tentang Standar dan Debugging yang terbilang sedikit membingungkan, tetapi selebihnya saya telah bisa. maaf semisalnya cuman bikin readme.me File saja, karena dibagian api saya tidak bisa🙏🙏
