# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ระบบ internal dashboard สำหรับทีม Ananda Development ใช้แสดงข้อมูลโครงการอสังหาริมทรัพย์ เช่น ยอดขาย สถานะการก่อสร้าง และรายงานสำหรับผู้บริหาร

## Tech stack

- Next.js 14 App Router
- TypeScript
- Tailwind CSS
- shadcn/ui
- Prisma + PostgreSQL
- NextAuth.js (authentication)

## Folder Structure

```text
src/
├── app/                  # Next.js App Router pages
│   ├── (auth)/           # Auth routes (login, register)
│   ├── (dashboard)/      # Protected dashboard routes
│   │   ├── projects/     # Project listing & detail
│   │   └── reports/      # Reports pages
│   └── api/              # API route handlers
├── components/
│   ├── ui/               # shadcn/ui base components
│   └── shared/           # Reusable app components
├── lib/
│   ├── db.ts             # Prisma client singleton
│   └── utils.ts          # Utility functions
├── types/                # Shared TypeScript types
└── hooks/                # Custom React hooks
```

## Security Rules

### Authentication & Authorization

- ทุก route ใน `(dashboard)/` ต้องผ่าน middleware auth check
- ใช้ `getServerSession()` ใน Server Component — ห้ามเชื่อถือ client-side session โดยตรง
- ตรวจสอบ role/permission ในทุก API route handler ก่อน return ข้อมูล

### Input Validation

- Validate ทุก user input ด้วย Zod ก่อนนำไปใช้หรือ query database
- ห้ามใช้ raw SQL string concatenation — ใช้ Prisma query เสมอเพื่อป้องกัน SQL injection
- Sanitize ข้อมูลก่อน render ใน HTML เพื่อป้องกัน XSS

### Secrets & Environment

- ห้าม hardcode secret, API key หรือ credential ในโค้ดเด็ดขาด
- `.env.local` ใช้สำหรับ local dev เท่านั้น — ห้าม commit
- ตัวแปรที่ขึ้นต้นด้วย `NEXT_PUBLIC_` จะ expose ไปฝั่ง client — ต้องไม่ใช่ข้อมูลลับ

### API Security

- ทุก API route ต้องเช็ค HTTP method ที่ยอมรับก่อนประมวลผล
- Response ต้องไม่รั่ว stack trace หรือ internal error ออกไปยัง client
- ใส่ rate limiting สำหรับ endpoint ที่ sensitive เช่น login, data export

### Dependencies

- รัน `npm audit` ก่อน merge ทุกครั้ง
- อัปเดต dependency ที่มี critical vulnerability ทันที

## Coding Rules

- ใช้ TypeScript strict mode — ห้าม `any` โดยไม่มีเหตุผล
- Component ต้องเป็น Server Component by default ถ้าไม่ต้องการ interactivity
- ใช้ `"use client"` เฉพาะเมื่อจำเป็น
- ตั้งชื่อ file แบบ kebab-case เช่น `project-card.tsx`
- ตั้งชื่อ Component แบบ PascalCase เช่น `ProjectCard`
- API routes ต้อง validate input ด้วย Zod ทุกครั้ง
- ห้าม commit ข้อมูล credentials หรือ secret ลงใน repository

## Commands

```bash
# Dev server
npm run dev

# Build
npm run build

# Type check
npm run type-check

# Lint
npm run lint

# Database migrations
npx prisma migrate dev
npx prisma studio
```

## Workflow

ก่อนแก้โค้ดทำตามนี้

1. อ่านไฟล์ที่เกี่ยวข้องก่อน
2. อธิบายแผนการแก้แบบสั้นๆ
3. แก้เฉพาะไฟล์ที่จำเป็น
4. ตรวจว่าโค้ดไม่กระทบส่วนอื่น (`npm run type-check && npm run lint`)
5. สรุปสิ่งที่แก้หลังทำเสร็จ

## Architecture

- **App Router** — layout สำหรับ auth และ dashboard แยกกัน ใช้ middleware ป้องกัน route
- **Data fetching** — ดึงข้อมูลใน Server Component โดยตรงผ่าน Prisma; ใช้ React Query สำหรับ client-side mutations
- **Auth** — NextAuth.js กับ credentials provider เชื่อมกับ AD ของบริษัท
- **Database** — PostgreSQL ผ่าน Prisma ORM; schema อยู่ที่ `prisma/schema.prisma`
