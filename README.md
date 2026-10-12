# คู่มือสรุปคำสั่ง Git ฉบับใช้งานจริง (Git Command Reference & Guide)

คู่มือรวบรวมคำสั่ง Git พื้นฐานจนถึงระดับการทำงานร่วมกันเป็นทีม (Collaboration & Review Workflow) อธิบายด้วยภาษาไทยเข้าใจง่าย พร้อมตัวอย่างคำสั่ง คำอธิบายการทำงาน และข้อควรระวังเพื่อความปลอดภัยของข้อมูล

---

## สารบัญ (Table of Contents)

1. [ภาพรวม (Overview)](#1-ภาพรวม-overview)
2. [ข้อกำหนดเบื้องต้น (Prerequisites)](#2-ข้อกำหนดเบื้องต้น-prerequisites)
3. [เวิร์กโฟลว์พื้นฐานจาก Local สู่ Remote (Common Local-to-Remote Workflow)](#3-เวิร์กโฟลว์พื้นฐานจาก-local-สู่-remote-common-local-to-remote-workflow)
4. [คู่มือคำสั่ง Git แยกตามหมวดหมู่ (Command Reference)](#4-คู่มือคำสั่ง-git-แยกตามหมวดหมู่-command-reference)
   - [4.1 การตั้งค่าตัวตนและการจัดการคอนฟิก (Configuration & Identity)](#41-การตั้งค่าตัวตนและการจัดการคอนฟิก-configuration--identity)
   - [4.2 การตั้งค่า SSH Key สำหรับ Remote (SSH Key Configuration)](#42-การตั้งค่า-ssh-key-สำหรับ-remote-ssh-key-configuration)
   - [4.3 การเริ่มต้นโปรเจกต์และการ Clone (Repository Initialization & Clone)](#43-การเริ่มต้นโปรเจกต์และการ-clone-repository-initialization--clone)
   - [4.4 การตรวจสอบสถานะและความเปลี่ยนแปลง (Inspection & Diff)](#44-การตรวจสอบสถานะและความเปลี่ยนแปลง-inspection--diff)
   - [4.5 การจัดเตรียมและบันทึกความเปลี่ยนแปลง (Staging & Committing)](#45-การจัดเตรียมและบันทึกความเปลี่ยนแปลง-staging--committing)
   - [4.6 การจัดการ Remote Repository](#46-การจัดการ-remote-repository)
   - [4.7 การส่งและดึงข้อมูล (Push & Pull)](#47-การส่งและดึงข้อมูล-push--pull)
   - [4.8 การยกเลิกและย้อนสถานะ (Reset, Restore & Revert)](#48-การยกเลิกและย้อนสถานะ-reset-restore--revert)
   - [4.9 การจัดการ Branch และการเปลี่ยนชื่อ Default Branch](#49-การจัดการ-branch-และการเปลี่ยนชื่อ-default-branch)
5. [กระบวนการทำงานร่วมกันผ่าน Branch และ Pull Request (Collaboration & PR Workflow)](#5-กระบวนการทำงานร่วมกันผ่าน-branch-และ-pull-request-collaboration--pr-workflow)
6. [ตารางเปรียบเทียบคำสั่งที่มีความเสี่ยงและทางเลือกที่ปลอดภัย (Safety Cheat Sheet)](#6-ตารางเปรียบเทียบคำสั่งที่มีความเสี่ยงและทางเลือกที่ปลอดภัย-safety-cheat-sheet)

---

## 1. ภาพรวม (Overview)

Git เป็นระบบควบคุมเวอร์ชันแบบกระจายศูนย์ (Distributed Version Control System) ที่ใช้ติดตามการเปลี่ยนแปลงของโค้ดและช่วยให้ทำงานร่วมกันในทีมได้อย่างมีประสิทธิภาพ คู่มือนี้จัดทำขึ้นเพื่อเป็นเอกสารอ้างอิงฉบับสมบูรณ์ ครอบคลุมคำสั่งพื้นฐาน คำแนะนำด้านความปลอดภัย ข้อควรระวังเมื่อใช้คำสั่งที่มีผลกระทบสูง และแนวทางปฏิบัติที่ดี (Best Practices) สำหรับการเปิด Pull Request

---

## 2. ข้อกำหนดเบื้องต้น (Prerequisites)

ก่อนเริ่มใช้งาน ควรตรวจสอบความพร้อมของสภาพแวดล้อมดังนี้:

1. **ติดตั้ง Git**: ตรวจสอบว่ามีการติดตั้ง Git ในเครื่องแล้วด้วยคำสั่ง:
   ```bash
   git --version
   ```
2. **มีบัญชี Git Hosting**: เช่น GitHub, GitLab หรือ Bitbucket เพื่อใช้งาน Remote Repository
3. **ความคุ้นเคยกับ Terminal / Command Line**: สามารถเปิดและรันคำสั่งพื้นฐานได้

---

## 3. เวิร์กโฟลว์พื้นฐานจาก Local สู่ Remote (Common Local-to-Remote Workflow)

ลำดับขั้นตอนการทำงานทั่วไปตั้งแต่เริ่มสร้างงานในเครื่องจนนำขึ้น Remote Repository:

```
[แก้ไขโค้ดในเครื่อง]
       │
       ▼
  git status / git diff          (ตรวจสอบความเปลี่ยนแปลง)
       │
       ▼
  git add <file> / git add .     (นำไฟล์เข้า Staging Area)
       │
       ▼
  git commit -m "ข้อความ"        (บันทึกประวัติการเปลี่ยนแปลงใน Local)
       │
       ▼
  git remote add origin <URL>    (เชื่อมต่อ Remote Repository ครั้งแรก)
       │
       ▼
  git push -u origin <branch>    (ส่ง commit ขึ้น Remote พร้อมตั้ง upstream)
       │
       ▼
  git pull origin <branch>       (ดึงการเปลี่ยนแปลงล่าสุดจาก Remote ก่อนเริ่มงานใหม่)
```

---

## 4. คู่มือคำสั่ง Git แยกตามหมวดหมู่ (Command Reference)

### 4.1 การตั้งค่าตัวตนและการจัดการคอนฟิก (Configuration & Identity)

ใช้สำหรับระบุชื่อและอีเมลของผู้พัฒนาที่จะปรากฏในบันทึก commit ทุกครั้ง

```bash
# กำหนดอีเมลผู้ใช้งานสำหรับทุก repository ในเครื่อง
git config --global user.email "your_email@example.com"

# กำหนดชื่อผู้ใช้งานสำหรับทุก repository ในเครื่อง
git config --global user.name "Your Name"

# เปิดไฟล์คอนฟิกเพื่อตรวจสอบหรือแก้ไขผ่านตัวแก้ไขข้อความ (Editor)
git config --global --edit
```

#### การยกเลิกคอนฟิก (Unsetting Config Values)
```bash
# ยกเลิกชื่อผู้ใช้ในระดับ global
git config --global --unset user.name

# ยกเลิกอีเมลในระดับ global
git config --global --unset user.email

# ยกเลิกการตั้งค่า credential helper
git config --global --unset credential.helper

# ยกเลิกการตั้งค่าหลายค่าของ key ที่ระบุ (จำเป็นต้องระบุชื่อ key เสมอ)
git config --global --unset-all <key>
```

> **ข้อสังเกตและข้อเท็จจริงสำคัญเรื่องการ "Log out":**
> - การสั่ง `git config --global --unset user.name` หรือ `user.email` **ไม่ใช่การ Log out** ออกจาก Git Hosting (เช่น GitHub) แต่เป็นเพียงการลบข้อมูลตัวตนผู้เขียน commit (author identity) เท่านั้น
> - การล็อกเอาต์หรือล้างสิทธิ์เข้าถึงจริง ให้ดำเนินการผ่านระบบจัดเก็บรหัสผ่าน (Credential Helper) เช่น:
>   - **Windows**: ไปที่ *Windows Credential Manager* -> ลบรายการที่เกี่ยวกับ GitHub / Git
>   - **macOS**: ไปที่ *Keychain Access* -> ค้นหาและลบ `github.com`
>   - **Linux**: จัดการผ่าน Secret Service / Keyring หรือลบ Personal Access Token ออกจากคอนฟิก
>   - **SSH**: ลบหรือเปลี่ยน Key บนเว็บไซต์ GitHub (Settings -> SSH Keys)
> - ออปชัน `--unset-all` ต้องระบุชื่อ key เสมอ เช่น `git config --global --unset-all user.name` ไม่สามารถพิมพ์คำสั่งลอย ๆ โดยไม่มี key ได้

---

### 4.2 การตั้งค่า SSH Key สำหรับ Remote (SSH Key Configuration)

การเชื่อมต่อผ่าน SSH ช่วยให้สามารถ push/pull ได้อย่างปลอดภัยโดยไม่ต้องกรอกรหัสผ่านทุกครั้ง

```bash
# สร้าง SSH Key แบบ Ed25519 (แนะนำสำหรับระบบสมัยใหม่)
ssh-keygen -t ed25519 -C "your_email@example.com"

# หรือสร้าง SSH Key แบบ RSA 4096-bit (รูปแบบดั้งเดิม พร้อมออปชัน -o)
ssh-keygen -t rsa -b 4096 -o -C "your_email@example.com"

# แสดง Public Key เพื่อคัดลอก (เลือกไฟล์ตามประเภท key ที่สร้าง)
cat ~/.ssh/id_ed25519.pub
# หรือสำหรับ RSA:
cat ~/.ssh/id_rsa.pub
```

**ขั้นตอนนำ Key ไปใช้งาน:**
1. คัดลอกข้อความทั้งหมดที่แสดงจากคำสั่ง `cat`
2. ไปที่ GitHub -> **Settings** -> **SSH and GPG keys** -> คลิก **New SSH key**
3. ตั้งชื่อ Title แล้ววางข้อความ Key ในช่อง จากนั้นกด **Add SSH key**

---

### 4.3 การเริ่มต้นโปรเจกต์และการ Clone (Repository Initialization & Clone)

```bash
# เริ่มต้นสร้าง Git repository ใหม่ใน directory ปัจจุบัน
git init

# โคลน repository จาก remote ลงมายังเครื่อง
git clone <URL>

# โคลนเฉพาะ branch ที่ระบุลงมาใช้งาน
git clone -b <branch_name> <URL>
```

#### การยกเลิก Git จาก Directory อย่างถูกต้องและปลอดภัย
หากต้องการยกเลิกการติดตามของ Git และคืนค่าโฟลเดอร์ให้กลายเป็น directory ทั่วไป:

```bash
# ลบเฉพาะโฟลเดอร์ .git บน Linux/macOS
rm -rf .git

# หรือบน Windows PowerShell
Remove-Item -Recurse -Force .git
```

> ⚠️ **คำเตือนความเสี่ยงสูง (Irreversible History Warning):**
> - **หลีกเลี่ยงคำสั่ง `rm -rf .git*` โดยเด็ดขาด**: เพราะการใช้ wildcard `*` อาจลบไฟล์สำคัญอื่น ๆ ที่ขึ้นต้นด้วย `.git` ไปด้วยโดยไม่ตั้งใจ เช่น `.gitignore`, `.gitattributes` หรือโฟลเดอร์ `.github/`
> - **การลบโฟลเดอร์ `.git` เป็นการกระทำที่ไม่สามารถกู้คืนได้**: ประวัติการ commit ทั้งหมด, branch, tag, stash และการติดตามเวอร์ชันจะสูญหายทันที (คงเหลือเฉพาะไฟล์งานล่าสุดในปัจจุบัน)

---

### 4.4 การตรวจสอบสถานะและความเปลี่ยนแปลง (Inspection & Diff)

```bash
# ตรวจสอบสถานะของไฟล์ใน Working Tree และ Staging Area
git status

# ดูความแตกต่างของโค้ดที่แก้ไข (Modified) เทียบกับ commit ล่าสุด (ยังไม่ได้ git add)
git diff

# ดูความแตกต่างของไฟล์ที่ถูกเพิ่มเข้า Staging Area แล้ว (หลัง git add แต่ก่อน git commit)
git diff --staged
```

---

### 4.5 การจัดเตรียมและบันทึกความเปลี่ยนแปลง (Staging & Committing)

```bash
# นำไฟล์หรือ path ที่ระบุเข้าสู่ Staging Area
git add <path>

# นำไฟล์ที่มีการเปลี่ยนแปลงทั้งหมดใน directory ปัจจุบันเข้าสู่ Staging Area
git add .

# บันทึก commit พร้อมระบุข้อความอธิบายการเปลี่ยนแปลง
git commit -m "คำอธิบายการเปลี่ยนแปลงอย่างกระชับและชัดเจน"
```

#### การใช้งาน `git commit -a` อย่างถูกต้อง

```bash
# บันทึก commit พร้อม stage ไฟล์ที่ถูก tracked อยู่แล้วโดยอัตโนมัติ
git commit -a

# รวมการ stage ไฟล์ที่ tracked และใส่ข้อความ commit ในคำสั่งเดียว
git commit -am "คำอธิบายการเปลี่ยนแปลง"
```

> **คำชี้แจงสำคัญเรื่อง `git commit -a`:**
> - ออปชัน `-a` (หรือ `--all`) จะทำหน้าที่ auto-stage เฉพาะไฟล์ที่มีการแก้ไข (modified) หรือถูกลบ (deleted) ซึ่งเป็น **ไฟล์ที่ถูกติดตามอยู่ในระบบแล้ว (tracked files) เท่านั้น**
> - **ไม่รวมไฟล์สร้างใหม่ที่ยังไม่เคยถูกติดตาม (untracked files)** หากมีไฟล์สร้างใหม่ คุณจำเป็นต้องใช้คำสั่ง `git add <file>` ก่อนเสมอ

---

### 4.6 การจัดการ Remote Repository

```bash
# เพิ่ม Remote Repository และตั้งชื่อว่า origin
git remote add origin <URL>

# ตรวจสอบรายการ Remote URL ที่เชื่อมต่ออยู่ทั้งหมด (ทั้ง fetch และ push)
git remote -v

# ลบการเชื่อมต่อ Remote ที่ชื่อ origin
git remote remove origin
```

---

### 4.7 การส่งและดึงข้อมูล (Push & Pull)

```bash
# ส่ง commit ไปยัง Remote Branch พร้อมตั้งค่า upstream (-u) สำหรับครั้งแรก
git push -u origin <branch_name>

# ดึง commit ล่าสุดจาก Remote Branch มารวมเข้ากับ Local Branch ปัจจุบัน
git pull origin <branch_name>
```

#### ข้อควรระวังในการ Force Push และทางเลือกที่ปลอดภัย

```bash
# คำสั่ง Force Push แบบดั้งเดิม (มีความเสี่ยงสูง)
git push -f
# หรือ
git push --force
```

> ⚠️ **คำเตือนความเสี่ยงสูง:**
> - `git push -f` จะเขียนทับประวัติ commit บน Remote Branch โดยไม่สนใจว่ามีคนอื่น push โค้ดใหม่ขึ้นไปหรือไม่ ซึ่งอาจทำให้งานของเพื่อนร่วมทีมสูญหายอย่างถาวร
> - **แนวทางปฏิบัติที่ปลอดภัยกว่า**: ใช้ `--force-with-lease` แทน
>   ```bash
>   git push --force-with-lease
>   ```
>   คำสั่งนี้จะตรวจสอบก่อนว่า Remote มี commit ใหม่ที่เครื่องของคุณยังไม่ได้ดึงมาหรือไม่ หากมี Git จะปฏิเสธการ push เพื่อป้องกันการเขียนทับงานของผู้อื่นโดยไม่ได้ตั้งใจ

---

### 4.8 การยกเลิกและย้อนสถานะ (Reset, Restore & Revert)

```bash
# นำไฟล์ออกจาก Staging Area กลับสู่ Modified (Unstage ไฟล์ทั้งหมด)
git reset

# ทางเลือกสมัยใหม่: unstage เฉพาะไฟล์ที่ต้องการ
git restore --staged <file>
```

#### การทำ Hard Reset (`reset --hard`) และคำเตือนความเสี่ยง

```bash
# ย้อนกลับไปที่ HEAD ปัจจุบัน (ทิ้งการเปลี่ยนแปลงที่ยังไม่ commit ทั้งหมด)
git reset --hard HEAD

# ย้อนกลับไป 1 commit ก่อนหน้า HEAD
git reset --hard HEAD^
# หรือ (มีความหมายเทียบเท่ากัน)
git reset --hard HEAD~1

# ย้อนกลับไป 2 commit ก่อนหน้า HEAD
git reset --hard HEAD~2

# ย้อนกลับไปยัง commit hash ที่ระบุ (ตัวอย่าง: 0ad5a7a6)
git reset --hard 0ad5a7a6
```

> ⚠️ **คำเตือนความเสี่ยงสูงของ `reset --hard`:**
> - `git reset --hard` จะ**ทิ้งการเปลี่ยนแปลงทั้งหมดใน Working Tree และ Staging Area** โดยไม่สามารถกู้คืนได้
> - หากนำไปใช้บน branch ที่แชร์ร่วมกันกับทีม จะทำให้ประวัติของ Local และ Remote ไม่ตรงกัน เกิดปัญหาประวัติขัดแย้งเมื่อทำงานร่วมกัน
>
> **ทางเลือกที่ปลอดภัยกว่า:**
> 1. **ใช้ `git revert` สำหรับ commit ที่แชร์กับทีมแล้ว:**
>    ```bash
>    git revert <commit-hash>
>    ```
>    จะสร้าง commit ใหม่เพื่อลบล้างผลของ commit เดิม โดยไม่แก้ประวัติย้อนหลัง ปลอดภัยสำหรับการทำงานร่วมกัน
> 2. **ใช้ `git reset --soft` หากต้องการยกเลิก commit แต่เก็บโค้ดไว้:**
>    ```bash
>    git reset --soft HEAD~1
>    ```
> 3. **ใช้ `git restore` สำหรับยกเลิกการแก้ไขไฟล์เฉพาะจุด:**
>    ```bash
>    git restore <file>
>    ```

---

### 4.9 การจัดการ Branch และการเปลี่ยนชื่อ Default Branch

```bash
# แสดงรายชื่อ Local Branch
git branch

# แสดงรายชื่อ Branch ทั้งหมด ทั้ง Local และ Remote
git branch -a

# สร้าง Branch ใหม่
git branch <branch_name>

# สลับไปยัง Branch ที่ต้องการ
git checkout <branch_name>
# หรือใช้คำสั่งสมัยใหม่:
git switch <branch_name>

# สร้างและสลับไปยัง Branch ใหม่ในคำสั่งเดียว
git checkout -b <branch_name>
# หรือใช้คำสั่งสมัยใหม่:
git switch -c <branch_name>
```

#### การเปลี่ยนชื่อ Default Branch จาก `master` เป็น `main`

หากต้องการเปลี่ยนชื่อ branch หลักจาก `master` ไปเป็น `main` ให้เป็นไปตามมาตรฐานปัจจุบัน:

```bash
# 1. เปลี่ยนชื่อ branch ในเครื่องจาก master เป็น main
git branch -m master main

# 2. ดึงข้อมูลล่าสุดจาก Remote
git fetch origin

# 3. ตั้งค่า Upstream ให้ branch main ชี้ไปที่ origin/main
git branch -u origin/main main

# 4. อัปเดตการชี้ค่า HEAD ของ Remote origin ให้ตรงกับค่า default ใหม่
git remote set-head origin -a
```

---

## 5. กระบวนการทำงานร่วมกันผ่าน Branch และ Pull Request (Collaboration & PR Workflow)

แนวทางการทำงานร่วมกันในทีมแบบมืออาชีพ เพื่อป้องกันความผิดพลาดและตรวจสอบคุณภาพโค้ดก่อนรวมเข้าสู่ branch หลัก:

1. **อัปเดต branch หลักให้เป็นปัจจุบันเสมอ:**
   ```bash
   git checkout main
   git pull origin main
   ```

2. **สร้าง branch ใหม่สำหรับงานหรือฟีเจอร์ที่รับผิดชอบ:**
   ```bash
   # ตั้งชื่อ branch ตามลักษณะงาน เช่น feature/..., fix/..., docs/...
   git checkout -b docs/improve-readme
   ```

3. **แก้ไขโค้ด ตรวจสอบ และ commit เป็นระยะ:**
   ```bash
   git status
   git add README.md
   git commit -m "docs: improve readme structure and command examples"
   ```

4. **ส่ง branch ขึ้น Remote:**
   ```bash
   git push -u origin docs/improve-readme
   ```

5. **เปิด Pull Request (PR) หรือ Merge Request (MR):**
   - ไปที่ระบบ Git Hosting (เช่น GitHub) แล้วกดสร้าง Pull Request โดยตั้งค่าเป้าหมาย (Base branch) ไปที่ `main`
   - เขียนคำอธิบายสรุปสิ่งที่มีการเปลี่ยนแปลง ผลการทดสอบ และหมายเลข Issue ที่เกี่ยวข้อง

6. **กระบวนการ Code Review:**
   - รอการรีวิวจากผู้ร่วมทีม ตรวจสอบข้อเสนอแนะและผลการรัน CI/CD
   - หากมีข้อแก้ไข ให้ปรับปรุงโค้ดใน branch เดิม แล้ว commit และ push ตามปกติ PR จะอัปเดตให้อัตโนมัติ

7. **Merge เข้าสู่ branch หลักและลบ branch ย่อย:**
   - เมื่อได้รับการอนุมัติ (Approve) ทำการ Merge เข้า `main`
   - ลบ branch ที่ทำงานเสร็จแล้วทั้งบน Local และ Remote เพื่อรักษาความสะอาดของ repository

---

## 6. ตารางเปรียบเทียบคำสั่งที่มีความเสี่ยงและทางเลือกที่ปลอดภัย (Safety Cheat Sheet)

| คำสั่งที่มีความเสี่ยง | ความเสี่ยงที่อาจเกิดขึ้น | ทางเลือกที่ปลอดภัยกว่า (Recommended) | คำอธิบาย |
| :--- | :--- | :--- | :--- |
| `rm -rf .git*` | อาจลบไฟล์ `.gitignore`, `.github/` หรือไฟล์คอนฟิกอื่น ๆ ไปด้วย | `rm -rf .git` | ระบุลบเฉพาะโฟลเดอร์ `.git` เท่านั้น พร้อมตระหนักว่าจะสูญเสียประวัติทั้งหมดอย่างถาวร |
| `git push -f` | เขียนทับประวัติบน Remote ทำให้งานของผู้อื่นสูญหาย | `git push --force-with-lease` | ตรวจสอบว่ามี commit ของผู้อื่นหรือไม่ก่อนอนุญาตให้ force push |
| `git reset --hard` | ลบการแก้ไขที่ยังไม่ได้ commit ทิ้งทั้งหมดทันที | `git revert <commit>` หรือ `git restore` | ใช้ revert เพื่อสร้าง commit ลบล้างโดยไม่ทำลายประวัติ หรือใช้ restore เพื่อคืนค่าเฉพาะไฟล์ |
| `git commit -a` (เข้าใจผิดว่าเก็บทุกไฟล์) | ไฟล์สร้างใหม่ (untracked) จะไม่ถูกบันทึกใน commit | `git add . && git commit -m "..."` | ตรวจสอบสถานะด้วย `git status` ก่อน แล้วจึง add ไฟล์ทั้งหมด |
| `git config --unset-all` (ไม่มี key) | คำสั่งจะล้มเหลวเนื่องจากขาดชื่อ key ที่ต้องการลบ | `git config --unset-all <key>` | ต้องระบุชื่อ key ให้ชัดเจน เช่น `git config --global --unset-all user.name` |

