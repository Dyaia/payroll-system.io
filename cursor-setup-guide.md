# 🚀 Payroll Management System - Complete Technical Package
## نظام إدارة الرواتب - الحزمة التقنية الكاملة

**Version:** 1.0  
**Date:** March 2026  
**Developer:** Dyaia Hameed  
**Status:** Ready for Development in Cursor AI

---

## 📁 Project Structure

```
payroll-system/
├── backend/                      # FastAPI Backend
│   ├── alembic/                 # Database migrations
│   │   ├── versions/
│   │   └── env.py
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py             # FastAPI app entry
│   │   ├── config.py           # Configuration
│   │   ├── database.py         # Database connection
│   │   ├── models/             # SQLAlchemy models
│   │   │   ├── __init__.py
│   │   │   ├── employee.py
│   │   │   ├── department.py
│   │   │   ├── salary_component.py
│   │   │   ├── payroll.py
│   │   │   ├── attendance.py
│   │   │   ├── leave.py
│   │   │   ├── loan.py
│   │   │   └── user.py
│   │   ├── schemas/            # Pydantic schemas
│   │   │   ├── __init__.py
│   │   │   ├── employee.py
│   │   │   ├── payroll.py
│   │   │   ├── attendance.py
│   │   │   └── auth.py
│   │   ├── routers/            # API endpoints
│   │   │   ├── __init__.py
│   │   │   ├── employees.py
│   │   │   ├── departments.py
│   │   │   ├── payroll.py
│   │   │   ├── attendance.py
│   │   │   ├── leaves.py
│   │   │   ├── reports.py
│   │   │   └── auth.py
│   │   ├── services/           # Business logic
│   │   │   ├── __init__.py
│   │   │   ├── payroll_engine.py
│   │   │   ├── salary_calculator.py
│   │   │   ├── payslip_generator.py
│   │   │   ├── notification_service.py
│   │   │   ├── zoho_service.py
│   │   │   └── encryption_service.py
│   │   ├── utils/              # Utilities
│   │   │   ├── __init__.py
│   │   │   ├── security.py
│   │   │   ├── validators.py
│   │   │   └── helpers.py
│   │   └── middleware/         # Middlewares
│   │       ├── __init__.py
│   │       ├── auth.py
│   │       └── cors.py
│   ├── tests/                  # Tests
│   │   ├── __init__.py
│   │   ├── test_employees.py
│   │   ├── test_payroll.py
│   │   └── test_auth.py
│   ├── requirements.txt        # Python dependencies
│   ├── .env.example           # Environment variables template
│   ├── Dockerfile
│   └── README.md
│
├── frontend/                    # Next.js Frontend
│   ├── src/
│   │   ├── app/
│   │   │   ├── (auth)/
│   │   │   │   ├── login/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── layout.tsx
│   │   │   ├── (dashboard)/
│   │   │   │   ├── dashboard/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── employees/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   ├── [id]/
│   │   │   │   │   │   └── page.tsx
│   │   │   │   │   └── new/
│   │   │   │   │       └── page.tsx
│   │   │   │   ├── payroll/
│   │   │   │   │   ├── page.tsx
│   │   │   │   │   └── [id]/
│   │   │   │   │       └── page.tsx
│   │   │   │   ├── attendance/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── leaves/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── reports/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── layout.tsx
│   │   │   ├── layout.tsx
│   │   │   └── globals.css
│   │   ├── components/
│   │   │   ├── ui/              # shadcn components
│   │   │   │   ├── button.tsx
│   │   │   │   ├── input.tsx
│   │   │   │   ├── card.tsx
│   │   │   │   ├── table.tsx
│   │   │   │   ├── dialog.tsx
│   │   │   │   └── ...
│   │   │   ├── layouts/
│   │   │   │   ├── Sidebar.tsx
│   │   │   │   ├── Header.tsx
│   │   │   │   └── Footer.tsx
│   │   │   ├── forms/
│   │   │   │   ├── EmployeeForm.tsx
│   │   │   │   ├── PayrollForm.tsx
│   │   │   │   └── LeaveForm.tsx
│   │   │   └── tables/
│   │   │       ├── EmployeeTable.tsx
│   │   │       └── PayrollTable.tsx
│   │   ├── lib/
│   │   │   ├── api.ts          # API client
│   │   │   ├── utils.ts        # Utilities
│   │   │   └── constants.ts
│   │   └── stores/
│   │       ├── auth.ts         # Zustand auth store
│   │       └── employees.ts
│   ├── public/
│   │   ├── images/
│   │   └── icons/
│   ├── package.json
│   ├── tsconfig.json
│   ├── tailwind.config.ts
│   ├── next.config.js
│   ├── .env.local.example
│   └── README.md
│
├── mobile/                      # Flutter Mobile App
│   ├── lib/
│   │   ├── main.dart
│   │   ├── models/
│   │   │   ├── employee.dart
│   │   │   ├── payslip.dart
│   │   │   └── attendance.dart
│   │   ├── screens/
│   │   │   ├── login_screen.dart
│   │   │   ├── home_screen.dart
│   │   │   ├── payslip_screen.dart
│   │   │   └── attendance_screen.dart
│   │   ├── widgets/
│   │   │   ├── custom_button.dart
│   │   │   └── custom_card.dart
│   │   ├── services/
│   │   │   ├── api_service.dart
│   │   │   └── auth_service.dart
│   │   └── utils/
│   │       ├── constants.dart
│   │       └── helpers.dart
│   ├── assets/
│   │   ├── images/
│   │   └── fonts/
│   ├── pubspec.yaml
│   └── README.md
│
├── docker-compose.yml          # Docker orchestration
├── .github/
│   └── workflows/
│       ├── backend-ci.yml
│       └── frontend-ci.yml
├── docs/                        # Documentation
│   ├── API.md
│   ├── DATABASE.md
│   └── DEPLOYMENT.md
└── README.md                    # Main README

```

---

## 🔧 Backend Setup Instructions

### 1. Prerequisites
```bash
# Required software
Python 3.11+
PostgreSQL 15+
Redis 7+
Git
```

### 2. Clone and Setup
```bash
# Clone the repository
git clone https://github.com/your-org/payroll-system.git
cd payroll-system/backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On Mac/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

### 3. requirements.txt
```txt
# FastAPI & Server
fastapi==0.110.0
uvicorn[standard]==0.27.0
python-multipart==0.0.9

# Database
sqlalchemy==2.0.25
alembic==1.13.1
psycopg2-binary==2.9.9

# Authentication & Security
fastapi-users[sqlalchemy]==12.1.3
python-jose[cryptography]==3.3.0
passlib[bcrypt]==1.7.4
cryptography==42.0.2

# Validation & Serialization
pydantic==2.6.0
pydantic-settings==2.1.0
email-validator==2.1.0

# Task Queue
celery==5.3.6
redis==5.0.1

# Email & Notifications
sendgrid==6.11.0
twilio==8.12.0

# PDF Generation
reportlab==4.0.9
python-bidi==0.4.2

# Excel/CSV
openpyxl==3.1.2
pandas==2.2.0

# HTTP Clients
httpx==0.26.0
requests==2.31.0

# Monitoring & Logging
sentry-sdk==1.40.0
python-json-logger==2.0.7

# Testing
pytest==8.0.0
pytest-asyncio==0.23.4
pytest-cov==4.1.0

# Code Quality
ruff==0.2.0
mypy==1.8.0
black==24.1.1

# Utilities
python-dotenv==1.0.1
python-dateutil==2.8.2
pytz==2024.1
```

### 4. Environment Variables (.env)
```bash
# Application
APP_NAME="Payroll Management System"
APP_ENV=development
DEBUG=True
SECRET_KEY=your-super-secret-key-change-in-production
API_V1_PREFIX=/api/v1

# Database
DATABASE_URL=postgresql://postgres:password@localhost:5432/payroll_db
DATABASE_POOL_SIZE=10
DATABASE_MAX_OVERFLOW=20

# Redis
REDIS_URL=redis://localhost:6379/0

# Celery
CELERY_BROKER_URL=redis://localhost:6379/1
CELERY_RESULT_BACKEND=redis://localhost:6379/2

# JWT
JWT_SECRET_KEY=your-jwt-secret-key
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
REFRESH_TOKEN_EXPIRE_DAYS=7

# Encryption (for sensitive data)
ENCRYPTION_KEY=your-32-byte-encryption-key-base64

# CORS
CORS_ORIGINS=["http://localhost:3000", "http://localhost:5173"]

# SendGrid (Email)
SENDGRID_API_KEY=your-sendgrid-api-key
SENDGRID_FROM_EMAIL=noreply@company.com
SENDGRID_FROM_NAME=Payroll System

# Twilio (SMS/WhatsApp)
TWILIO_ACCOUNT_SID=your-twilio-account-sid
TWILIO_AUTH_TOKEN=your-twilio-auth-token
TWILIO_PHONE_NUMBER=+1234567890

# Zoho CRM
ZOHO_CLIENT_ID=your-zoho-client-id
ZOHO_CLIENT_SECRET=your-zoho-client-secret
ZOHO_REDIRECT_URI=http://localhost:8000/api/zoho/callback
ZOHO_REFRESH_TOKEN=your-zoho-refresh-token

# File Storage
STORAGE_TYPE=local  # or 's3'
S3_BUCKET_NAME=payroll-files
S3_REGION=us-east-1
AWS_ACCESS_KEY_ID=your-aws-access-key
AWS_SECRET_ACCESS_KEY=your-aws-secret-key

# Sentry (Error Monitoring)
SENTRY_DSN=your-sentry-dsn

# Rate Limiting
RATE_LIMIT_PER_MINUTE=60
```

### 5. Database Setup
```bash
# Initialize Alembic
alembic init alembic

# Create initial migration
alembic revision --autogenerate -m "Initial schema"

# Apply migrations
alembic upgrade head

# Seed initial data (optional)
python scripts/seed_data.py
```

### 6. Run Backend
```bash
# Development (with auto-reload)
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000

# Production
uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4

# Celery Worker (for background tasks)
celery -A app.celery_app worker --loglevel=info

# Celery Beat (for scheduled tasks)
celery -A app.celery_app beat --loglevel=info
```

---

## 🎨 Frontend Setup Instructions

### 1. Prerequisites
```bash
Node.js 18+
npm or yarn
```

### 2. Setup
```bash
cd frontend

# Install dependencies
npm install
# or
yarn install
```

### 3. package.json (Key Dependencies)
```json
{
  "name": "payroll-frontend",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "^14.1.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "@radix-ui/react-alert-dialog": "^1.0.5",
    "@radix-ui/react-dialog": "^1.0.5",
    "@radix-ui/react-dropdown-menu": "^2.0.6",
    "@radix-ui/react-select": "^2.0.0",
    "@radix-ui/react-tabs": "^1.0.4",
    "@tanstack/react-query": "^5.17.19",
    "axios": "^1.6.5",
    "zustand": "^4.5.0",
    "react-hook-form": "^7.49.3",
    "zod": "^3.22.4",
    "@hookform/resolvers": "^3.3.4",
    "date-fns": "^3.3.0",
    "recharts": "^2.10.4",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.0",
    "tailwind-merge": "^2.2.0",
    "lucide-react": "^0.309.0"
  },
  "devDependencies": {
    "@types/node": "^20.11.5",
    "@types/react": "^18.2.48",
    "@types/react-dom": "^18.2.18",
    "typescript": "^5.3.3",
    "tailwindcss": "^3.4.1",
    "autoprefixer": "^10.4.17",
    "postcss": "^8.4.33",
    "eslint": "^8.56.0",
    "eslint-config-next": "^14.1.0"
  }
}
```

### 4. Environment Variables (.env.local)
```bash
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_APP_NAME=Payroll Management System
NEXT_PUBLIC_APP_VERSION=1.0.0
```

### 5. Run Frontend
```bash
# Development
npm run dev

# Build for production
npm run build

# Run production build
npm run start
```

---

## 📱 Mobile App Setup

### 1. Prerequisites
```bash
Flutter 3.16+
Dart 3+
Android Studio / Xcode
```

### 2. Setup
```bash
cd mobile

# Get dependencies
flutter pub get

# Run on device/emulator
flutter run

# Build APK
flutter build apk

# Build iOS
flutter build ios
```

### 3. pubspec.yaml (Key Dependencies)
```yaml
dependencies:
  flutter:
    sdk: flutter
  
  # State Management
  flutter_riverpod: ^2.4.10
  
  # HTTP Client
  dio: ^5.4.0
  
  # Local Storage
  hive: ^2.2.3
  hive_flutter: ^1.1.0
  
  # Secure Storage
  flutter_secure_storage: ^9.0.0
  
  # Authentication
  local_auth: ^2.1.7
  
  # UI
  google_fonts: ^6.1.0
  flutter_svg: ^2.0.9
  
  # PDF Viewer
  flutter_pdfview: ^1.3.2
  
  # Notifications
  firebase_messaging: ^14.7.6
  
  # Utilities
  intl: ^0.18.1
  path_provider: ^2.1.2
```

---

## 🐳 Docker Setup

### docker-compose.yml
```yaml
version: '3.8'

services:
  # PostgreSQL Database
  db:
    image: postgres:15-alpine
    container_name: payroll_db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: payroll_db
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Redis Cache
  redis:
    image: redis:7-alpine
    container_name: payroll_redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5

  # Backend API
  backend:
    build: ./backend
    container_name: payroll_backend
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://postgres:postgres@db:5432/payroll_db
      REDIS_URL: redis://redis:6379/0
    volumes:
      - ./backend:/app
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy

  # Celery Worker
  celery_worker:
    build: ./backend
    container_name: payroll_celery_worker
    command: celery -A app.celery_app worker --loglevel=info
    environment:
      DATABASE_URL: postgresql://postgres:postgres@db:5432/payroll_db
      CELERY_BROKER_URL: redis://redis:6379/1
      CELERY_RESULT_BACKEND: redis://redis:6379/2
    volumes:
      - ./backend:/app
    depends_on:
      - db
      - redis

  # Frontend
  frontend:
    build: ./frontend
    container_name: payroll_frontend
    ports:
      - "3000:3000"
    environment:
      NEXT_PUBLIC_API_URL: http://localhost:8000
    volumes:
      - ./frontend:/app
      - /app/node_modules
    depends_on:
      - backend

volumes:
  postgres_data:
  redis_data:
```

### Quick Start with Docker
```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down

# Rebuild and start
docker-compose up -d --build

# Run migrations
docker-compose exec backend alembic upgrade head

# Access backend shell
docker-compose exec backend python

# Access database
docker-compose exec db psql -U postgres -d payroll_db
```

---

## 🚀 Key Backend Files to Create in Cursor

### 1. app/main.py
```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from app.routers import employees, departments, payroll, attendance, leaves, reports, auth
from app.database import engine
from app.models import Base
import sentry_sdk

# Initialize Sentry
sentry_sdk.init(dsn="your-sentry-dsn")

# Create tables
Base.metadata.create_all(bind=engine)

app = FastAPI(
    title="Payroll Management API",
    description="Complete payroll management system API",
    version="1.0.0",
    docs_url="/docs",
    redoc_url="/redoc"
)

# CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Include routers
app.include_router(auth.router, prefix="/api/auth", tags=["Authentication"])
app.include_router(employees.router, prefix="/api/employees", tags=["Employees"])
app.include_router(departments.router, prefix="/api/departments", tags=["Departments"])
app.include_router(payroll.router, prefix="/api/payroll", tags=["Payroll"])
app.include_router(attendance.router, prefix="/api/attendance", tags=["Attendance"])
app.include_router(leaves.router, prefix="/api/leaves", tags=["Leaves"])
app.include_router(reports.router, prefix="/api/reports", tags=["Reports"])

@app.get("/")
async def root():
    return {"message": "Payroll Management API", "version": "1.0.0"}

@app.get("/health")
async def health_check():
    return {"status": "healthy"}
```

### 2. app/database.py
```python
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from app.config import settings

SQLALCHEMY_DATABASE_URL = settings.DATABASE_URL

engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    pool_size=settings.DATABASE_POOL_SIZE,
    max_overflow=settings.DATABASE_MAX_OVERFLOW,
    echo=settings.DEBUG
)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

### 3. app/models/employee.py
```python
from sqlalchemy import Column, String, Date, Decimal, Integer, ForeignKey, Boolean, Text, DateTime, Enum as SQLEnum
from sqlalchemy.orm import relationship
from datetime import datetime
import enum
from app.database import Base

class EmploymentStatus(enum.Enum):
    active = "active"
    inactive = "inactive"
    terminated = "terminated"
    on_leave = "on_leave"

class ContractType(enum.Enum):
    permanent = "permanent"
    temporary = "temporary"
    contract = "contract"
    part_time = "part_time"

class Employee(Base):
    __tablename__ = "employees"

    employee_id = Column(String(20), primary_key=True, index=True)
    full_name_ar = Column(String(100), nullable=False)
    full_name_en = Column(String(100))
    nationality = Column(String(50))
    national_id = Column(String(20), unique=True)
    iqama_number = Column(String(20))
    passport_number = Column(String(20))
    date_of_birth = Column(Date)
    gender = Column(String(10))
    marital_status = Column(String(20))
    
    # Employment Info
    hire_date = Column(Date, nullable=False)
    contract_type = Column(SQLEnum(ContractType), default=ContractType.permanent)
    department_id = Column(Integer, ForeignKey("departments.id"))
    job_title_id = Column(Integer, ForeignKey("job_titles.id"))
    manager_id = Column(String(20), ForeignKey("employees.employee_id"))
    work_location = Column(String(100))
    employment_status = Column(SQLEnum(EmploymentStatus), default=EmploymentStatus.active, index=True)
    
    # Salary Info
    base_salary = Column(Decimal(10, 2), nullable=False)
    payment_method = Column(String(20))
    
    # Bank Details (Encrypted)
    bank_name = Column(String(100))
    bank_account_number = Column(Text)  # Encrypted
    iban = Column(String(34))
    
    # Contact
    phone_number = Column(String(20))
    emergency_contact = Column(String(20))
    email = Column(String(100), unique=True, index=True)
    address = Column(Text)
    
    # GOSI
    gosi_number = Column(String(20))
    gosi_subscription_date = Column(Date)
    
    # Metadata
    created_at = Column(DateTime, default=datetime.utcnow)
    updated_at = Column(DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)
    created_by = Column(Integer, ForeignKey("users.id"))
    updated_by = Column(Integer, ForeignKey("users.id"))
    
    # Relationships
    department = relationship("Department", back_populates="employees")
    job_title = relationship("JobTitle", back_populates="employees")
    manager = relationship("Employee", remote_side=[employee_id], back_populates="subordinates")
    subordinates = relationship("Employee", back_populates="manager")
    salary_structure = relationship("EmployeeSalaryStructure", back_populates="employee")
    attendance_records = relationship("AttendanceRecord", back_populates="employee")
    leave_requests = relationship("LeaveRequest", back_populates="employee")
    loans = relationship("Loan", back_populates="employee")
    payslips = relationship("Payslip", back_populates="employee")
```

### 4. app/services/payroll_engine.py
```python
from decimal import Decimal
from datetime import date, timedelta
from sqlalchemy.orm import Session
from app.models.employee import Employee
from app.models.salary_component import SalaryComponent
from app.models.attendance import AttendanceRecord
from app.models.loan import Loan

class PayrollEngine:
    def __init__(self, db: Session, pay_period_start: date, pay_period_end: date):
        self.db = db
        self.start = pay_period_start
        self.end = pay_period_end
    
    async def calculate_employee_salary(self, employee_id: str):
        employee = self.db.query(Employee).filter(Employee.employee_id == employee_id).first()
        
        # 1. Calculate Earnings
        earnings = await self._calculate_earnings(employee)
        
        # 2. Calculate Deductions
        deductions = await self._calculate_deductions(employee)
        
        # 3. Calculate Totals
        gross = sum(earnings.values())
        total_deductions = sum(deductions.values())
        net = gross - total_deductions
        
        return {
            "employee_id": employee_id,
            "earnings": earnings,
            "deductions": deductions,
            "gross_salary": float(gross),
            "total_deductions": float(total_deductions),
            "net_salary": float(net)
        }
    
    async def _calculate_earnings(self, employee: Employee):
        earnings = {}
        
        # Basic Salary
        earnings["basic_salary"] = float(employee.base_salary)
        
        # Housing Allowance (25% of basic)
        earnings["housing_allowance"] = float(employee.base_salary * Decimal("0.25"))
        
        # Transport Allowance (fixed)
        earnings["transport_allowance"] = 500.00
        
        # Overtime Pay
        overtime_hours = self._get_overtime_hours(employee.employee_id)
        if overtime_hours > 0:
            hourly_rate = employee.base_salary / Decimal("30") / Decimal("8")
            earnings["overtime_pay"] = float(hourly_rate * Decimal(str(overtime_hours)) * Decimal("1.5"))
        
        return earnings
    
    async def _calculate_deductions(self, employee: Employee):
        deductions = {}
        
        # GOSI
        gosi_base = employee.base_salary + (employee.base_salary * Decimal("0.25"))
        if employee.nationality == "Saudi":
            deductions["gosi"] = float(gosi_base * Decimal("0.10"))
        else:
            deductions["gosi"] = float(gosi_base * Decimal("0.02"))
        
        # Absence Deduction
        absence_days = self._get_absence_days(employee.employee_id)
        if absence_days > 0:
            daily_rate = employee.base_salary / Decimal("30")
            deductions["absence"] = float(daily_rate * Decimal(str(absence_days)))
        
        # Loan Deduction
        loan = self.db.query(Loan).filter(
            Loan.employee_id == employee.employee_id,
            Loan.status == "active"
        ).first()
        if loan:
            deductions["loan"] = float(loan.monthly_deduction)
        
        return deductions
    
    def _get_overtime_hours(self, employee_id: str) -> float:
        records = self.db.query(AttendanceRecord).filter(
            AttendanceRecord.employee_id == employee_id,
            AttendanceRecord.date >= self.start,
            AttendanceRecord.date <= self.end
        ).all()
        return sum(r.overtime_hours or 0 for r in records)
    
    def _get_absence_days(self, employee_id: str) -> int:
        records = self.db.query(AttendanceRecord).filter(
            AttendanceRecord.employee_id == employee_id,
            AttendanceRecord.date >= self.start,
            AttendanceRecord.date <= self.end,
            AttendanceRecord.status == "absent"
        ).all()
        return len(records)
```

---

## 📊 SQL Schema for Quick Setup

```sql
-- Run this in PostgreSQL to create all tables quickly

-- 1. Departments
CREATE TABLE departments (
    id SERIAL PRIMARY KEY,
    department_name_ar VARCHAR(100) NOT NULL,
    department_name_en VARCHAR(100),
    department_code VARCHAR(20) UNIQUE,
    manager_id VARCHAR(20),
    parent_department_id INTEGER REFERENCES departments(id),
    cost_center VARCHAR(50),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 2. Job Titles
CREATE TABLE job_titles (
    id SERIAL PRIMARY KEY,
    title_ar VARCHAR(100) NOT NULL,
    title_en VARCHAR(100),
    job_code VARCHAR(20) UNIQUE,
    job_level INTEGER CHECK (job_level BETWEEN 1 AND 10),
    min_salary NUMERIC(10, 2),
    max_salary NUMERIC(10, 2),
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CHECK (max_salary >= min_salary)
);

-- 3. Employees
CREATE TABLE employees (
    employee_id VARCHAR(20) PRIMARY KEY,
    full_name_ar VARCHAR(100) NOT NULL,
    full_name_en VARCHAR(100),
    nationality VARCHAR(50),
    national_id VARCHAR(20) UNIQUE,
    iqama_number VARCHAR(20),
    passport_number VARCHAR(20),
    date_of_birth DATE,
    gender VARCHAR(10),
    marital_status VARCHAR(20),
    hire_date DATE NOT NULL,
    contract_type VARCHAR(20),
    department_id INTEGER REFERENCES departments(id),
    job_title_id INTEGER REFERENCES job_titles(id),
    manager_id VARCHAR(20) REFERENCES employees(employee_id),
    work_location VARCHAR(100),
    employment_status VARCHAR(20) DEFAULT 'active',
    base_salary NUMERIC(10, 2) NOT NULL,
    payment_method VARCHAR(20),
    bank_name VARCHAR(100),
    bank_account_number TEXT,
    iban VARCHAR(34),
    phone_number VARCHAR(20),
    emergency_contact VARCHAR(20),
    email VARCHAR(100) UNIQUE,
    address TEXT,
    gosi_number VARCHAR(20),
    gosi_subscription_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 4. Salary Components
CREATE TABLE salary_components (
    id SERIAL PRIMARY KEY,
    component_name_ar VARCHAR(100) NOT NULL,
    component_name_en VARCHAR(100),
    component_code VARCHAR(20) UNIQUE NOT NULL,
    component_type VARCHAR(20) NOT NULL,
    calculation_type VARCHAR(20) NOT NULL,
    default_amount NUMERIC(10, 2),
    default_percentage NUMERIC(5, 2),
    formula TEXT,
    is_taxable BOOLEAN DEFAULT FALSE,
    affects_gosi BOOLEAN DEFAULT FALSE,
    is_recurring BOOLEAN DEFAULT TRUE,
    display_order INTEGER,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 5. Payroll Runs
CREATE TABLE payroll_runs (
    id SERIAL PRIMARY KEY,
    run_name VARCHAR(100) NOT NULL,
    pay_period_start DATE NOT NULL,
    pay_period_end DATE NOT NULL,
    payment_date DATE NOT NULL,
    status VARCHAR(20) DEFAULT 'draft',
    total_employees INTEGER DEFAULT 0,
    total_gross NUMERIC(12, 2) DEFAULT 0,
    total_deductions NUMERIC(12, 2) DEFAULT 0,
    total_net NUMERIC(12, 2) DEFAULT 0,
    calculation_started_at TIMESTAMP,
    calculation_completed_at TIMESTAMP,
    approved_by INTEGER,
    approved_at TIMESTAMP,
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 6. Payslips
CREATE TABLE payslips (
    id SERIAL PRIMARY KEY,
    payroll_run_id INTEGER REFERENCES payroll_runs(id),
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    pay_period_start DATE NOT NULL,
    pay_period_end DATE NOT NULL,
    basic_salary NUMERIC(10, 2) NOT NULL,
    allowances JSONB,
    overtime_pay NUMERIC(10, 2) DEFAULT 0,
    total_earnings NUMERIC(10, 2) NOT NULL,
    gosi_deduction NUMERIC(10, 2) DEFAULT 0,
    absence_deduction NUMERIC(10, 2) DEFAULT 0,
    loan_deduction NUMERIC(10, 2) DEFAULT 0,
    total_deductions NUMERIC(10, 2) NOT NULL,
    net_salary NUMERIC(10, 2) NOT NULL,
    working_days INTEGER,
    present_days INTEGER,
    absent_days INTEGER,
    overtime_hours NUMERIC(5, 2),
    pdf_url TEXT,
    status VARCHAR(20) DEFAULT 'draft',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert Sample Data
INSERT INTO departments (department_name_ar, department_name_en, department_code) VALUES
('الموارد البشرية', 'Human Resources', 'HR'),
('تقنية المعلومات', 'Information Technology', 'IT'),
('المالية', 'Finance', 'FIN'),
('التسويق', 'Marketing', 'MKT');

INSERT INTO job_titles (title_ar, title_en, job_code, job_level, min_salary, max_salary) VALUES
('مدير', 'Manager', 'MGR', 5, 15000, 25000),
('مطور برمجيات', 'Software Developer', 'DEV', 3, 8000, 15000),
('محاسب', 'Accountant', 'ACC', 3, 7000, 12000),
('موظف إداري', 'Administrative Staff', 'ADM', 2, 5000, 8000);
```

---

## 🎯 Cursor AI Prompts to Get Started

### Prompt 1: Setup Backend Structure
```
Create a FastAPI backend for a payroll management system with:
1. SQLAlchemy models for Employee, Department, JobTitle, SalaryComponent, PayrollRun, Payslip
2. Pydantic schemas for validation
3. CRUD routers for employees with search and filters
4. JWT authentication with role-based access control
5. Database configuration with PostgreSQL
6. Alembic migrations setup

Use the provided database schema and follow best practices for FastAPI development.
```

### Prompt 2: Payroll Engine
```
Create a PayrollEngine service class that:
1. Calculates gross salary (basic + allowances + overtime)
2. Calculates deductions (GOSI 10% for Saudis, 2% for non-Saudis, absence, loans)
3. Handles payroll run for multiple employees
4. Uses Celery for background processing
5. Generates payslip records

Include error handling, logging, and proper decimal precision for financial calculations.
```

### Prompt 3: Frontend Employee Management
```
Create a Next.js employee management page with:
1. DataTable component using @tanstack/react-table
2. Search and filters (name, department, status)
3. Add/Edit employee modal with multi-step form
4. Zod validation
5. React Query for data fetching
6. Responsive design with TailwindCSS and shadcn/ui

Include proper TypeScript types and error handling.
```

### Prompt 4: Authentication System
```
Implement complete authentication system:
1. Login page with email/password
2. JWT token management
3. Protected routes middleware
4. Zustand store for auth state
5. Password reset flow
6. Role-based UI rendering

Use FastAPI-Users on backend and secure token storage on frontend.
```

---

## 🔍 Testing Commands

```bash
# Backend Tests
cd backend
pytest
pytest --cov=app tests/
pytest -v -s tests/test_payroll.py

# Frontend Tests
cd frontend
npm run test
npm run test:coverage

# Linting
# Backend
ruff check .
mypy app/

# Frontend
npm run lint
```

---

## 📈 Monitoring & Logging

### Sentry Setup
```python
# In app/main.py
import sentry_sdk
from sentry_sdk.integrations.fastapi import FastApiIntegration

sentry_sdk.init(
    dsn="your-sentry-dsn",
    integrations=[FastApiIntegration()],
    traces_sample_rate=1.0,
    environment="production"
)
```

### Structured Logging
```python
import logging
from pythonjsonlogger import jsonlogger

logger = logging.getLogger()
logHandler = logging.StreamHandler()
formatter = jsonlogger.JsonFormatter()
logHandler.setFormatter(formatter)
logger.addHandler(logHandler)
logger.setLevel(logging.INFO)
```

---

## 🚢 Deployment Checklist

- [ ] Update all environment variables in production
- [ ] Change SECRET_KEY and JWT_SECRET_KEY
- [ ] Set DEBUG=False
- [ ] Configure CORS for production domains
- [ ] Setup SSL/TLS certificates
- [ ] Configure database backups
- [ ] Setup Sentry error monitoring
- [ ] Configure rate limiting
- [ ] Setup CDN for static assets
- [ ] Test all API endpoints
- [ ] Run security audit
- [ ] Load testing with 100+ concurrent users
- [ ] Documentation review
- [ ] User acceptance testing (UAT)

---

## 📞 Support & Resources

**Developer:** Dyaia Hameed  
**GitHub:** [Your GitHub Profile]  
**Documentation:** See `docs/` folder  
**API Docs:** http://localhost:8000/docs (when running)

**Tech Stack:**
- Backend: Python 3.11 + FastAPI + PostgreSQL + Redis + Celery
- Frontend: Next.js 14 + TypeScript + TailwindCSS + shadcn/ui
- Mobile: Flutter 3.16 + Dart 3
- DevOps: Docker + GitHub Actions + Railway/Vercel

---

**Ready to start developing in Cursor AI! 🚀**

Use the prompts above and the complete structure to build the payroll system step by step.