# Ananda Development — Internal Dashboard

ระบบ internal dashboard สำหรับทีม Ananda Development ใช้แสดงข้อมูลโครงการอสังหาริมทรัพย์ เช่น ยอดขาย สถานะการก่อสร้าง และรายงานสำหรับผู้บริหาร

## Tech Stack

- [Next.js 14](https://nextjs.org/) App Router
- [TypeScript](https://www.typescriptlang.org/)
- [Tailwind CSS](https://tailwindcss.com/)
- [shadcn/ui](https://ui.shadcn.com/)
- [Prisma](https://www.prisma.io/) + PostgreSQL
- [NextAuth.js](https://next-auth.js.org/)

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL database

### Installation

```bash
# Install dependencies
npm install

# Copy environment variables
cp .env.example .env.local
# แก้ไข .env.local ด้วย values ที่ถูกต้อง

# Run database migrations
npx prisma migrate dev

# Start development server
npm run dev
```

เปิด [http://localhost:3000](http://localhost:3000) ในเบราว์เซอร์

## Environment Variables

| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `NEXTAUTH_SECRET` | Secret key สำหรับ NextAuth.js |
| `NEXTAUTH_URL` | Base URL ของแอปพลิเคชัน |

## Commands

```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run type-check   # TypeScript type checking
npm run lint         # ESLint
npx prisma migrate dev   # Run database migrations
npx prisma studio        # Open Prisma Studio
```

## Project Structure

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

## Contributing

1. แตก branch จาก `develop`: `git checkout -b feature/your-feature`
2. ทำ type check ก่อน commit: `npm run type-check && npm run lint`
3. เปิด PR ไปที่ `develop` พร้อม description ที่ชัดเจน
4. รอ code review อย่างน้อย 1 คนก่อน merge
