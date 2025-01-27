# การสร้างและ Deploy React SPA ด้วย Vite

บทสอนนี้จะแนะนำวิธีการสร้าง Single Page Application (SPA) โดยใช้ React และ Vite จากนั้นจะ deploy ขึ้น GitHub Pages

## สิ่งที่ต้องมีก่อนเริ่มต้น

- Node.js (เวอร์ชัน 14.18+ หรือสูงกว่า)
- Git ติดตั้งในเครื่อง
- บัญชี GitHub
- โปรแกรม VS Code

## ขั้นตอนที่ 1: สร้างโปรเจค Vite ใหม่

1. เปิด terminal และรันคำสั่ง:
```bash
npm create vite@latest my-react-spa -- --template react
cd my-react-spa
```

2. ติดตั้ง dependencies:
```bash
npm install
```

## ขั้นตอนที่ 2: จัดโครงสร้างโปรเจค

1. โครงสร้างโปรเจคควรเป็นดังนี้:
```
my-react-spa/
├── node_modules/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── pages/
│   ├── App.jsx
│   └── main.jsx
├── index.html
├── package.json
└── vite.config.js
```

2. สร้างไดเรกทอรีที่จำเป็น:
```bash
mkdir src/components src/pages src/utils
```

## ขั้นตอนที่ 3: ติดตั้ง Dependencies ที่จำเป็น

ติดตั้ง React Router และแพ็คเกจที่จำเป็นอื่นๆ:
```bash
npm install react-router-dom @vitejs/plugin-react
```

## ขั้นตอนที่ 4: ตั้งค่า Vite

อัปเดตไฟล์ `vite.config.js`:
```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  base: '/my-react-spa/', // แทนที่ด้วยชื่อ repository ของตัวเอง
})
```

## ขั้นตอนที่ 5: ตั้งค่า React Router

1. สร้างหน้าพื้นฐานใน `src/pages/`:

`src/pages/Home.jsx`:
```jsx
const Home = () => {
  return (
    <div>
      <h1>ยินดีต้อนรับสู่ SPA ของฉัน</h1>
      <p>นี่คือหน้าแรก</p>
    </div>
  )
}

export default Home
```

`src/pages/About.jsx`:
```jsx
const About = () => {
  return (
    <div>
      <h1>เกี่ยวกับเรา</h1>
      <p>เรียนรู้เพิ่มเติมเกี่ยวกับแอปพลิเคชันของเรา</p>
    </div>
  )
}

export default About
```

2. อัปเดต `App.jsx`:
```jsx
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom'
import Home from './pages/Home'
import About from './pages/About'

function App() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li><Link to="/">หน้าแรก</Link></li>
            <li><Link to="/about">เกี่ยวกับเรา</Link></li>
          </ul>
        </nav>

        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </div>
    </Router>
  )
}

export default App
```

## ขั้นตอนที่ 6: ตั้งค่า GitHub

1. สร้าง repository ใหม่บน GitHub ชื่อ `my-react-spa`

2. เริ่มต้น Git และ push โค้ด:
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/your-username/my-react-spa.git
git push -u origin main
```

## ขั้นตอนที่ 7: Deploy ขึ้น GitHub Pages

1. ติดตั้งแพ็คเกจ gh-pages:
```bash
npm install gh-pages --save-dev
```

2. เพิ่มสคริปต์ deploy ใน `package.json`:
```json
{
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

3. Build และ deploy:
```bash
npm run deploy
```

4. ตั้งค่า GitHub Pages:
   - ไปที่การตั้งค่า repository
   - ไปที่ "Pages"
   - เลือก branch "gh-pages" เป็น source
   - บันทึกการเปลี่ยนแปลง

เช็คว่าเว็บไซต์ใช้งานได้ที่: `https://your-username.github.io/my-react-spa`

## เคล็ดลับเพิ่มเติม

### จัดการกับ 404 เมื่อรีเฟรช

เพิ่มไฟล์ 404.html ในไดเรกทอรี `public` พร้อมสคริปต์ redirect:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>กำลังเปลี่ยนเส้นทาง...</title>
    <script>
      sessionStorage.redirect = location.href;
    </script>
    <meta http-equiv="refresh" content="0;URL='/'">
  </head>
  <body>
    กำลังเปลี่ยนเส้นทาง...
  </body>
</html>
```

### ตัวแปรสภาพแวดล้อม

สร้างไฟล์ `.env` สำหรับสภาพแวดล้อมต่างๆ:
```
.env.development
.env.production
```

เข้าถึงตัวแปรโดยใช้: `import.meta.env.VITE_YOUR_VARIABLE`

### การทดสอบ

1. ติดตั้ง dependencies สำหรับการทดสอบ:
```bash
npm install -D vitest jsdom @testing-library/react @testing-library/jest-dom
```

2. เพิ่มสคริปต์ทดสอบใน `package.json`:
```json
{
  "scripts": {
    "test": "vitest"
  }
}
```

## ปัญหาที่พบบ่อยและวิธีแก้ไข

1. **ปัญหาการ Routing**: หากพบปัญหาการ routing ใน production ให้อัปเดต `vite.config.js`:
```javascript
export default defineConfig({
  base: process.env.NODE_ENV === 'production' ? '/my-react-spa/' : '/',
})
```

2. **การโหลด Asset**: ใช้ alias `@` สำหรับการ import:
```javascript
import logo from '@/assets/logo.png'
```

3. **การ Optimize การ Build**: เพิ่มตัวเลือกการ build ใน `vite.config.js`:
```javascript
export default defineConfig({
  build: {
    sourcemap: false,
    chunkSizeWarningLimit: 1000,
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
        },
      },
    },
  },
})
```

## การดูแลรักษาและอัปเดต

1. อัปเดต dependencies เป็นประจำ:
```bash
npm update
```

2. ตรวจสอบความปลอดภัย:
```bash
npm audit
npm audit fix
```

3. อัปเดตการ deploy ให้ทันสมัย:
```bash
git pull origin main
npm run deploy
```

ก่อน Deploy ไปยัง Production อย่าลืมทดสอบแอปพลิเคชันอย่างละเอียดก่อน ควรรันเซิร์ฟเวอร์ในโหมดสำหรับการพัฒนา (`npm run dev`) ทดสอบในเครื่องตัวเองก่อน ให้แน่ใจว่าทุกเส้นทางและฟีเจอร์ทำงานเป็นไปตามที่คาดหวัง
