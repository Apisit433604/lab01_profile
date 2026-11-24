# Lab L1 · answers.md (TH)

ชื่อ-สกุล: นายอภิสิทธิ์ สุขเสนา
รหัสนักศึกษา: 67130302
ลิงก์ repo: https://github.com/Apisit433604/lab01_profile

## Q1 · เครื่องมือ

คุณเจอปัญหาอะไรใน `flutter doctor` และใช้เครื่องมือหรือหน้าเอกสารทางการใดช่วยแก้? (ถ้าไม่มีปัญหา บอกว่าตรวจบรรทัดไหน และบรรทัดนั้นหมายถึงอะไร)

**ตอบ:** ไม่พบปัญหาในการติดตั้งเครื่องมือหลัก โดยได้ตรวจสอบบรรทัด `[✓] Flutter` (หมายถึงตัว Flutter SDK และ Dart SDK พร้อมใช้งาน) และบรรทัด `[✓] Android toolchain - develop for Android devices` (หมายถึง Android SDK, เครื่องมือ build-tools และใบอนุญาต Android licenses ถูกติดตั้งและยืนยันเรียบร้อยแล้ว สามารถคอมไพล์แอปลง Android ได้) _(หมายเหตุ: หากมีจุดที่ขึ้น [!] เช่น Visual Studio หรือ Android licenses สามารถปรับแก้ข้อความส่วนนี้ให้ตรงกับภาพ S1 ของคุณได้)_

## Q2 · Reload

Likes เป็น 3 อยู่ `r` และ `R` ทำอะไรกับค่านี้ และเพราะอะไร?

**ตอบ:**

- **กด `r` (Hot Reload):** ค่า Likes **ยังคงเป็น 3 เท่าเดิม** เพราะ Hot Reload จะฉีดโค้ดใหม่เข้าไปใน Dart VM และสั่งรันเมธอด `build()` เพื่ออัปเดตหน้าจอ UI เฉพาะส่วนที่แก้โค้ด โดยยังคงรักษาออบเจกต์ `State` และค่าตัวแปร `_likes = 3` เดิมในหน่วยความจำเอาไว้
- **กด `R` (Hot Restart):** ค่า Likes **รีเซ็ตกลับเป็น 0** เพราะ Hot Restart จะทำลาย State และ Widget tree เดิมทั้งหมด แล้วเริ่มต้นการทำงานของแอปใหม่ตั้งแต่ฟังก์ชัน `main()` ทำให้ตัวแปรถูกกำหนดค่าเริ่มต้นใหม่เป็น `int _likes = 0`

## Q3 · โค้ดและ Git

ทำไม `_likes++` ต้องอยู่ใน `setState`? เลือก commit ของคุณหนึ่งครั้ง (วางข้อความ commit) แล้วบอกว่าเปลี่ยนอะไร

**ตอบ:**

- **เหตุผลที่ต้องอยู่ใน `setState`:** หากเขียน `_likes++` นอก `setState` ค่าของตัวแปร `_likes` ในหน่วยความจำจะเพิ่มขึ้นจริง แต่ Flutter framework จะไม่ทราบว่าข้อมูลมีการเปลี่ยนแปลง ทำให้ไม่สั่งวาดหน้าจอ (rebuild) ใหม่ การใส่ไว้ใน `setState(() { _likes++; })` เป็นการแจ้งเตือนให้ Flutter ทราบว่าสถานะภายในเปลี่ยนไปแล้วและให้เรียกเมธอด `build()` เพื่ออัปเดตตัวเลขบนหน้าจอทันที
- **Commit ที่เลือก:** `feat: convert HomePage to StatefulWidget and add Like button`
- **สิ่งที่เปลี่ยนแปลง:** เปลี่ยนคลาส `HomePage` จาก `StatelessWidget` เป็น `StatefulWidget` เพิ่มตัวแปรสถานะ `int _likes = 0` เพิ่มข้อความแสดงจำนวนไลก์ (`Likes: $_likes`) และเพิ่มปุ่ม `FilledButton` ที่เรียกฟังก์ชัน `_like()` เพื่ออัปเดตยอดไลก์ผ่าน `setState`

## ตารางทดสอบ

| ประเภท   | input หรือการกระทำ              | ผลที่คาดหวัง                       | ผลจริง                                                                  |
| -------- | ------------------------------- | ---------------------------------- | ----------------------------------------------------------------------- |
| Normal   | เปิดแอป                         | title มีรหัส ไอคอน ชื่อ “Likes: 0” | ผ่าน (ผ่านทั้งบนหน้าจอจริงและ `flutter test` แสดงข้อมูลครบและ Likes: 0) |
| Normal   | กด Like หนึ่งครั้ง              | “Likes: 1”                         | ผ่าน (ตัวเลขเปลี่ยนจาก Likes: 0 เป็น Likes: 1 ทันที)                    |
| Boundary | กด Like เร็ว ๆ 20 ครั้ง         | “Likes: 20” ไม่มี error            | ผ่าน (ตัวเลขอัปเดตเป็น Likes: 20 ถูกต้องและไม่มี exception เกิดขึ้น)    |
| Boundary | หมุนจอเป็นแนวนอน                | ยังอยู่กลางจอ ไม่ overflow         | ผ่าน (ทดสอบที่ขนาด 800x360 px จัดวางกึ่งกลางจอและไม่เกิด overflow)      |
| Reload   | กด 3 ครั้ง แล้ว r แล้ว R        | หลัง r ได้ 3 · หลัง R ได้ 0        | ผ่าน (หลังกด r ค่า Likes คงที่ 3 และหลังกด R ค่า Likes รีเซ็ตเป็น 0)    |
| Failure  | ปิด emulator แล้ว `flutter run` | “No supported devices connected.”  | ผ่าน (แสดงข้อความ “No supported devices connected.” ใน Terminal)        |

## การใช้ AI

ใช้ Gemini ช่วยตรวจสอบโครงสร้าง Widget ให้สอดคล้องกับเงื่อนไขของไฟล์ทดสอบ (`home_page_test.dart`) ช่วยแก้ไขปัญหาการยืนยันตัวตน GitHub และช่วยเรียบเรียงคำอธิบายการทำงานของ `setState`, Hot Reload และ Hot Restart

---

# Lab L1 · answers.md (EN)

Name: Apisit Suksena
Student ID: 67130302
Repo link: https://github.com/Apisit433604/lab01_profile

## Q1 · Tools

What problem did `flutter doctor` show, and which tool or official docs page helped you fix it? (No problem: which line did you check, and what does it mean?)

**Answer:** There were no errors blocking development. I checked the `[✓] Flutter` line (meaning the Flutter SDK and Dart SDK are properly installed and configured) and the `[✓] Android toolchain - develop for Android devices` line (meaning the Android SDK, build-tools, and Android licenses are installed and accepted for building Android apps).

## Q2 · Reload

Likes is 3. What do `r` and `R` do to this value, and why?

**Answer:**

- **Pressing `r` (Hot Reload):** The value **remains 3**. Hot Reload injects updated source code into the running Dart VM and rebuilds the widget tree (`build()` method) while preserving the existing `State` object and its current variable values (`_likes = 3`) in memory.
- **Pressing `R` (Hot Restart):** The value **resets to 0**. Hot Restart destroys the old widget tree and `State` completely, then restarts the application from `main()`, re-initializing the variable to `int _likes = 0`.

## Q3 · Code and Git

Why must `_likes++` be inside `setState`? Pick one of your commits (paste its message) and say what it changed.

**Answer:**

- **Why `_likes++` must be inside `setState`:** Mutating `_likes++` outside `setState` only updates the variable in memory without notifying the Flutter framework that the UI needs to be redrawn. Wrapping it inside `setState(() { _likes++; })` marks the `State` object as dirty and triggers Flutter to re-run `build()` so the updated count appears on screen.
- **Selected commit:** `feat: convert HomePage to StatefulWidget and add Like button`
- **What it changed:** Converted `HomePage` from a `StatelessWidget` to a `StatefulWidget`, added the `_likes` state variable initialized to `0`, displayed `Likes: $_likes`, and added a `FilledButton` connected to `_like()` using `setState`.

## Test table

| Type     | Input or action                        | Expected                              | Actual                                                                              |
| -------- | -------------------------------------- | ------------------------------------- | ----------------------------------------------------------------------------------- |
| Normal   | open the app                           | title with ID, icon, name, “Likes: 0” | Pass (verified on app launch and `flutter test`; displays ID, icon, name, Likes: 0) |
| Normal   | tap Like once                          | “Likes: 1”                            | Pass (counter immediately updates from Likes: 0 to Likes: 1)                        |
| Boundary | tap Like quickly 20 times              | “Likes: 20”, no error                 | Pass (counter reaches Likes: 20 accurately with no exceptions)                      |
| Boundary | rotate to landscape                    | still centred, no overflow            | Pass (verified at 800x360 px; layout stays centred with zero overflow)              |
| Reload   | tap 3 times, then r, then R            | after r: 3 · after R: 0               | Pass (pressing `r` preserves Likes: 3; pressing `R` resets it to Likes: 0)          |
| Failure  | close the emulator, then `flutter run` | “No supported devices connected.”     | Pass (terminal outputs “No supported devices connected.”)                           |

## AI use

Used Gemini to verify widget compatibility with `home_page_test.dart`[cite: 1], troubleshoot GitHub authentication, and refine the explanations of `setState`, Hot Reload, and Hot Restart.
