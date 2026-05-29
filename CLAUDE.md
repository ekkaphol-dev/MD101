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

## Architecture

- **App Router** — layout สำหรับ auth และ dashboard แยกกัน ใช้ middleware ป้องกัน route
- **Data fetching** — ดึงข้อมูลใน Server Component โดยตรงผ่าน Prisma; ใช้ React Query สำหรับ client-side mutations
- **Auth** — NextAuth.js กับ credentials provider เชื่อมกับ AD ของบริษัท
- **Database** — PostgreSQL ผ่าน Prisma ORM; schema อยู่ที่ `prisma/schema.prisma`
