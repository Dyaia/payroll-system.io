# 📅 خطة التنفيذ التفصيلية - نظام إدارة الرواتب
## Detailed Implementation Plan (Week by Week)

---

## نظرة عامة | Overview

**مدة المشروع:** 16 أسبوع (4 أشهر)  
**تاريخ البدء المتوقع:** أبريل 2026  
**تاريخ الانتهاء المتوقع:** يوليو 2026

**الفريق المطلوب:**
- Backend Developer (Python/FastAPI)
- Frontend Developer (React/Next.js)
- Mobile Developer (Flutter)
- UI/UX Designer
- DevOps Engineer
- QA Engineer

---

## 🗓️ الأسبوع 1: الإعداد والتخطيط | Setup & Planning

### الأهداف:
✅ إعداد بيئة التطوير الكاملة  
✅ إنشاء المستودعات (Repositories)  
✅ تصميم قاعدة البيانات  
✅ إعداد الأدوات والخدمات

### المهام اليومية:

**اليوم 1-2: إعداد البيئة التقنية**
- [ ] إنشاء GitHub Organization
- [ ] إنشاء 3 repositories:
  - `payroll-backend` (Python/FastAPI)
  - `payroll-frontend` (Next.js)
  - `payroll-mobile` (Flutter)
- [ ] إعداد GitHub Actions للـ CI/CD
- [ ] إنشاء حسابات:
  - Railway/Vercel للاستضافة
  - Supabase لقاعدة البيانات
  - Sentry للمراقبة
  - Cloudflare للـ CDN

**اليوم 3-4: تصميم قاعدة البيانات**
- [ ] رسم ERD (Entity Relationship Diagram) في draw.io
- [ ] كتابة SQL schema كاملة
- [ ] تحديد العلاقات والقيود (Constraints)
- [ ] تحديد الـ Indexes المطلوبة
- [ ] مراجعة مع الفريق

**اليوم 5: إعداد الأدوات**
- [ ] تثبيت Cursor AI
- [ ] إعداد حسابات API:
  - Anthropic (Claude)
  - Zoho Developer
  - SendGrid (Email)
  - Twilio (SMS)
- [ ] إعداد Figma للتصميم
- [ ] إنشاء Notion/Jira للمهام

### المخرجات:
📦 بيئة تطوير جاهزة  
📦 ERD كامل  
📦 Repositories منظمة  
📦 CI/CD pipeline جاهز

---

## 🗓️ الأسبوع 2: قاعدة البيانات والـ Backend الأساسي

### الأهداف:
✅ إنشاء قاعدة البيانات  
✅ بناء API الأساسي  
✅ نظام المصادقة (Authentication)

### المهام:

**اليوم 1-2: قاعدة البيانات**
```bash
# Backend Setup
cd payroll-backend
python -m venv venv
source venv/bin/activate
pip install fastapi sqlalchemy alembic psycopg2-binary

# إنشاء قاعدة البيانات
alembic init alembic
# نسخ SQL schema
alembic revision --autogenerate -m "initial_schema"
alembic upgrade head
```

**المهام التفصيلية:**
- [ ] إنشاء كل الجداول (14 جدول)
- [ ] إضافة Sample Data للاختبار
- [ ] اختبار العلاقات (Foreign Keys)
- [ ] إنشاء الـ Indexes

**اليوم 3-4: Models & Schemas**
```python
# backend/models/employee.py
from sqlalchemy import Column, String, Date, Decimal
from database import Base.

class Employee(Base):
    __tablename__ = "employees"
    
    employee_id = Column(String(20), primary_key=True)
    full_name_ar = Column(String(100), nullable=False)
    nationality = Column(String(50))
    base_salary = Column(Decimal(10, 2), nullable=False)
    # ... باقي الحقول
```

**المهام:**
- [ ] إنشاء Models لكل الجداول
- [ ] إنشاء Pydantic Schemas للـ validation
- [ ] إنشاء CRUD operations أساسية

**اليوم 5: Authentication System**
```python
# backend/auth.py
from fastapi_users import FastAPIUsers
from fastapi_users.authentication import JWTAuthentication

# إعداد JWT authentication
# إعداد User model
# إعداد login/logout endpoints
```

**المهام:**
- [ ] إعداد FastAPI-Users
- [ ] JWT authentication
- [ ] Password hashing (bcrypt)
- [ ] Login/Logout endpoints
- [ ] Role-based access control (RBAC) أساسي

### المخرجات:
📦 قاعدة بيانات جاهزة  
📦 Models و Schemas كاملة  
📦 نظام تسجيل دخول يعمل  
📦 API documentation (Swagger) أولية

---

## 🗓️ الأسبوع 3: إدارة الموظفين (Backend)

### الأهداف:
✅ APIs كاملة لإدارة الموظفين  
✅ APIs للأقسام والمسميات الوظيفية  
✅ Validation شاملة

### المهام:

**اليوم 1-2: Employee CRUD APIs**
```python
# backend/routers/employees.py
from fastapi import APIRouter, Depends, HTTPException
from typing import List

router = APIRouter(prefix="/api/employees", tags=["employees"])

@router.get("/", response_model=List[EmployeeResponse])
async def get_all_employees(
    skip: int = 0,
    limit: int = 100,
    status: str = None
):
    # جلب كل الموظفين مع filters
    pass

@router.post("/", response_model=EmployeeResponse)
async def create_employee(employee: EmployeeCreate):
    # إنشاء موظف جديد
    # Validation
    # Encryption للحساب البنكي
    pass

@router.get("/{employee_id}")
async def get_employee(employee_id: str):
    pass

@router.put("/{employee_id}")
async def update_employee(employee_id: str, employee: EmployeeUpdate):
    pass

@router.delete("/{employee_id}")
async def delete_employee(employee_id: str):
    # Soft delete (تغيير الحالة فقط)
    pass
```

**المهام:**
- [ ] GET /api/employees (مع filters و pagination)
- [ ] POST /api/employees (إنشاء موظف)
- [ ] GET /api/employees/{id}
- [ ] PUT /api/employees/{id}
- [ ] DELETE /api/employees/{id} (soft delete)
- [ ] Validation كاملة (Pydantic)
- [ ] تشفير الحسابات البنكية

**اليوم 3: Department & Job Title APIs**
- [ ] CRUD للأقسام
- [ ] CRUD للمسميات الوظيفية
- [ ] ربط الموظفين بالأقسام

**اليوم 4: Employee Search & Filters**
```python
@router.get("/search")
async def search_employees(
    q: str = None,  # نص البحث
    department: str = None,
    status: str = None,
    nationality: str = None,
    salary_min: float = None,
    salary_max: float = None
):
    # بحث متقدم
    pass
```

**اليوم 5: Testing**
- [ ] Unit tests لكل endpoint
- [ ] Integration tests
- [ ] Test coverage > 80%

### المخرجات:
📦 15+ API endpoint للموظفين  
📦 Validation شاملة  
📦 Tests جاهزة

---

## 🗓️ الأسبوع 4: مكونات الراتب وهيكل الرواتب

### الأهداف:
✅ إدارة مكونات الراتب  
✅ ربط هيكل الراتب بالموظفين  
✅ APIs مرنة للتكوين

### المهام:

**اليوم 1-2: Salary Components APIs**
```python
# backend/routers/salary_components.py

@router.post("/components")
async def create_salary_component(component: ComponentCreate):
    # إنشاء مكون راتب جديد
    # نوع: استحقاق أو استقطاع
    # نوع الحساب: ثابت أو نسبة أو معادلة
    pass

@router.get("/components")
async def get_all_components(type: str = None):
    # جلب كل المكونات
    # فلترة حسب النوع (earning/deduction)
    pass
```

**المهام:**
- [ ] CRUD لمكونات الراتب
- [ ] دعم 3 أنواع حساب (fixed, percentage, formula)
- [ ] Pre-populate بالمكونات الشائعة:
  ```python
  default_components = [
      {"name_ar": "الراتب الأساسي", "type": "earning", "calculation": "fixed"},
      {"name_ar": "بدل السكن", "type": "earning", "calculation": "percentage"},
      {"name_ar": "بدل النقل", "type": "earning", "calculation": "fixed"},
      {"name_ar": "التأمينات الاجتماعية", "type": "deduction", "calculation": "percentage"},
  ]
  ```

**اليوم 3-4: Employee Salary Structure**
```python
# backend/routers/salary_structure.py

@router.get("/employees/{employee_id}/salary-structure")
async def get_employee_salary_structure(employee_id: str):
    # جلب هيكل راتب الموظف
    pass

@router.put("/employees/{employee_id}/salary-structure")
async def update_salary_structure(
    employee_id: str,
    structure: List[SalaryStructureItem]
):
    # تحديث هيكل الراتب
    # دعم Effective Dates
    pass

@router.post("/employees/{employee_id}/salary-structure/preview")
async def preview_salary(employee_id: str):
    # معاينة الراتب المحتسب
    pass
```

**المهام:**
- [ ] API لربط مكونات براتب موظف
- [ ] دعم تواريخ السريان (Effective Dates)
- [ ] API لمعاينة الراتب قبل الحفظ
- [ ] Validation للتأكد من عدم وجود تضارب في التواريخ

**اليوم 5: Salary Calculator Service**
```python
# backend/services/salary_calculator.py

class SalaryCalculator:
    def calculate_component(
        self,
        base_salary: Decimal,
        component: SalaryComponent,
        amount: Decimal = None,
        percentage: Decimal = None
    ) -> Decimal:
        if component.calculation_type == "fixed":
            return amount
        elif component.calculation_type == "percentage":
            return base_salary * (percentage / 100)
        elif component.calculation_type == "formula":
            # تقييم المعادلة
            return eval_formula(component.formula, base_salary)
```

### المخرجات:
📦 نظام مرن لمكونات الراتب  
📦 ربط ديناميكي بالموظفين  
📦 Salary Calculator service

---

## 🗓️ الأسبوع 5: محرك حساب الرواتب (Payroll Engine)

### الأهداف:
✅ بناء محرك الحساب الذكي  
✅ حساب الاستحقاقات والاستقطاعات  
✅ معالجة مسير رواتب كامل

### المهام:

**اليوم 1-2: Payroll Engine Core**
```python
# backend/services/payroll_engine.py

class PayrollEngine:
    def __init__(self, pay_period_start, pay_period_end):
        self.start = pay_period_start
        self.end = pay_period_end
    
    async def calculate_employee_salary(self, employee_id):
        # 1. جلب بيانات الموظف
        employee = await get_employee(employee_id)
        
        # 2. حساب الاستحقاقات
        earnings = await self._calculate_earnings(employee)
        
        # 3. حساب الاستقطاعات
        deductions = await self._calculate_deductions(employee)
        
        # 4. حساب الصافي
        gross = sum(earnings.values())
        total_deductions = sum(deductions.values())
        net = gross - total_deductions
        
        return PayrollResult(
            employee_id=employee_id,
            earnings=earnings,
            deductions=deductions,
            gross=gross,
            net=net
        )
```

**المهام:**
- [ ] بناء PayrollEngine class
- [ ] _calculate_earnings() - حساب الاستحقاقات
- [ ] _calculate_deductions() - حساب الاستقطاعات
- [ ] _calculate_overtime() - العمل الإضافي
- [ ] _calculate_absence() - خصم الغياب
- [ ] _calculate_late() - خصم التأخير
- [ ] _calculate_gosi() - التأمينات
- [ ] _calculate_loans() - القروض والسلف

**اليوم 3: Payroll Run APIs**
```python
# backend/routers/payroll.py

@router.post("/payroll/runs")
async def create_payroll_run(payroll: PayrollRunCreate):
    # إنشاء مسير رواتب جديد
    pass

@router.post("/payroll/runs/{id}/calculate")
async def calculate_payroll(run_id: int, background_tasks: BackgroundTasks):
    # حساب الرواتب في الخلفية (Celery)
    background_tasks.add_task(run_payroll_calculation, run_id)
    return {"status": "processing"}

@router.get("/payroll/runs/{id}")
async def get_payroll_run(run_id: int):
    # جلب تفاصيل مسير الرواتب
    pass

@router.post("/payroll/runs/{id}/approve")
async def approve_payroll(run_id: int):
    # اعتماد مسير الرواتب
    pass
```

**المهام:**
- [ ] POST /payroll/runs - إنشاء مسير جديد
- [ ] POST /payroll/runs/{id}/calculate - حساب الرواتب
- [ ] GET /payroll/runs/{id} - جلب التفاصيل
- [ ] POST /payroll/runs/{id}/approve - اعتماد
- [ ] POST /payroll/runs/{id}/cancel - إلغاء

**اليوم 4: Celery Tasks**
```python
# backend/celery_tasks.py
from celery import Celery

celery_app = Celery('payroll', broker='redis://localhost:6379')

@celery_app.task
def run_payroll_calculation(payroll_run_id: int):
    """مهمة خلفية لحساب الرواتب"""
    engine = PayrollEngine(start_date, end_date)
    
    # جلب كل الموظفين النشطين
    employees = get_active_employees()
    
    for emp in employees:
        result = engine.calculate_employee_salary(emp.id)
        save_payslip(payroll_run_id, result)
    
    # تحديث حالة المسير
    update_payroll_status(payroll_run_id, "completed")
```

**المهام:**
- [ ] إعداد Celery + Redis
- [ ] مهمة حساب الرواتب في الخلفية
- [ ] Progress tracking
- [ ] Error handling

**اليوم 5: Testing & Edge Cases**
- [ ] اختبار سيناريوهات مختلفة:
  - موظف بدوام كامل
  - موظف بدوام جزئي
  - موظف سعودي (GOSI 10%)
  - موظف غير سعودي (GOSI 2%)
  - موظف بعمل إضافي
  - موظف بغياب
  - موظف بسلفة نشطة
- [ ] Edge cases:
  - راتب أساسي = 0
  - استقطاعات > راتب
  - موظف انضم منتصف الشهر

### المخرجات:
📦 PayrollEngine يعمل بكفاءة  
📦 معالجة 100 موظف في < 30 ثانية  
📦 دقة حسابية 100%

---

## 🗓️ الأسبوع 6: قسائم الرواتب والتقارير

### الأهداف:
✅ توليد قسائم الرواتب  
✅ PDF generation  
✅ تقارير أساسية

### المهام:

**اليوم 1-2: Payslip Generation**
```python
# backend/services/payslip_generator.py
from reportlab.lib.pagesizes import A4
from reportlab.pdfgen import canvas
from bidi.algorithm import get_display  # للعربية RTL

class PayslipGenerator:
    def generate_pdf(self, payslip_data):
        # إنشاء PDF باللغة العربية
        # Header: شعار الشركة
        # معلومات الموظف
        # جدول الاستحقاقات
        # جدول الاستقطاعات
        # الإجمالي والصافي
        # Footer: توقيع إلكتروني
        pass
```

**المهام:**
- [ ] تصميم قالب قسيمة الراتب
- [ ] دعم العربية (RTL)
- [ ] إنشاء PDF احترافي
- [ ] حفظ PDF في S3/Cloudinary
- [ ] API لتحميل القسيمة

**اليوم 3: Reports APIs**
```python
# backend/routers/reports.py

@router.get("/reports/payroll-summary")
async def payroll_summary_report(month: int, year: int):
    # ملخص الرواتب للشهر
    return {
        "total_employees": 100,
        "total_gross": 500000,
        "total_deductions": 50000,
        "total_net": 450000,
        "by_department": [...]
    }

@router.get("/reports/department-costs")
async def department_costs():
    # تكلفة كل قسم
    pass

@router.get("/reports/gosi")
async def gosi_report(month: int, year: int):
    # تقرير التأمينات الاجتماعية
    pass
```

**المهام:**
- [ ] تقرير ملخص الرواتب
- [ ] تقرير حسب القسم
- [ ] تقرير GOSI
- [ ] تقرير WPS
- [ ] Export to Excel/PDF

**اليوم 4-5: Email & Notifications**
```python
# backend/services/notification_service.py

class NotificationService:
    async def send_payslip_email(self, employee_email, payslip_pdf):
        # إرسال القسيمة عبر البريد
        pass
    
    async def send_whatsapp_notification(self, phone, message):
        # إرسال إشعار واتساب
        pass
    
    async def send_sms(self, phone, message):
        # إرسال SMS
        pass
```

**المهام:**
- [ ] إعداد SendGrid للإيميل
- [ ] قالب إيميل احترافي
- [ ] إرسال قسائم تلقائياً عند الاعتماد
- [ ] WhatsApp Business API integration
- [ ] SMS notifications

### المخرجات:
📦 قسائم رواتب احترافية بصيغة PDF  
📦 5+ تقارير جاهزة  
📦 نظام إشعارات متكامل

---

## 🗓️ الأسبوع 7: Frontend - الإعداد والتصميم

### الأهداف:
✅ إعداد Next.js project  
✅ نظام التصميم (Design System)  
✅ Authentication UI

### المهام:

**اليوم 1: Next.js Setup**
```bash
npx create-next-app@latest payroll-frontend --typescript --tailwind --app
cd payroll-frontend
npm install @radix-ui/react-* shadcn-ui
npm install zustand @tanstack/react-query
npm install recharts date-fns
npm install axios
```

**Structure:**
```
src/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   └── layout.tsx
│   ├── (dashboard)/
│   │   ├── dashboard/
│   │   ├── employees/
│   │   ├── payroll/
│   │   └── layout.tsx
│   └── layout.tsx
├── components/
│   ├── ui/  (shadcn components)
│   ├── layouts/
│   └── forms/
├── lib/
│   ├── api.ts
│   └── utils.ts
└── stores/
    └── auth.ts
```

**اليوم 2: Design System**
```typescript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        primary: '#6366F1',
        secondary: '#8B5CF6',
        success: '#10B981',
        // ... المزيد
      }
    }
  }
}
```

**المهام:**
- [ ] إعداد Tailwind config
- [ ] تثبيت Shadcn UI components
- [ ] إنشاء Design Tokens
- [ ] إنشاء مكونات قابلة لإعادة الاستخدام

**اليوم 3-4: Authentication Pages**
```typescript
// app/(auth)/login/page.tsx
'use client'

export default function LoginPage() {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  
  const handleLogin = async () => {
    const response = await fetch('/api/auth/login', {
      method: 'POST',
      body: JSON.stringify({ email, password })
    })
    // ... handle response
  }
  
  return (
    <div className="min-h-screen flex items-center justify-center">
      <Card className="w-[400px]">
        <CardHeader>
          <CardTitle>تسجيل الدخول</CardTitle>
        </CardHeader>
        <CardContent>
          <Form onSubmit={handleLogin}>
            <Input label="البريد الإلكتروني" />
            <Input label="كلمة المرور" type="password" />
            <Button>دخول</Button>
          </Form>
        </CardContent>
      </Card>
    </div>
  )
}
```

**المهام:**
- [ ] صفحة تسجيل الدخول
- [ ] صفحة نسيت كلمة المرور
- [ ] Zustand store للـ Auth
- [ ] Protected routes middleware

**اليوم 5: Dashboard Layout**
```typescript
// app/(dashboard)/layout.tsx
export default function DashboardLayout({ children }) {
  return (
    <div className="flex h-screen">
      <Sidebar />
      <main className="flex-1 overflow-y-auto">
        <Header />
        <div className="p-6">
          {children}
        </div>
      </main>
    </div>
  )
}
```

### المخرجات:
📦 Next.js project جاهز  
📦 Design system متناسق  
📦 Authentication UI كاملة

---

## 🗓️ الأسبوع 8: Frontend - إدارة الموظفين

### الأهداف:
✅ صفحة قائمة الموظفين  
✅ نموذج إضافة/تعديل موظف  
✅ صفحة تفاصيل الموظف

### المهام:

**اليوم 1-2: Employees List Page**
```typescript
// app/(dashboard)/employees/page.tsx
'use client'

import { useQuery } from '@tanstack/react-query'
import { DataTable } from '@/components/ui/data-table'

export default function EmployeesPage() {
  const { data: employees, isLoading } = useQuery({
    queryKey: ['employees'],
    queryFn: () => fetch('/api/employees').then(res => res.json())
  })
  
  const columns = [
    { accessorKey: 'employee_id', header: 'رقم الموظف' },
    { accessorKey: 'full_name_ar', header: 'الاسم' },
    { accessorKey: 'department', header: 'القسم' },
    { accessorKey: 'job_title_ar', header: 'المسمى الوظيفي' },
    { accessorKey: 'base_salary', header: 'الراتب الأساسي' },
    { accessorKey: 'status', header: 'الحالة' },
    { 
      id: 'actions',
      cell: ({ row }) => <EmployeeActions employee={row.original} />
    }
  ]
  
  return (
    <div>
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-3xl font-bold">الموظفين</h1>
        <Button onClick={() => router.push('/employees/new')}>
          + إضافة موظف
        </Button>
      </div>
      
      <Card>
        <CardContent>
          <DataTable columns={columns} data={employees} />
        </CardContent>
      </Card>
    </div>
  )
}
```

**المهام:**
- [ ] DataTable مع pagination
- [ ] Search & filters
- [ ] Column sorting
- [ ] Export to Excel
- [ ] Bulk actions

**اليوم 3-4: Employee Form**
```typescript
// app/(dashboard)/employees/new/page.tsx
'use client'

import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { z } from 'zod'

const employeeSchema = z.object({
  employee_id: z.string().min(1, 'رقم الموظف مطلوب'),
  full_name_ar: z.string().min(1, 'الاسم مطلوب'),
  nationality: z.string(),
  national_id: z.string(),
  department_id: z.number(),
  base_salary: z.number().positive('الراتب يجب أن يكون أكبر من صفر'),
  // ... المزيد
})

export default function NewEmployeePage() {
  const { register, handleSubmit, formState: { errors } } = useForm({
    resolver: zodResolver(employeeSchema)
  })
  
  const onSubmit = async (data) => {
    await fetch('/api/employees', {
      method: 'POST',
      body: JSON.stringify(data)
    })
    router.push('/employees')
  }
  
  return (
    <Form onSubmit={handleSubmit(onSubmit)}>
      {/* Multi-step form */}
      <Tabs defaultValue="basic">
        <TabsList>
          <TabsTrigger value="basic">البيانات الأساسية</TabsTrigger>
          <TabsTrigger value="employment">بيانات التوظيف</TabsTrigger>
          <TabsTrigger value="salary">بيانات الراتب</TabsTrigger>
          <TabsTrigger value="bank">الحساب البنكي</TabsTrigger>
        </TabsList>
        
        <TabsContent value="basic">
          {/* حقول البيانات الأساسية */}
        </TabsContent>
        
        {/* ... باقي الخطوات */}
      </Tabs>
    </Form>
  )
}
```

**المهام:**
- [ ] Multi-step form
- [ ] Zod validation
- [ ] Dropdown للأقسام والمسميات
- [ ] Date picker
- [ ] File upload للصور
- [ ] Preview قبل الحفظ

**اليوم 5: Employee Details Page**
- [ ] صفحة تفاصيل الموظف
- [ ] Tabs: (معلومات، الراتب، الحضور، الإجازات)
- [ ] تعديل البيانات
- [ ] Timeline للتغييرات

### المخرجات:
📦 إدارة موظفين كاملة في الـ Frontend  
📦 Forms متقدمة مع Validation  
📦 UX احترافية

---

**[يتبع في الرد التالي - باقي 8 أسابيع...]**

## ملاحظة:
الخطة كاملة 16 أسبوع جاهزة! هل تريد:
1. ✅ باقي الأسابيع (9-16) الآن؟
2. ✅ ملف Figma mockups للواجهات؟
3. ✅ Docker Compose للنشر السريع؟
