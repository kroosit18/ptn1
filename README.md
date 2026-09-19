Employee Hub — SPA

ระบบข้อมูลพนักงานแบบ Single Page Application (SPA) ใช้:

Frontend: index.html ไฟล์เดียว รวม HTML + Tailwind CSS + JavaScript

Database/Auth/Storage: Supabase

Charts: Chart.js

Icons: Lucide

Font: Kanit

Hosting: GitHub Pages / GitHub repository ใดก็ได้ที่เสิร์ฟ index.html

โครงสร้างระบบ

employee-spa/
├─ index.html      # Frontend ทั้งหมดในไฟล์เดียว
├─ supabase.sql    # Schema + RLS + Storage + Policies + Sample Data
└─ README.md

Tables

departments — แผนก

positions — ตำแหน่ง

employees — ข้อมูลพนักงานและข้อมูลรูป

activity_logs — ประวัติกิจกรรมของระบบ

Storage

Bucket: employee-avatars

Public bucket สำหรับแสดงรูปผ่าน public URL

รับ JPEG / PNG / WebP

จำกัด bucket 256KB ต่อไฟล์

Frontend resize ด้านยาวสูงสุด 600px และแปลงเป็น WebP ก่อนอัปโหลด

เป้าหมายไฟล์บีบอัดประมาณไม่เกิน 150KB

Insert / Update / Delete ต้องเป็นผู้ใช้ authenticated

วิธีติดตั้ง

1. สร้าง Supabase Project

สร้างโปรเจกต์ใหม่ใน Supabase จากนั้นเปิด SQL Editor

2. รัน SQL ครั้งเดียว

เปิด supabase.sql → Copy ทั้งไฟล์ → วางใน Supabase SQL Editor → Run

สคริปต์จะสร้างตาราง, index, trigger, RLS, 4 policies ต่อหนึ่งตาราง, bucket, Storage policies และข้อมูลตัวอย่าง

3. เปิดเว็บ

เปิด index.html ใน browser หรือ push ขึ้น GitHub แล้วเปิดด้วย GitHub Pages

4. เชื่อมต่อ Supabase

ไปที่ ตั้งค่าระบบ ในเว็บ แล้วกรอก:

Project URL

Publishable / Anon Key

ค่าเหล่านี้ถูกเก็บใน localStorage ของ browser เครื่องนั้น

ห้ามใส่ service_role key ใน frontend

5. อัปโหลดรูป

กดเข้าสู่ระบบ → ใช้อีเมล/รหัสผ่านของ Supabase Auth → เพิ่มหรือแก้ไขพนักงาน → เลือกไฟล์รูปจากเครื่อง

ระบบจะบีบอัดไฟล์บน browser ก่อนส่งไป Storage และเก็บเฉพาะ path + public URL ลงในตาราง employees

RLS ที่ใช้ในโหมดทดสอบ

ตาราง public ทุกตัวเปิด RLS และมี policy 4 ตัว:

SELECT: USING (true)

INSERT: WITH CHECK (true)

UPDATE: USING (true) WITH CHECK (true)

DELETE: USING (true)

รูปใน Storage ใช้ policy แยก 4 ตัว โดย public อ่านไฟล์ได้ และ authenticated เท่านั้นที่ upload/update/delete ได้

นโยบายนี้สะดวกสำหรับการทดสอบ แต่ไม่ควรนำขึ้นระบบ production ที่มีข้อมูลพนักงานจริงโดยไม่ปรับสิทธิ์ตามบทบาทผู้ใช้

ฟังก์ชันหลัก

Dashboard summary

Employee CRUD

Search / filter

Department / Position relationship

Recent activity log

Supabase Auth sign-in/sign-up/sign-out

Image upload + compression

Image fallback เมื่อรูปโหลดไม่ได้

Global loader ทุก operation หลัก

Toast error/success

ล้างฟอร์ม/ตัวกรองโดยไม่ reload หน้าเว็บ

Responsive Mobile First

GitHub Pages

สร้าง repository ใหม่

อัปโหลด index.html, supabase.sql, README.md

ไปที่ Settings → Pages

เลือก Deploy from a branch → main / root

เปิด URL ที่ GitHub Pages ให้
