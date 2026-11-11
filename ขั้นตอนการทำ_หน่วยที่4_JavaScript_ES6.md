# ขั้นตอนการทำงาน หน่วยที่ 4: JavaScript Modern (ES6+) สำหรับ Frontend

วิชา การออกแบบและพัฒนาเว็บไซต์ (21901-2013) — ทฤษฎี 1 ชม. / ปฏิบัติ 2 ชม.

## ภาพรวมงานในหน่วยนี้

| เอกสาร | ลักษณะงาน | เวลา | คะแนน |
|---|---|---|---|
| ใบงานที่ 4 | ระบบแสดงรายการบทความด้วย ES6+ และ Fetch API (รายบุคคล) | 120 นาที | 5 |
| ใบกิจกรรมที่ 4 | กิจกรรมกลุ่ม "นักสืบโค้ด ES6+" (ทีมละ 3–4 คน) | 60 นาที | 10 |
| ใบมอบหมายงานที่ 4 | Mini Project "Blog Reader" (รายบุคคล ทำนอกเวลา 2–3 ชม.) | ส่งก่อนคาบสัปดาห์ที่ 5 | 10 |

**เครื่องมือที่ต้องมี:** VS Code, เบราว์เซอร์ (Chrome/Edge) เปิด DevTools > Console (F12), อินเทอร์เน็ต (ใช้ API ทดสอบ `jsonplaceholder.typicode.com`)

**ข้อควรระวังทั่วไป**
- พิมพ์โค้ดเอง ไม่คัดลอกจากเพื่อน
- ตรวจวงเล็บ ปีกกา เครื่องหมายคำพูดให้ครบคู่ ถ้า error ให้อ่านข้อความสีแดงใน Console ก่อน
- แก้โค้ดทุกครั้งต้องบันทึก (Ctrl + S) แล้วรีเฟรช (F5)
- JSONPlaceholder เป็น API ทดสอบ การ POST จะไม่บันทึกจริง
- ไม่ใส่ข้อมูลส่วนตัวหรือรหัสผ่านจริงในโค้ด

---

# ส่วนที่ 1: ใบงานที่ 4 — ระบบแสดงรายการบทความ

## ตอนที่ 0: เตรียมไฟล์งาน (5 นาที)

1. สร้างโฟลเดอร์ `worksheet4` แล้วเปิดด้วย VS Code
2. สร้างไฟล์ `index.html`

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>ใบงานที่ 4</title>
</head>
<body>
  <h1>รายการบทความ</h1>
  <p id="status">กำลังโหลด...</p>
  <ul id="post-list"></ul>
  <script src="app.js"></script>
</body>
</html>
```

3. สร้างไฟล์ `app.js` (เว้นว่างไว้ก่อน) เปิด `index.html` ในเบราว์เซอร์ กด F12 เลือกแท็บ Console

## ตอนที่ 1: Arrow Functions, Destructuring, Spread/Rest (30 นาที)

พิมพ์ข้อมูลนี้ไว้บนสุดของ `app.js` (ใช้ในตอนที่ 1–2)

```js
const posts = [
  { id: 1, title: 'เริ่มต้นเรียน React', author: 'สมชาย', category: 'React', likes: 12 },
  { id: 2, title: 'รู้จัก Tailwind CSS', author: 'สมหญิง', category: 'CSS', likes: 8 },
  { id: 3, title: 'JavaScript ES6+ ที่ควรรู้', author: 'สมชาย', category: 'JavaScript', likes: 25 },
  { id: 4, title: 'สร้าง REST API ด้วย Express', author: 'มานะ', category: 'Backend', likes: 15 },
];
```

**1.1 Arrow Function `getSummary`** — คืนข้อความ "ชื่อบทความ โดย ผู้เขียน"

```js
const getSummary = (post) => `${post.title} โดย ${post.author}`;
console.log(getSummary(posts[0]));   // เริ่มต้นเรียน React โดย สมชาย
```

**1.2 Destructuring** — แตก `title` และ `likes` จาก `posts[0]`

```js
const { title, likes } = posts[0];
console.log(title, likes);           // เริ่มต้นเรียน React 12
```

**1.3 Spread** — สร้าง `newPosts` (เพิ่ม id 5) และ `updatedPosts` (แก้ likes ของ id 2 เป็น 9)

```js
const newPosts = [...posts, { id: 5, title: 'Deploy เว็บไซต์', author: 'มานะ', category: 'Backend', likes: 0 }];
const updatedPosts = posts.map(p => (p.id === 2 ? { ...p, likes: 9 } : p));

console.log(posts.length, newPosts.length);          // 4 5
console.log(posts[1].likes, updatedPosts[1].likes);  // 8 9
```

**1.4 Rest Parameter `sum`**

```js
const sum = (...nums) => nums.reduce((total, n) => total + n, 0);
console.log(sum(1, 2, 3, 4));        // 10
```

## ตอนที่ 2: map, filter, reduce (30 นาที)

| ข้อ | โจทย์ | โค้ด | ผลลัพธ์ที่ควรได้ |
|---|---|---|---|
| 2.1 | `map` เก็บเฉพาะชื่อบทความ | `const titles = posts.map(p => p.title);` | อาร์เรย์ 4 ชื่อ |
| 2.2 | `filter` likes ≥ 10 | `const popular = posts.filter(p => p.likes >= 10);` | `popular.length` = 3 |
| 2.3 | `reduce` รวม likes | `const totalLikes = posts.reduce((sum, p) => sum + p.likes, 0);` | 60 |
| 2.4 | chaining ชื่อบทความของ สมชาย | `posts.filter(p => p.author === 'สมชาย').map(p => p.title)` | 2 รายการ |

**2.5 ท้าทาย: นับบทความต่อผู้เขียนด้วย reduce** (ค่าเริ่มต้นเป็น `{}` และใช้ `[author]` เป็น key)

```js
const countByAuthor = posts.reduce((acc, { author }) => {
  acc[author] = (acc[author] || 0) + 1;
  return acc;
}, {});
console.log(countByAuthor);   // { สมชาย: 2, สมหญิง: 1, มานะ: 1 }
```

## ตอนที่ 3: Promises, Async/Await, Fetch API (40 นาที)

ลบหรือคอมเมนต์โค้ดตอนที่ 1–2 ที่ไม่ใช้ แล้วพิมพ์โค้ดใหม่

**3.1 ฟังก์ชัน `wait`** — ทดสอบการหน่วงเวลา

```js
const wait = (ms) => new Promise(resolve => setTimeout(resolve, ms));

const testWait = async () => {
  console.log('เริ่ม');
  await wait(1000);
  console.log('ผ่านไป 1 วินาที');
};
testWait();
```

**3.2 `getPosts`** — ดึง 5 บทความ ตรวจ `res.ok` ใช้ `try/catch`

```js
const getPosts = async (url) => {
  try {
    const res = await fetch(url);
    if (!res.ok) throw new Error('HTTP ' + res.status);
    return await res.json();
  } catch (err) {
    console.error('ดึงข้อมูลไม่สำเร็จ:', err.message);
  }
};

getPosts('https://jsonplaceholder.typicode.com/posts?_limit=5')
  .then(data => data.forEach(({ title }) => console.log(title)));
```

**3.3 ทดสอบ error** — เปลี่ยน URL เป็น `https://jsonplaceholder.typicode.com/postz` แล้วดู Console
ผลที่ควรได้: `ดึงข้อมูลไม่สำเร็จ: HTTP 404`

> หมายเหตุ: เมื่อเกิด error `getPosts` จะคืน `undefined` ทำให้ `.then(data => data.forEach...)` ฟ้องข้อผิดพลาดเพิ่มอีกบรรทัดใน Console เป็นเรื่องปกติของขั้นตอนนี้ ให้ดูบรรทัด "HTTP 404" เป็นหลัก

**3.4 `createPost`** — ส่งข้อมูลด้วย POST

```js
const createPost = async (post) => {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(post),
  });
  if (!res.ok) throw new Error('HTTP ' + res.status);
  return await res.json();
};

createPost({ title: 'บทความของฉัน', body: 'ทดสอบ', userId: 1 })
  .then(result => console.log(result));   // ได้ id: 101
```

## ตอนที่ 4: แสดงรายการบทความบนหน้าเว็บ (15 นาที)

ลบโค้ดเดิมใน `app.js` ทั้งหมด แล้วพิมพ์โค้ดนี้ จากนั้นรีเฟรช

```js
const statusEl = document.getElementById('status');
const listEl = document.getElementById('post-list');

const loadPosts = async () => {
  try {
    const url = 'https://jsonplaceholder.typicode.com/posts?_limit=20';
    const res = await fetch(url);
    if (!res.ok) throw new Error('HTTP ' + res.status);
    const posts = await res.json();

    const myPosts = posts.filter(p => p.userId === 1);

    listEl.innerHTML = myPosts
      .map(({ id, title }) => `<li>${id}. ${title}</li>`)
      .join('');

    const totalChars = myPosts.reduce((sum, p) => sum + p.title.length, 0);
    statusEl.textContent =
      `พบ ${myPosts.length} บทความ (ชื่อเรื่องรวม ${totalChars} ตัวอักษร)`;
  } catch (err) {
    statusEl.textContent = 'เกิดข้อผิดพลาด: ' + err.message;
  }
};
loadPosts();
```

- **4.1 ตรวจผล:** ต้องเห็นบทความหมายเลข 1–10 และข้อความ "พบ 10 บทความ ..."
- **4.2 ปรับปรุง:** เปลี่ยนเงื่อนไข filter เป็น `p.userId === 2` → ควรได้บทความ 11–20 แล้วบันทึกผล
- **4.3 ทดสอบ error:** เปลี่ยน URL ให้ผิด → หน้าเว็บต้องแสดง "เกิดข้อผิดพลาด: HTTP 404" แล้วแก้ URL กลับ
- **4.4 ส่งงาน:** ภาพหน้าจอหน้าเว็บและ Console + ไฟล์ `index.html` และ `app.js` ส่งครูตามที่กำหนด

## หลังทำเสร็จ: บันทึกผลและตอบคำถาม (ข้อ 8)

1. กรอก **ตารางบันทึกผล** (ผลที่ได้จริง / ผ่าน-ไม่ผ่าน) ของข้อ 1.1–1.4, 2.1–2.4, 3.2–3.4, 4.1
2. ตอบคำถามสรุป 5 ข้อ
   1. Arrow Function ต่างจากฟังก์ชันแบบเดิมอย่างไร และเมื่อใดละ `return` ได้
   2. `map`, `filter`, `reduce` ต่างกันอย่างไร (เทียบค่าที่คืนกลับ)
   3. เหตุใดต้องตรวจ `res.ok` หลัง `await fetch()` และผลทดสอบ URL ผิดในข้อ 3.3 คืออะไร
   4. ปัญหาที่พบระหว่างทำงานและวิธีแก้
   5. นำความรู้ไปประยุกต์ใช้กับ Blog App ในหน่วยถัดไปได้อย่างไร

**เกณฑ์คะแนน (เต็ม 5):** ตอน 1 (1) · ตอน 2 (1) · ตอน 3 (1) · ตอน 4 (1) · ความรับผิดชอบ/ละเอียดรอบคอบ/บันทึกผลครบ/ส่งตรงเวลา (1)

---

# ส่วนที่ 2: ใบกิจกรรมที่ 4 — "นักสืบโค้ด ES6+" (Code Detective)

ทำเป็นทีมละ 3–4 คน รวมประมาณ 60 นาที

## ขั้นเตรียมทีม (5 นาที)

1. แบ่งทีม กรอกชื่อสมาชิก และเลือกบทบาท **สลับบทบาทกันทุกภารกิจ**

| บทบาท | หน้าที่ |
|---|---|
| ผู้ขับ (Driver) | พิมพ์โค้ดบนคอมพิวเตอร์ตามที่ทีมตกลง |
| ผู้นำทาง (Navigator) | อ่านโจทย์ ตรวจโค้ด เสนอแนวทางแก้ไข |
| ผู้บันทึกผล (Recorder) | บันทึกคำทำนาย ผลจริง และโค้ดที่แก้ไขลงในใบกิจกรรม |
| ผู้นำเสนอ (Presenter) | คุมเวลา และสรุปผลของทีมให้ครูและเพื่อนฟัง |

2. สร้างโฟลเดอร์ `activity4` มี `index.html` และ `app.js` (โครงเดียวกับใบงานที่ 4) หรือใช้ JSFiddle / CodePen
3. เตรียมข้อมูล `posts` ชุดเดียวกับในใบงาน (ใช้ในภารกิจที่ 3)

## ภารกิจที่ 1: ทายก่อนรัน (10 นาที)

**วิธีทำ:** ทีมช่วยกันทำนายผลของโค้ดแต่ละข้อ **ก่อนรัน** → บันทึกคำทำนายลงตาราง → รันใน Console → เทียบผล

| ข้อ | โค้ด |
|---|---|
| 1 | `const f = (a, b = 2) => a * b; console.log(f(5));` |
| 2 | `const { x, y = 5 } = { x: 1 }; console.log(x, y);` |
| 3 | `console.log([...[1, 2], ...[3]]);` |
| 4 | `console.log([1, 2, 3, 4].filter(n => n > 2).map(n => n * 10));` |
| 5 | `console.log([3, 4].reduce((a, n) => a + n));` |
| 6 (ท้าทาย) | `console.log('A'); setTimeout(() => console.log('B'), 0); Promise.resolve().then(() => console.log('C')); console.log('D');` |

ตารางบันทึก: คำทำนายของทีม | ผลที่รันได้จริง | ตรงกันหรือไม่
ข้อ 6 ให้อธิบายเหตุผลของลำดับผลลัพธ์ด้วย (ใบ้: Promise กับ `setTimeout` ถูกจัดคิวต่างกัน)

## ภารกิจที่ 2: ซ่อมโค้ด (12 นาที)

**วิธีทำ:** รันโค้ดเพื่อ **สังเกตอาการ** → **หาสาเหตุ** → เขียน **โค้ดที่แก้แล้ว** ลงตาราง

| ข้อ | โค้ดที่มีปัญหา | ต้องการ |
|---|---|---|
| 1 | `const makePost = title => { title, likes: 0 };` | คืนอ็อบเจกต์บทความ |
| 2 | `const titles = posts.map(p => { p.title });` | อาร์เรย์ชื่อบทความ |
| 3 | `const total = posts.reduce((sum, p) => sum + p.likes);` | ผลรวม likes เป็นตัวเลข |
| 4 | `const res = fetch('...'); const data = await res.json();` (ในฟังก์ชัน async) | ข้อมูลบทความจาก API |
| 5 | `const load = () => { const res = await fetch('...'); return res.json(); };` | ดึงข้อมูลเมื่อเรียกฟังก์ชัน |

**แนวคิดในการหาสาเหตุ** (ให้ทีมคิดเองก่อน แล้วค่อยตรวจกับครู)
- ข้อ 1–2: ดูว่า `{ }` หลังลูกศรถูกตีความเป็น "บล็อกฟังก์ชัน" หรือ "อ็อบเจกต์" และมี `return` หรือไม่
- ข้อ 3: ดูว่า `reduce` มีค่าเริ่มต้นหรือไม่
- ข้อ 4–5: ดูว่า `await` ครบหรือไม่ และฟังก์ชันประกาศ `async` แล้วหรือยัง

## ภารกิจที่ 3: รายงานบทความยอดนิยม (13 นาที)

เขียนโค้ดจากข้อมูล `posts` ตามลำดับ

1. `filter` — คัดบทความที่ likes ≥ 10
2. `map` — แปลงเป็นข้อความ "ชื่อบทความ (จำนวน likes)" โดยใช้ Destructuring + Template Literal
3. `reduce` — หาผลรวม likes ของบทความที่คัด
4. `console.log` แสดงทั้งรายการข้อความและผลรวม

```js
const popular = posts.filter(p => p.likes >= 10);
const report = popular.map(({ title, likes }) => `${title} (${likes} likes)`);
const totalLikes = popular.reduce((sum, p) => sum + p.likes, 0);

console.log(report);      // 3 รายการ
console.log(totalLikes);  // 52
```

**เกณฑ์ตรวจ:** ได้ 3 บทความ และผลรวม likes = 52

## ภารกิจที่ 4: รายชื่อผู้ใช้จาก API (15 นาที)

เขียนฟังก์ชัน async ดึงข้อมูลจาก `https://jsonplaceholder.typicode.com/users`

1. ใช้ `fetch` + `async/await` + ตรวจ `res.ok` + `try/catch`
2. ใช้ `map` + Destructuring แปลงเป็นข้อความ `ชื่อ <อีเมล>`
3. แสดงจำนวนผู้ใช้ทั้งหมด (ควรได้ 10 คน)
4. ท้าทาย: ใช้ `reduce` หาผู้ใช้ที่มีชื่อยาวที่สุด

```js
const showUsers = async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/users');
    if (!res.ok) throw new Error('HTTP ' + res.status);
    const users = await res.json();

    users.map(({ name, email }) => `${name} <${email}>`)
         .forEach(line => console.log(line));
    console.log('จำนวนผู้ใช้:', users.length);

    const longest = users.reduce(
      (best, u) => (u.name.length > best.length ? u.name : best), '');
    console.log('ชื่อยาวที่สุด:', longest);
  } catch (err) {
    console.error('ผิดพลาด:', err.message);
  }
};
showUsers();
```

บันทึก: จำนวนผู้ใช้ที่ได้ ........ คน / ผู้ใช้ที่มีชื่อยาวที่สุด ........

## ขั้นสรุปและนำเสนอ (5 นาที)

1. ผู้นำเสนอสรุปว่าภารกิจใดยากที่สุดและแก้อย่างไร (ไม่เกิน 1 นาที/ทีม)
2. ครูสรุปข้อผิดพลาดที่พบบ่อย แล้วทีมแลกใบกิจกรรมกันตรวจตามเฉลย

## อภิปรายและบันทึก (ข้อ 7)

1. ข้อผิดพลาดที่พบบ่อยในภารกิจที่ 2 และวิธีสังเกต/ป้องกัน
2. การทำนายผลก่อนรันช่วยให้เข้าใจโค้ดอย่างไร
3. การสลับบทบาท Driver/Navigator ช่วยทีมอย่างไร
4. นำไปใช้กับ React.js และ Blog App ในหน่วยถัดไปอย่างไร

**เกณฑ์คะแนน (เต็ม 10 รายทีม):** ภารกิจ 1 (2) · ภารกิจ 2 (2) · ภารกิจ 3 (2) · ภารกิจ 4 (2) · การทำงานเป็นทีม/สลับบทบาท/นำเสนอ/ตรงเวลา (2)

---

# ส่วนที่ 3: ใบมอบหมายงานที่ 4 — Mini Project "Blog Reader"

**งานรายบุคคล** ใช้ HTML + JavaScript ES6+ ล้วน (ห้ามใช้ React, jQuery, axios) ใช้ Tailwind CSS ตกแต่งได้ ข้อมูลทั้งหมดจาก `https://jsonplaceholder.typicode.com` (`/posts` 100 รายการ และ `/users` 10 คน)

## ข้อกำหนดของงาน 8 ข้อ

| ที่ | ข้อกำหนด | เทคนิคที่ต้องใช้ |
|---|---|---|
| 1 | ดึง `/posts` และ `/users` พร้อมกัน | async/await, fetch, `Promise.all`, `res.ok` |
| 2 | แสดง "กำลังโหลด..." ระหว่างรอ และแสดง error บนหน้าเว็บ | try/catch, `throw new Error` |
| 3 | รวมบทความกับชื่อผู้เขียน (`userId` ↔ `id`) | map, find, Spread `{ ...post, authorName }` |
| 4 | แสดงเป็นการ์ด: ชื่อเรื่อง ชื่อผู้เขียน เนื้อหาย่อ 100 ตัวอักษรแรก | Arrow Function, Destructuring, Template Literal, slice |
| 5 | ช่องค้นหาชื่อเรื่อง (ไม่สนตัวพิมพ์เล็ก/ใหญ่) | filter, includes, toLowerCase |
| 6 | เมนูเลือกผู้เขียน ใช้ร่วมกับช่องค้นหาได้ | filter, Spread สร้างตัวเลือก |
| 7 | สรุป "แสดง N จาก 100 บทความ" + จำนวนบทความต่อผู้เขียน | reduce, Object.entries |
| 8 | โค้ดอ่านง่าย ใช้ `const/let` (ไม่ใช้ `var`) ชื่อสื่อความหมาย มีคอมเมนต์ | - |

## ขั้นตอนการทำ (ทำทีละข้อ)

### ขั้นที่ 1: ทบทวนและเตรียมโครงงาน
- ทบทวนใบความรู้ ใบงาน และใบกิจกรรมที่ 4
- สร้างโฟลเดอร์ `assignment4_รหัสนักเรียน`

### ขั้นที่ 2: สร้างโครง `index.html`
ต้องมี `input` (ค้นหา), `select` (ผู้เขียน), พื้นที่สรุป, พื้นที่แสดงการ์ด และพื้นที่แสดงสถานะ โดยกำหนด `id` ให้ทุกส่วน

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Blog Reader</title>
  <!-- ใส่ Tailwind CSS ได้ตามที่เรียนในหน่วยที่ 2–3 -->
</head>
<body>
  <h1>Blog Reader — รายการบทความ</h1>
  <input id="search-input" type="text" placeholder="ค้นหาชื่อบทความ">
  <select id="author-select"></select>
  <p id="status"></p>
  <p id="summary"></p>
  <div id="card-list"></div>
  <script src="app.js"></script>
</body>
</html>
```

### ขั้นที่ 3: เขียนฟังก์ชันดึงข้อมูล และตรวจผลใน Console ให้ได้ก่อน

ใช้โครงโค้ดเริ่มต้นจากใบมอบหมายงาน แล้วเติมส่วน TODO

```js
const API = 'https://jsonplaceholder.typicode.com';
const searchInput = document.getElementById('search-input');
const authorSelect = document.getElementById('author-select');
const statusEl = document.getElementById('status');
const summaryEl = document.getElementById('summary');
const cardListEl = document.getElementById('card-list');

let allPosts = []; // บทความทั้งหมดที่รวมชื่อผู้เขียนแล้ว

const loadData = async () => {
  try {
    statusEl.textContent = 'กำลังโหลด...';           // ข้อกำหนดข้อ 2
    const [postsRes, usersRes] = await Promise.all([
      fetch(`${API}/posts`),
      fetch(`${API}/users`),
    ]);
    if (!postsRes.ok || !usersRes.ok) throw new Error('โหลดข้อมูลไม่สำเร็จ');
    const [posts, users] = await Promise.all([postsRes.json(), usersRes.json()]);

    // TODO 1: รวม posts กับชื่อผู้เขียน (map + find + Spread)
    allPosts = posts.map(post => ({
      ...post,
      authorName: users.find(u => u.id === post.userId)?.name ?? 'ไม่ทราบ',
    }));

    // TODO 2: สร้างตัวเลือกใน <select> จาก users
    const options = [{ id: 0, name: 'ทุกผู้เขียน' }, ...users];
    authorSelect.innerHTML = options
      .map(({ id, name }) => `<option value="${id}">${name}</option>`)
      .join('');

    statusEl.textContent = '';
    // TODO 3: แสดงผลครั้งแรก
    applyFilters();
  } catch (err) {
    // TODO 4: แสดงข้อความผิดพลาดบนหน้าเว็บ
    statusEl.textContent = 'เกิดข้อผิดพลาด: ' + err.message;
  }
};
```

### ขั้นที่ 4: เขียน `applyFilters` (ค้นหา + กรองผู้เขียน ใช้ร่วมกัน)

`applyFilters` ต้องอ่านค่าจาก **ทั้งสองส่วนทุกครั้ง**

```js
const applyFilters = () => {
  // TODO 5: อ่านคำค้นและผู้เขียนที่เลือก แล้ว filter allPosts
  const keyword = searchInput.value.trim().toLowerCase();
  const authorId = Number(authorSelect.value);

  const filtered = allPosts.filter(p =>
    p.title.toLowerCase().includes(keyword) &&
    (authorId === 0 || p.userId === authorId)
  );

  // TODO 6: แสดงผล
  render(filtered);
  showSummary(filtered);
};
```

### ขั้นที่ 5: เขียน `render` (การ์ดบทความ) และ `showSummary` (สรุป)

```js
// แสดงการ์ด: ชื่อเรื่อง ผู้เขียน เนื้อหาย่อ 100 ตัวอักษรแรก
const render = (posts) => {
  cardListEl.innerHTML = posts
    .map(({ title, authorName, body }) => `
      <div class="card">
        <h2>${title}</h2>
        <p>ผู้เขียน: ${authorName}</p>
        <p>${body.slice(0, 100)}...</p>
      </div>`)
    .join('');
};

// สรุป: แสดง N จาก 100 + จำนวนบทความต่อผู้เขียนที่กำลังแสดง
const showSummary = (posts) => {
  const countByAuthor = posts.reduce((acc, { authorName }) => {
    acc[authorName] = (acc[authorName] || 0) + 1;
    return acc;
  }, {});
  const perAuthor = Object.entries(countByAuthor)
    .map(([name, count]) => `${name}: ${count}`)
    .join(' | ');
  summaryEl.textContent =
    `แสดง ${posts.length} จาก ${allPosts.length} บทความ | ${perAuthor}`;
};

// ผูกเหตุการณ์
searchInput.addEventListener('input', applyFilters);
authorSelect.addEventListener('change', applyFilters);

loadData();
```

> `const` ฟังก์ชัน arrow ต้องประกาศก่อนถูกเรียกใช้จริง ในโค้ดข้างต้น `loadData()` ถูกเรียกท้ายไฟล์ จึงเรียก `applyFilters`, `render`, `showSummary` ได้ถูกต้อง

### ขั้นที่ 6: ตกแต่งหน้าเว็บ
ปรับการ์ดและเลย์เอาต์ด้วย Tailwind CSS ได้ตามต้องการ (Wireframe เป็นเพียงแนวทาง ปรับสีสันได้)

### ขั้นที่ 7: ทดสอบ
- ลองค้นหาคำ (พิมพ์เล็ก/ใหญ่) และเลือกผู้เขียนพร้อมกัน
- ทดสอบ error: เปลี่ยน URL ให้ผิดชั่วคราว ตรวจว่าหน้าเว็บแสดง "เกิดข้อผิดพลาด: ..." แล้วแก้กลับ
- ตรวจโค้ดตามข้อกำหนด 8 ข้อ (ไม่มี `var`, ชื่อตัวแปรสื่อความหมาย, มีคอมเมนต์)

### ขั้นที่ 8: จัดทำและส่งงาน

ส่งภายในคาบเรียนสัปดาห์ที่ 5 (ก่อนเริ่มหน่วยที่ 5) ตามช่องทางที่ครูกำหนด

1. โฟลเดอร์ `assignment4_รหัสนักเรียน` บีบอัดเป็น `.zip` ภายในมี `index.html` และ `app.js` (และไฟล์อื่นถ้ามี)
2. ภาพหน้าจอ 2 ภาพ: (ก) หน้าแสดงบทความทั้งหมด (ข) หลังใช้ช่องค้นหาร่วมกับเมนูเลือกผู้เขียน
3. `README.txt` 3–5 บรรทัด ระบุว่าใช้ Arrow Function, Destructuring, Spread, map, filter, reduce และ async/await ที่ส่วนใดของโค้ด

**ข้อควรระวัง:** ปรึกษาเพื่อนได้ แต่ต้องเขียนโค้ดเอง งานที่คัดลอกกันจะไม่ได้คะแนนในส่วนที่เหมือนกัน

**เกณฑ์คะแนน (เต็ม 10):**

| รายการ | คะแนน |
|---|---|
| ดึงข้อมูลสำเร็จ (Promise.all, async/await, res.ok, try/catch) | 2 |
| รวมข้อมูลผู้เขียนและแสดงการ์ดถูกต้อง (Spread, Destructuring) | 2 |
| ค้นหาและกรองตามผู้เขียนทำงานถูกต้องและใช้ร่วมกันได้ (filter) | 2 |
| สรุปจำนวนที่แสดงและจำนวนต่อผู้เขียนถูกต้อง (reduce) | 1 |
| คุณภาพโค้ด อ่านง่าย ตั้งชื่อสื่อความหมาย มีคอมเมนต์ ใช้ ES6+ | 2 |
| ส่งตรงเวลา ครบ 3 รายการ และเป็นผลงานของตนเอง | 1 |

---

# ภาคผนวก: สรุปแนวคิดหลักที่ใช้ตลอดหน่วย

| หัวข้อ | สรุปสั้น ๆ |
|---|---|
| Arrow Function | `(a, b) => a + b` ละ `return` ได้เมื่อมีนิพจน์เดียว; คืนอ็อบเจกต์ต้องครอบ `({ ... })` |
| Destructuring | `const { title, likes } = post;` / `const [first, ...others] = posts;` |
| Spread / Rest | `...` ฝั่งขวาหรือตอนเรียกฟังก์ชัน = กระจาย (Spread); ในพารามิเตอร์หรือฝั่งซ้าย = รวม (Rest) |
| `map` | แปลงทุกตัว → อาร์เรย์ใหม่ขนาดเท่าเดิม |
| `filter` | คัดตามเงื่อนไข → อาร์เรย์ใหม่ขนาดเท่าหรือน้อยกว่า |
| `reduce` | รวมเป็นค่าเดียว (ควรใส่ค่าเริ่มต้นเสมอ) |
| Promise | 3 สถานะ: pending / fulfilled / rejected |
| async/await | `await` ใช้ได้ในฟังก์ชันที่ประกาศ `async` เท่านั้น; ใช้ `try/catch` ดักข้อผิดพลาด |
| Fetch API | `fetch` ไม่ reject เมื่อได้ 404/500 → ต้องตรวจ `res.ok` แล้วจึง `res.json()` |
| Promise.all | รอหลายงานพร้อมกัน ได้ผลเป็นอาร์เรย์ตามลำดับเดิม |
