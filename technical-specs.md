# 📋 وثيقة المتطلبات الفنية - نظام إدارة الرواتب
## Technical Requirements Document (TRD)

**إصدار:** 1.0  
**تاريخ:** مارس 2026  
**إعداد:** Dyaia Hameed  
**الحالة:** مسودة نهائية

---

## 📌 نظرة عامة على المشروع

### الغرض من المشروع
نظام متكامل لإدارة الرواتب والموارد البشرية يهدف إلى:
- **أتمتة كاملة** لحساب الرواتب والاستحقاقات
- **دقة 100%** في الحسابات المالية
- **توافق كامل** مع نظام GOSI ومتطلبات WPS في السعودية
- **واجهة سهلة الاستخدام** بالعربية والإنجليزية
- **قابلية توسع** لآلاف الموظفين

### الأهداف الرئيسية
1. ✅ تقليل وقت معالجة الرواتب من 5 أيام إلى ساعات
2. ✅ القضاء على الأخطاء اليدوية في الحسابات
3. ✅ توفير تقارير فورية للإدارة
4. ✅ تسهيل التكامل مع Zoho CRM
5. ✅ دعم تطبيق موبايل للموظفين

---

## 🏗️ البنية المعمارية (Architecture)

### 1. المعمارية الشاملة

```
┌─────────────────────────────────────────────────────┐
│                   Frontend Layer                     │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │
│  │   Web App    │  │  Mobile App  │  │   Admin   │ │
│  │  (Next.js)   │  │  (Flutter)   │  │   Panel   │ │
│  └──────────────┘  └──────────────┘  └───────────┘ │
└─────────────────────────────────────────────────────┘
                          ↕ HTTPS/REST
┌─────────────────────────────────────────────────────┐
│                    API Gateway                       │
│              (FastAPI + Rate Limiting)               │
└─────────────────────────────────────────────────────┘
                          ↕
┌─────────────────────────────────────────────────────┐
│                  Backend Services                    │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │
│  │   Employee   │  │   Payroll    │  │   Auth    │ │
│  │   Service    │  │   Engine     │  │  Service  │ │
│  └──────────────┘  └──────────────┘  └───────────┘ │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │
│  │  Attendance  │  │   Reports    │  │   Zoho    │ │
│  │   Service    │  │   Service    │  │Integration│ │
│  └──────────────┘  └──────────────┘  └───────────┘ │
└─────────────────────────────────────────────────────┘
                          ↕
┌─────────────────────────────────────────────────────┐
│                   Data Layer                         │
│  ┌──────────────┐  ┌──────────────┐  ┌───────────┐ │
│  │  PostgreSQL  │  │    Redis     │  │    S3     │ │
│  │  (Primary)   │  │   (Cache)    │  │ (Storage) │ │
│  └──────────────┘  └──────────────┘  └───────────┘ │
└─────────────────────────────────────────────────────┘
                          ↕
┌─────────────────────────────────────────────────────┐
│              External Services                       │
│  [Zoho CRM] [SendGrid] [Twilio] [GOSI API] [WPS]   │
└─────────────────────────────────────────────────────┘
```

### 2. Technology Stack

#### Backend
```yaml
Framework: FastAPI 0.110+
Language: Python 3.11+
ORM: SQLAlchemy 2.0+
Database: PostgreSQL 15+
Cache: Redis 7+
Task Queue: Celery + Redis
API Documentation: OpenAPI 3.0 (Swagger)
Testing: pytest, pytest-cov
Code Quality: ruff, mypy, black
```

#### Frontend (Web)
```yaml
Framework: Next.js 14+ (App Router)
Language: TypeScript 5+
UI Library: React 18+
Styling: TailwindCSS 3+ + shadcn/ui
State Management: Zustand + React Query
Forms: react-hook-form + zod
Charts: Recharts
Date Handling: date-fns
HTTP Client: axios
```

#### Mobile
```yaml
Framework: Flutter 3.16+
Language: Dart 3+
State Management: Riverpod
HTTP Client: dio
Local Storage: Hive
Authentication: flutter_secure_storage
```

#### DevOps & Infrastructure
```yaml
Hosting: Railway (Backend) + Vercel (Frontend)
Database Hosting: Supabase / Railway
CDN: Cloudflare
Container: Docker + Docker Compose
CI/CD: GitHub Actions
Monitoring: Sentry + Uptime Robot
Logging: Grafana Loki
Backup: Automated daily backups
```

---

## 🗃️ قاعدة البيانات (Database Schema)

### ERD Overview

```
┌─────────────┐       ┌──────────────┐       ┌─────────────┐
│ Departments │◄──────┤  Employees   │──────►│  Job Titles │
└─────────────┘       └──────────────┘       └─────────────┘
                            │ │
                    ┌───────┘ └───────┐
                    │                 │
            ┌───────▼──────┐  ┌──────▼──────────┐
            │ Salary       │  │   Attendance    │
            │ Structure    │  │   Records       │
            └───────┬──────┘  └─────────────────┘
                    │
            ┌───────▼──────────┐
            │ Salary           │
            │ Components       │
            └──────────────────┘
                    │
            ┌───────▼──────────┐
            │ Payroll          │
            │ Runs             │
            └───────┬──────────┘
                    │
            ┌───────▼──────────┐
            │ Payslips         │
            └──────────────────┘
```

### المواصفات التقنية للجداول

#### 1. employees (الموظفين)
```sql
CREATE TABLE employees (
    employee_id VARCHAR(20) PRIMARY KEY,
    full_name_ar VARCHAR(100) NOT NULL,
    full_name_en VARCHAR(100),
    nationality VARCHAR(50),
    national_id VARCHAR(20) UNIQUE,
    iqama_number VARCHAR(20),
    passport_number VARCHAR(20),
    date_of_birth DATE,
    gender VARCHAR(10) CHECK (gender IN ('male', 'female')),
    marital_status VARCHAR(20),
    
    -- Employment Info
    hire_date DATE NOT NULL,
    contract_type VARCHAR(20) CHECK (contract_type IN ('permanent', 'temporary', 'contract', 'part_time')),
    department_id INTEGER REFERENCES departments(id),
    job_title_id INTEGER REFERENCES job_titles(id),
    manager_id VARCHAR(20) REFERENCES employees(employee_id),
    work_location VARCHAR(100),
    employment_status VARCHAR(20) CHECK (employment_status IN ('active', 'inactive', 'terminated', 'on_leave')) DEFAULT 'active',
    
    -- Salary Info
    base_salary NUMERIC(10, 2) NOT NULL CHECK (base_salary >= 0),
    payment_method VARCHAR(20) CHECK (payment_method IN ('bank_transfer', 'cash', 'cheque')),
    
    -- Bank Details (Encrypted)
    bank_name VARCHAR(100),
    bank_account_number TEXT, -- Encrypted
    iban VARCHAR(34),
    
    -- Contact
    phone_number VARCHAR(20),
    emergency_contact VARCHAR(20),
    email VARCHAR(100) UNIQUE,
    address TEXT,
    
    -- GOSI
    gosi_number VARCHAR(20),
    gosi_subscription_date DATE,
    
    -- Metadata
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_by INTEGER REFERENCES users(id),
    updated_by INTEGER REFERENCES users(id),
    
    -- Indexes
    INDEX idx_employee_status (employment_status),
    INDEX idx_employee_dept (department_id),
    INDEX idx_employee_hire_date (hire_date)
);
```

**قيود خاصة:**
- `national_id` يجب أن يكون فريداً للسعوديين
- `iqama_number` يجب أن يكون فريداً لغير السعوديين
- `bank_account_number` مشفر باستخدام AES-256
- `email` يجب أن يكون فريداً

#### 2. departments (الأقسام)
```sql
CREATE TABLE departments (
    id SERIAL PRIMARY KEY,
    department_name_ar VARCHAR(100) NOT NULL,
    department_name_en VARCHAR(100),
    department_code VARCHAR(20) UNIQUE,
    manager_id VARCHAR(20) REFERENCES employees(employee_id),
    parent_department_id INTEGER REFERENCES departments(id),
    cost_center VARCHAR(50),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_dept_code (department_code),
    INDEX idx_dept_active (is_active)
);
```

#### 3. job_titles (المسميات الوظيفية)
```sql
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
    
    INDEX idx_job_code (job_code),
    CHECK (max_salary >= min_salary)
);
```

#### 4. salary_components (مكونات الراتب)
```sql
CREATE TABLE salary_components (
    id SERIAL PRIMARY KEY,
    component_name_ar VARCHAR(100) NOT NULL,
    component_name_en VARCHAR(100),
    component_code VARCHAR(20) UNIQUE NOT NULL,
    component_type VARCHAR(20) CHECK (component_type IN ('earning', 'deduction')) NOT NULL,
    calculation_type VARCHAR(20) CHECK (calculation_type IN ('fixed', 'percentage', 'formula')) NOT NULL,
    default_amount NUMERIC(10, 2),
    default_percentage NUMERIC(5, 2),
    formula TEXT,
    is_taxable BOOLEAN DEFAULT FALSE,
    affects_gosi BOOLEAN DEFAULT FALSE,
    is_recurring BOOLEAN DEFAULT TRUE,
    display_order INTEGER,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_component_type (component_type),
    INDEX idx_component_active (is_active)
);
```

**أمثلة على المكونات:**
```sql
-- Earnings
INSERT INTO salary_components VALUES
(1, 'الراتب الأساسي', 'Basic Salary', 'BASIC_SAL', 'earning', 'fixed', NULL, NULL, NULL, TRUE, TRUE, TRUE, 1, TRUE),
(2, 'بدل السكن', 'Housing Allowance', 'HOUSE_ALL', 'earning', 'percentage', NULL, 25, NULL, TRUE, TRUE, TRUE, 2, TRUE),
(3, 'بدل النقل', 'Transportation Allowance', 'TRANS_ALL', 'earning', 'fixed', 500, NULL, NULL, TRUE, TRUE, TRUE, 3, TRUE),
(4, 'العمل الإضافي', 'Overtime', 'OVERTIME', 'earning', 'formula', NULL, NULL, 'hourly_rate * hours * 1.5', TRUE, FALSE, FALSE, 10, TRUE);

-- Deductions
INSERT INTO salary_components VALUES
(10, 'التأمينات الاجتماعية', 'GOSI Deduction', 'GOSI_DED', 'deduction', 'percentage', NULL, 10, NULL, FALSE, TRUE, TRUE, 1, TRUE),
(11, 'الغياب', 'Absence Deduction', 'ABSENCE', 'deduction', 'formula', NULL, NULL, 'daily_rate * absence_days', FALSE, FALSE, FALSE, 2, TRUE),
(12, 'سلفة', 'Loan Deduction', 'LOAN', 'deduction', 'fixed', NULL, NULL, NULL, FALSE, FALSE, TRUE, 3, TRUE);
```

#### 5. employee_salary_structure (هيكل راتب الموظف)
```sql
CREATE TABLE employee_salary_structure (
    id SERIAL PRIMARY KEY,
    employee_id VARCHAR(20) REFERENCES employees(employee_id) ON DELETE CASCADE,
    component_id INTEGER REFERENCES salary_components(id),
    amount NUMERIC(10, 2),
    percentage NUMERIC(5, 2),
    effective_from DATE NOT NULL,
    effective_to DATE,
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_by INTEGER REFERENCES users(id),
    
    INDEX idx_emp_salary_struct (employee_id, effective_from),
    INDEX idx_component (component_id),
    CHECK (effective_to IS NULL OR effective_to > effective_from),
    UNIQUE (employee_id, component_id, effective_from)
);
```

#### 6. attendance_records (سجلات الحضور)
```sql
CREATE TABLE attendance_records (
    id SERIAL PRIMARY KEY,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    date DATE NOT NULL,
    check_in_time TIMESTAMP,
    check_out_time TIMESTAMP,
    work_hours NUMERIC(4, 2),
    overtime_hours NUMERIC(4, 2) DEFAULT 0,
    status VARCHAR(20) CHECK (status IN ('present', 'absent', 'late', 'on_leave', 'weekend', 'holiday')),
    late_minutes INTEGER DEFAULT 0,
    early_leave_minutes INTEGER DEFAULT 0,
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_attendance_emp_date (employee_id, date),
    INDEX idx_attendance_date (date),
    UNIQUE (employee_id, date)
);
```

#### 7. leave_requests (طلبات الإجازات)
```sql
CREATE TABLE leave_requests (
    id SERIAL PRIMARY KEY,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    leave_type VARCHAR(30) CHECK (leave_type IN ('annual', 'sick', 'unpaid', 'maternity', 'paternity', 'emergency')),
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    total_days INTEGER NOT NULL,
    reason TEXT,
    status VARCHAR(20) CHECK (status IN ('pending', 'approved', 'rejected', 'cancelled')) DEFAULT 'pending',
    approved_by INTEGER REFERENCES users(id),
    approved_at TIMESTAMP,
    rejection_reason TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_leave_emp (employee_id),
    INDEX idx_leave_status (status),
    INDEX idx_leave_dates (start_date, end_date),
    CHECK (end_date >= start_date),
    CHECK (total_days > 0)
);
```

#### 8. loans (السلف والقروض)
```sql
CREATE TABLE loans (
    id SERIAL PRIMARY KEY,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    loan_type VARCHAR(20) CHECK (loan_type IN ('advance', 'loan', 'emergency')),
    loan_amount NUMERIC(10, 2) NOT NULL CHECK (loan_amount > 0),
    remaining_amount NUMERIC(10, 2) NOT NULL,
    monthly_deduction NUMERIC(10, 2) NOT NULL CHECK (monthly_deduction > 0),
    start_date DATE NOT NULL,
    total_installments INTEGER NOT NULL CHECK (total_installments > 0),
    paid_installments INTEGER DEFAULT 0,
    status VARCHAR(20) CHECK (status IN ('active', 'completed', 'cancelled')) DEFAULT 'active',
    notes TEXT,
    approved_by INTEGER REFERENCES users(id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_loan_emp (employee_id),
    INDEX idx_loan_status (status)
);
```

#### 9. payroll_runs (مسيرات الرواتب)
```sql
CREATE TABLE payroll_runs (
    id SERIAL PRIMARY KEY,
    run_name VARCHAR(100) NOT NULL,
    pay_period_start DATE NOT NULL,
    pay_period_end DATE NOT NULL,
    payment_date DATE NOT NULL,
    status VARCHAR(20) CHECK (status IN ('draft', 'calculating', 'calculated', 'approved', 'paid', 'cancelled')) DEFAULT 'draft',
    total_employees INTEGER DEFAULT 0,
    total_gross NUMERIC(12, 2) DEFAULT 0,
    total_deductions NUMERIC(12, 2) DEFAULT 0,
    total_net NUMERIC(12, 2) DEFAULT 0,
    calculation_started_at TIMESTAMP,
    calculation_completed_at TIMESTAMP,
    approved_by INTEGER REFERENCES users(id),
    approved_at TIMESTAMP,
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_by INTEGER REFERENCES users(id),
    
    INDEX idx_payroll_status (status),
    INDEX idx_payroll_period (pay_period_start, pay_period_end),
    CHECK (pay_period_end > pay_period_start),
    CHECK (payment_date >= pay_period_end)
);
```

#### 10. payslips (قسائم الرواتب)
```sql
CREATE TABLE payslips (
    id SERIAL PRIMARY KEY,
    payroll_run_id INTEGER REFERENCES payroll_runs(id) ON DELETE CASCADE,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    pay_period_start DATE NOT NULL,
    pay_period_end DATE NOT NULL,
    
    -- Earnings
    basic_salary NUMERIC(10, 2) NOT NULL,
    allowances JSONB, -- {"housing": 2500, "transport": 500}
    overtime_pay NUMERIC(10, 2) DEFAULT 0,
    other_earnings JSONB,
    total_earnings NUMERIC(10, 2) NOT NULL,
    
    -- Deductions
    gosi_deduction NUMERIC(10, 2) DEFAULT 0,
    absence_deduction NUMERIC(10, 2) DEFAULT 0,
    late_deduction NUMERIC(10, 2) DEFAULT 0,
    loan_deduction NUMERIC(10, 2) DEFAULT 0,
    other_deductions JSONB,
    total_deductions NUMERIC(10, 2) NOT NULL,
    
    -- Net
    net_salary NUMERIC(10, 2) NOT NULL,
    
    -- Attendance Summary
    working_days INTEGER,
    present_days INTEGER,
    absent_days INTEGER,
    late_days INTEGER,
    overtime_hours NUMERIC(5, 2),
    
    -- PDF
    pdf_url TEXT,
    pdf_generated_at TIMESTAMP,
    
    -- Status
    status VARCHAR(20) CHECK (status IN ('draft', 'finalized', 'sent', 'paid')) DEFAULT 'draft',
    sent_at TIMESTAMP,
    paid_at TIMESTAMP,
    
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_payslip_run (payroll_run_id),
    INDEX idx_payslip_emp (employee_id),
    INDEX idx_payslip_period (pay_period_start, pay_period_end),
    UNIQUE (payroll_run_id, employee_id)
);
```

#### 11. users (مستخدمو النظام)
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash TEXT NOT NULL,
    full_name VARCHAR(100) NOT NULL,
    role VARCHAR(20) CHECK (role IN ('admin', 'hr_manager', 'accountant', 'employee', 'viewer')) NOT NULL,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    is_active BOOLEAN DEFAULT TRUE,
    last_login TIMESTAMP,
    failed_login_attempts INTEGER DEFAULT 0,
    locked_until TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_user_email (email),
    INDEX idx_user_role (role),
    INDEX idx_user_active (is_active)
);
```

#### 12. audit_logs (سجل المراجعة)
```sql
CREATE TABLE audit_logs (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    action VARCHAR(50) NOT NULL,
    entity_type VARCHAR(50) NOT NULL,
    entity_id VARCHAR(100),
    old_values JSONB,
    new_values JSONB,
    ip_address VARCHAR(45),
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    INDEX idx_audit_user (user_id),
    INDEX idx_audit_entity (entity_type, entity_id),
    INDEX idx_audit_date (created_at)
);
```

---

## 🔐 الأمان والصلاحيات (Security & Permissions)

### 1. Authentication & Authorization

#### JWT Token Structure
```json
{
  "sub": "user_id_123",
  "email": "dyaia@example.com",
  "role": "hr_manager",
  "employee_id": "EMP001",
  "permissions": ["read:employees", "write:payroll", "approve:leave"],
  "exp": 1678901234,
  "iat": 1678897634
}
```

#### Password Policy
```yaml
Minimum Length: 12 characters
Requirements:
  - At least 1 uppercase letter
  - At least 1 lowercase letter
  - At least 1 number
  - At least 1 special character
  - No common passwords (check against dictionary)
Expiry: 90 days (optional)
History: Cannot reuse last 5 passwords
Max Login Attempts: 5 (lock account for 30 minutes)
```

### 2. Role-Based Access Control (RBAC)

```yaml
Roles:
  admin:
    permissions:
      - "*:*"  # Full access
    description: "Super user with unrestricted access"
  
  hr_manager:
    permissions:
      - "read:employees"
      - "write:employees"
      - "delete:employees"
      - "read:payroll"
      - "write:payroll"
      - "approve:payroll"
      - "read:reports"
      - "approve:leave"
    description: "HR department manager"
  
  accountant:
    permissions:
      - "read:employees"
      - "read:payroll"
      - "write:payroll"
      - "approve:payroll"
      - "read:reports"
      - "export:reports"
    description: "Accounting department"
  
  manager:
    permissions:
      - "read:employees:department"  # Only their department
      - "approve:leave:department"
      - "read:attendance:department"
      - "read:reports:department"
    description: "Department manager"
  
  employee:
    permissions:
      - "read:profile:own"
      - "update:profile:own:limited"
      - "read:payslip:own"
      - "request:leave"
      - "read:attendance:own"
    description: "Regular employee (self-service)"
  
  viewer:
    permissions:
      - "read:reports"
      - "read:dashboard"
    description: "Read-only access for executives"
```

### 3. Data Encryption

```yaml
At Rest:
  Database: AES-256 encryption at rest (PostgreSQL)
  Sensitive Fields:
    - bank_account_number: AES-256-GCM
    - national_id: AES-256-GCM
    - iqama_number: AES-256-GCM
  Key Management: AWS KMS / HashiCorp Vault
  
In Transit:
  Protocol: TLS 1.3
  Certificate: Let's Encrypt (auto-renewal)
  HSTS: Enabled (max-age=31536000)
  
Application Level:
  Passwords: bcrypt (cost factor 12)
  API Keys: Stored in environment variables
  Secrets: Never committed to Git
```

### 4. Security Headers

```yaml
Headers:
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  X-XSS-Protection: "1; mode=block"
  Strict-Transport-Security: "max-age=31536000; includeSubDomains"
  Content-Security-Policy: "default-src 'self'; script-src 'self' 'unsafe-inline'"
  Referrer-Policy: "strict-origin-when-cross-origin"
```

---

## 📊 المتطلبات الوظيفية (Functional Requirements)

### FR1: إدارة الموظفين

#### FR1.1: إضافة موظف جديد
```yaml
المدخلات:
  - البيانات الشخصية (الاسم، الجنسية، رقم الهوية)
  - بيانات التوظيف (تاريخ التعيين، القسم، المسمى)
  - بيانات الراتب (الراتب الأساسي، المكونات)
  - الحساب البنكي (IBAN)
  - بيانات التواصل (هاتف، بريد)

المخرجات:
  - رقم موظف فريد (EMP00001)
  - تأكيد الإنشاء
  - إرسال بريد ترحيبي

قواعد العمل:
  - رقم الهوية يجب أن يكون فريداً
  - الراتب الأساسي > 0
  - IBAN يجب أن يكون صحيحاً (SA format)
  - تاريخ التعيين لا يمكن أن يكون في المستقبل
  - البريد الإلكتروني فريد
```

#### FR1.2: تعديل بيانات موظف
```yaml
السيناريو:
  1. HR يبحث عن الموظف
  2. يعدل البيانات المطلوبة
  3. النظام يحفظ السجل القديم في audit_logs
  4. يظهر رسالة تأكيد

القيود:
  - لا يمكن تعديل رقم الموظف
  - تعديل الراتب يتطلب صلاحية خاصة
  - تسجيل كل التعديلات في audit_logs
```

#### FR1.3: البحث والفلترة
```yaml
معايير البحث:
  - النص: (اسم، رقم الموظف، رقم الهوية)
  - القسم
  - المسمى الوظيفي
  - الحالة (نشط، غير نشط، مفصول)
  - الجنسية
  - نطاق الراتب (min, max)
  - تاريخ التعيين (from, to)

المخرجات:
  - جدول قابل للترتيب
  - Pagination (50 نتيجة/صفحة)
  - Export إلى Excel/CSV
```

### FR2: حساب الرواتب

#### FR2.1: إنشاء مسير رواتب
```yaml
المدخلات:
  - اسم المسير ("رواتب يناير 2026")
  - فترة الدفع (start_date, end_date)
  - تاريخ الدفع (payment_date)
  - اختيار الموظفين (الكل أو قائمة محددة)

العملية:
  1. إنشاء payroll_run بحالة "draft"
  2. جلب كل الموظفين النشطين
  3. حساب كل موظف (background task):
     a. الراتب الأساسي
     b. الاستحقاقات (بدل سكن، نقل، ...)
     c. العمل الإضافي (من attendance)
     d. الاستقطاعات (GOSI، غياب، قروض، ...)
     e. الصافي = الإجمالي - الاستقطاعات
  4. إنشاء payslip لكل موظف
  5. تحديث إحصائيات payroll_run
  6. تغيير الحالة إلى "calculated"

المخرجات:
  - payroll_run مع الإحصائيات
  - payslips لكل موظف
  - إشعار للـ HR عند الانتهاء
```

#### FR2.2: قواعد حساب الراتب

**الراتب الإجمالي (Gross Salary):**
```python
gross_salary = (
    base_salary +
    housing_allowance +  # عادة 25% من الأساسي
    transport_allowance +  # مبلغ ثابت
    other_allowances +
    overtime_pay  # (hourly_rate * overtime_hours * 1.5)
)
```

**الاستقطاعات:**

1. **GOSI (التأمينات الاجتماعية):**
```python
# للسعوديين
employee_gosi = (base_salary + housing_allowance) * 0.10  # 10%
employer_gosi = (base_salary + housing_allowance) * 0.12  # 12%

# لغير السعوديين
employee_gosi = (base_salary + housing_allowance) * 0.02  # 2%
employer_gosi = 0  # لا يوجد
```

2. **خصم الغياب:**
```python
daily_rate = base_salary / 30
absence_deduction = daily_rate * absence_days
```

3. **خصم التأخير:**
```python
# القاعدة: التأخير 3 مرات = يوم غياب
if late_count >= 3:
    late_deduction = daily_rate * (late_count // 3)
```

4. **خصم السلفة:**
```python
# من جدول loans
loan_deduction = loan.monthly_deduction
remaining_amount -= loan_deduction
```

**الراتب الصافي:**
```python
net_salary = gross_salary - total_deductions
```

#### FR2.3: اعتماد ودفع الرواتب
```yaml
خطوات الاعتماد:
  1. HR/Accountant يراجع payroll_run
  2. يتحقق من الأرقام والإحصائيات
  3. يوافق على المسير → status = "approved"
  4. النظام يولد:
     - PDF لكل payslip
     - WPS file (ملف البنوك)
     - GOSI report
  5. إرسال قسائم الرواتب للموظفين عبر:
     - Email (PDF مرفق)
     - WhatsApp (notification)
     - Mobile app notification
  6. تحديث الحالة → "paid"
```

### FR3: الحضور والغياب

#### FR3.1: تسجيل الحضور
```yaml
طرق التسجيل:
  1. بصمة/وجه (Integration مع جهاز البصمة)
  2. تطبيق الموبايل (GPS verification)
  3. يدوياً (من HR)

قواعد العمل:
  - Check-in: قبل 9:00 صباحاً
  - Check-out: بعد 5:00 مساءً
  - Late: بعد 9:15 صباحاً (15 دقيقة grace period)
  - Early leave: قبل 4:45 مساءً
  - Overtime: بعد 5:00 مساءً (بموافقة المدير)

الحساب:
  work_hours = check_out_time - check_in_time - break_time(1 hour)
  late_minutes = max(0, check_in_time - 9:00)
  overtime_hours = max(0, check_out_time - 17:00)
```

#### FR3.2: طلب إجازة
```yaml
أنواع الإجازات:
  - سنوية (Annual): 21 يوم/سنة
  - مرضية (Sick): مدفوعة حسب نظام العمل
  - طارئة (Emergency): 5 أيام/سنة
  - أمومة (Maternity): 10 أسابيع
  - أبوة (Paternity): 3 أيام

سير العمل:
  1. Employee يقدم طلب إجازة
  2. Manager يراجع ويوافق/يرفض
  3. HR يتلقى إشعار
  4. تحديث رصيد الإجازات
  5. إشعار الموظف بالقرار

القيود:
  - لا يمكن طلب إجازة بأثر رجعي
  - رصيد الإجازة السنوية يكفي
  - عدم وجود إجازة متداخلة
```

### FR4: التقارير

#### FR4.1: تقارير الرواتب
```yaml
1. ملخص مسير الرواتب:
   - إجمالي الموظفين
   - إجمالي الاستحقاقات
   - إجمالي الاستقطاعات
   - الصافي
   - التوزيع حسب القسم

2. تقرير حسب القسم:
   - تكلفة كل قسم
   - عدد الموظفين
   - متوسط الراتب

3. تقرير التأمينات (GOSI):
   - قائمة الموظفين
   - نسبة خصم الموظف
   - نسبة مساهمة الشركة
   - الإجمالي

4. تقرير WPS:
   - ملف Excel بتنسيق البنك
   - يحتوي: (رقم الهوية، IBAN، المبلغ، الشهر)

Export Options: PDF, Excel, CSV
```

#### FR4.2: تقارير الحضور
```yaml
1. تقرير الحضور الشهري:
   - أيام العمل
   - أيام الحضور
   - أيام الغياب
   - أيام التأخير
   - ساعات العمل الإضافي

2. تقرير التأخير:
   - الموظفين المتأخرين
   - عدد مرات التأخير
   - إجمالي دقائق التأخير

3. تقرير الإجازات:
   - الإجازات المقدمة
   - الإجازات الموافق عليها
   - رصيد الإجازات

Filters: موظف، قسم، تاريخ من/إلى
```

---

## 🔗 التكاملات (Integrations)

### INT1: Zoho CRM Integration

#### الغرض
مزامنة بيانات الموظفين بين نظام الرواتب و Zoho CRM

#### التكامل
```yaml
Direction: Bi-directional (two-way sync)

من Payroll إلى Zoho:
  - بيانات الموظف الأساسية
  - تحديثات الراتب
  - تغييرات الحالة الوظيفية

من Zoho إلى Payroll:
  - موظفين جدد
  - تحديثات البيانات الشخصية

Sync Frequency: Real-time (webhook) + Daily full sync

Authentication: OAuth 2.0
```

#### API Endpoints
```python
# Zoho → Payroll (Webhook)
POST /api/webhooks/zoho/employee-created
POST /api/webhooks/zoho/employee-updated

# Payroll → Zoho (REST API)
POST https://www.zohoapis.com/crm/v3/Employees
PUT https://www.zohoapis.com/crm/v3/Employees/{id}
```

#### Field Mapping
```yaml
Zoho Field          →  Payroll Field
-----------------      ----------------
Full_Name           →  full_name_ar
Employee_ID         →  employee_id
Email               →  email
Phone               →  phone_number
Department          →  department_id (lookup)
Designation         →  job_title_id (lookup)
Joining_Date        →  hire_date
Basic_Salary        →  base_salary
```

#### Error Handling
```yaml
Sync Failures:
  - Log in sync_errors table
  - Retry 3 times (exponential backoff)
  - Alert admin if still failing
  - Manual resolution UI in admin panel

Conflict Resolution:
  - Latest timestamp wins
  - Zoho is source of truth for personal data
  - Payroll is source of truth for salary data
```

### INT2: Email Service (SendGrid)

```yaml
Purpose: Transactional emails

Use Cases:
  - Welcome emails for new employees
  - Payslip delivery
  - Leave request notifications
  - Password reset emails
  - System alerts

Configuration:
  API Key: Stored in environment variable
  From Email: noreply@company.com
  From Name: "شركة XYZ - نظام الرواتب"
  
Templates:
  - payslip_email.html
  - welcome_email.html
  - leave_approval.html
  - password_reset.html

Rate Limits: 100 emails/second
```

### INT3: WhatsApp Business API

```yaml
Purpose: Instant notifications

Use Cases:
  - Payslip ready notification
  - Leave request status
  - Attendance alerts
  - System announcements

Provider: Twilio / MessageBird
Format: Text + PDF link

Message Template:
  "مرحباً {name}،
   قسيمة راتبك لشهر {month} جاهزة.
   يمكنك الاطلاع عليها من خلال الرابط:
   https://payroll.company.com/payslip/{id}"

Rate Limits: 60 messages/second
```

### INT4: WPS (Wage Protection System)

```yaml
Purpose: Comply with Saudi labor law

Format: SIF (Standard Import Format) Excel file

File Structure:
  Columns:
    - Employer ID (10 digits)
    - Employee ID (10 digits)
    - Bank Code (2 digits)
    - Employee Bank Account (IBAN)
    - Salary Amount
    - Payment Period (YYYYMM)
    - Salary Days
    - Worker Type (1=Saudi, 2=Non-Saudi)

Validation:
  - All IBANs start with "SA"
  - Total amount matches payroll
  - No duplicate employees
  - All required fields present

Upload: Through GOSI/MOL portal
Frequency: Monthly (before payment date)
```

---

## 📱 متطلبات تطبيق الموبايل

### Mobile Features

#### For Employees:
```yaml
1. Dashboard:
   - Next pay date
   - Last payslip summary
   - Leave balance
   - Attendance this month

2. Payslips:
   - View all payslips
   - Download PDF
   - Share via email/WhatsApp

3. Attendance:
   - Check-in / Check-out (with GPS)
   - View attendance history
   - Overtime hours

4. Leave Requests:
   - Request new leave
   - View pending requests
   - Check approval status
   - Leave balance

5. Profile:
   - View personal information
   - Update contact details
   - Change password

6. Notifications:
   - Payslip ready
   - Leave approved/rejected
   - System announcements
```

#### For Managers:
```yaml
1. Team Overview:
   - Team members list
   - Attendance summary
   - Leave requests pending approval

2. Leave Approval:
   - Approve/reject leave requests
   - View team calendar
   - Check leave balances

3. Reports:
   - Team attendance
   - Team payroll summary
```

#### Technical Requirements:
```yaml
Platform: Flutter (iOS + Android)
Minimum OS: iOS 13+ / Android 8+
Offline Mode: View last payslip, personal data
Authentication: Biometric (fingerprint/face ID)
Push Notifications: Firebase Cloud Messaging
Storage: Hive (local database)
API: REST API with JWT tokens
```

---

## 🚀 متطلبات الأداء (Performance Requirements)

### Response Time
```yaml
API Endpoints:
  - GET requests: < 200ms (p95)
  - POST requests: < 500ms (p95)
  - Payroll calculation (100 employees): < 30 seconds
  - Report generation: < 5 seconds
  - PDF generation: < 2 seconds per payslip

Database Queries:
  - Simple queries: < 50ms
  - Complex queries with JOINs: < 200ms
  - Full-text search: < 100ms

Page Load:
  - First Contentful Paint (FCP): < 1.5s
  - Time to Interactive (TTI): < 3.5s
  - Largest Contentful Paint (LCP): < 2.5s
```

### Scalability
```yaml
Current Target:
  - 1,000 employees
  - 100 concurrent users
  - 1,000 API requests/minute

Future (2 years):
  - 10,000 employees
  - 500 concurrent users
  - 5,000 API requests/minute

Database:
  - Indexed queries
  - Connection pooling (max 20 connections)
  - Read replicas for reports

Caching Strategy:
  - Redis for session data
  - Cache employee list (1 hour TTL)
  - Cache static data (departments, job titles)
```

### Availability
```yaml
Uptime Target: 99.5% (43.8 hours downtime/year)

Backup Strategy:
  - Daily automated backups (kept for 30 days)
  - Weekly full backups (kept for 1 year)
  - Backup to S3/Cloud Storage
  - Test restore monthly

Disaster Recovery:
  - RTO (Recovery Time Objective): 4 hours
  - RPO (Recovery Point Objective): 24 hours
  - Documented recovery procedures
```

---

## 🧪 متطلبات الاختبار (Testing Requirements)

### Testing Strategy

#### 1. Unit Testing
```yaml
Coverage Target: > 80%
Framework: pytest (backend), Jest (frontend)

Focus Areas:
  - PayrollEngine calculation logic
  - Salary component calculations
  - GOSI calculations
  - Data validation functions
  - Utility functions

Example:
  test_calculate_gosi_saudi_employee()
  test_calculate_overtime_pay()
  test_validate_iban()
```

#### 2. Integration Testing
```yaml
Tools: pytest + TestClient (FastAPI)

Scenarios:
  - Create employee → Calculate payroll → Generate payslip
  - Request leave → Manager approval → Update balance
  - Check-in → Check-out → Calculate work hours
  - Zoho webhook → Employee sync → Database update

Database: Use separate test database
```

#### 3. End-to-End Testing
```yaml
Tools: Playwright / Cypress

User Flows:
  1. Login → Dashboard → View employees
  2. Add new employee → Assign salary → Save
  3. Create payroll run → Calculate → Review → Approve
  4. Employee login → View payslip → Download PDF

Browsers: Chrome, Firefox, Safari
```

#### 4. Performance Testing
```yaml
Tools: Locust / k6

Scenarios:
  - 100 concurrent users browsing
  - Calculate payroll for 1000 employees
  - Generate 1000 PDFs simultaneously
  - Load test API endpoints (500 req/s)

Metrics:
  - Response time (p50, p95, p99)
  - Error rate < 0.1%
  - CPU usage < 70%
  - Memory usage < 80%
```

#### 5. Security Testing
```yaml
Tools: OWASP ZAP, Bandit (Python)

Checks:
  - SQL injection
  - XSS (Cross-Site Scripting)
  - CSRF protection
  - Authentication bypass
  - Sensitive data exposure
  - Insecure dependencies

Frequency: Before each major release
```

---

## 📚 التوثيق (Documentation)

### Required Documentation

#### 1. API Documentation
```yaml
Tool: Swagger/OpenAPI (auto-generated)

Includes:
  - All endpoints with descriptions
  - Request/response schemas
  - Authentication requirements
  - Example requests/responses
  - Error codes and messages

Access: /docs (development), /api-docs (production)
```

#### 2. User Manuals
```yaml
For HR Managers:
  - How to add employee
  - How to run payroll
  - How to generate reports
  - How to approve leaves

For Employees:
  - How to view payslip
  - How to request leave
  - How to use mobile app

For Admins:
  - System configuration
  - User management
  - Backup and restore

Format: PDF + Online help center
Language: Arabic + English
```

#### 3. Technical Documentation
```yaml
Architecture Document:
  - System overview
  - Component diagram
  - Data flow diagram
  - Technology stack

Database Schema:
  - ERD
  - Table descriptions
  - Relationships

Deployment Guide:
  - Infrastructure setup
  - Environment variables
  - Docker commands
  - CI/CD pipeline

API Integration Guide:
  - Zoho integration
  - Email service
  - WhatsApp API
```

---

## 📦 التسليم والنشر (Deployment)

### Deployment Strategy

#### Environments
```yaml
1. Development:
   URL: dev.payroll.company.com
   Purpose: Active development
   Database: dev_payroll_db
   
2. Staging:
   URL: staging.payroll.company.com
   Purpose: QA testing
   Database: staging_payroll_db (copy of prod)
   
3. Production:
   URL: payroll.company.com
   Purpose: Live system
   Database: prod_payroll_db
   Backup: Daily automated
```

#### CI/CD Pipeline (GitHub Actions)
```yaml
On Push to main:
  1. Run linters (ruff, mypy, eslint)
  2. Run unit tests
  3. Run integration tests
  4. Build Docker images
  5. Push to registry
  6. Deploy to staging
  7. Run smoke tests
  8. Await manual approval
  9. Deploy to production
  10. Run health checks
  11. Notify team (Slack/Email)

Rollback:
  - Keep last 3 versions
  - One-click rollback button
  - Automatic rollback if health check fails
```

#### Docker Setup
```dockerfile
# Backend Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/payroll
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis
  
  db:
    image: postgres:15
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=secure_password
  
  redis:
    image: redis:7-alpine
    
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      - NEXT_PUBLIC_API_URL=http://backend:8000

volumes:
  postgres_data:
```

---

## ✅ معايير القبول (Acceptance Criteria)

### Must Have (MVP)
- [ ] إدارة موظفين كاملة (CRUD)
- [ ] حساب رواتب دقيق (Gross, Net, GOSI)
- [ ] توليد قسائم رواتب PDF
- [ ] نظام تسجيل دخول آمن
- [ ] تقرير ملخص الرواتب
- [ ] تقرير GOSI
- [ ] Export إلى Excel
- [ ] واجهة عربية احترافية
- [ ] تطبيق موبايل للموظفين (view payslips)

### Should Have (Phase 2)
- [ ] إدارة حضور وغياب
- [ ] طلبات الإجازات
- [ ] السلف والقروض
- [ ] التكامل مع Zoho
- [ ] إشعارات WhatsApp
- [ ] Dashboard تحليلي
- [ ] Multi-language (AR/EN)

### Could Have (Future)
- [ ] تقييم أداء الموظفين
- [ ] إدارة المستندات
- [ ] التوقيع الإلكتروني
- [ ] AI-powered analytics
- [ ] Chatbot للموظفين

---

## 📞 جهات الاتصال

**Project Owner:** Dyaia Hameed  
**Email:** dyaia.hameed@example.com  
**Role:** Full Stack Developer  

**Technical Stack Expertise:**
- Backend: Python, FastAPI, SQLAlchemy
- Frontend: React, Next.js, TypeScript
- Mobile: Flutter
- Database: PostgreSQL
- DevOps: Docker, GitHub Actions

---

**نهاية الوثيقة**

*آخر تحديث: مارس 10, 2026*