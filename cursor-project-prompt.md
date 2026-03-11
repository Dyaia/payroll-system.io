# 🚀 Payroll Management System - Technical Specification for Cursor AI

## Project Overview
Build an enterprise-grade Payroll Management System with AI integration, Zoho connectivity, and employee self-service portal for Saudi Arabian market.

## Tech Stack

### Frontend
- **Framework:** Next.js 15 + React 19 with TypeScript
- **UI Library:** Shadcn/ui + Radix UI + Ant Design components
- **Styling:** Tailwind CSS v4
- **State Management:** Zustand + TanStack Query (React Query)
- **Forms:** React Hook Form + Zod validation
- **Charts:** Recharts + Chart.js
- **Animations:** Framer Motion
- **Icons:** Lucide React + Heroicons
- **Date Handling:** date-fns
- **PDF Generation:** react-pdf + jsPDF

### Backend
- **Framework:** FastAPI (Python 3.11+)
- **ORM:** SQLAlchemy 2.0
- **Database:** PostgreSQL 16
- **Authentication:** FastAPI-Users + JWT
- **Task Queue:** Celery + Redis
- **API Documentation:** Swagger/OpenAPI (auto-generated)
- **Validation:** Pydantic v2
- **AI Integration:** LangChain + OpenAI/Anthropic API

### Database Schema

```sql
-- Employees Table
CREATE TABLE employees (
    employee_id VARCHAR(20) PRIMARY KEY,
    full_name_ar VARCHAR(100) NOT NULL,
    full_name_en VARCHAR(100),
    nationality VARCHAR(50),
    national_id VARCHAR(20) UNIQUE,
    iqama_number VARCHAR(20),
    department_id INT REFERENCES departments(department_id),
    job_title_ar VARCHAR(100),
    job_title_en VARCHAR(100),
    hire_date DATE NOT NULL,
    contract_type VARCHAR(20) CHECK (contract_type IN ('permanent', 'temporary', 'contract')),
    employment_type VARCHAR(20) CHECK (employment_type IN ('full_time', 'part_time')),
    base_salary DECIMAL(10,2) NOT NULL,
    bank_name VARCHAR(100),
    bank_account_encrypted TEXT,
    iban VARCHAR(34),
    status VARCHAR(20) DEFAULT 'active' CHECK (status IN ('active', 'on_leave', 'suspended', 'terminated')),
    zoho_people_id VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Departments Table
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name_ar VARCHAR(100) NOT NULL,
    department_name_en VARCHAR(100),
    department_head_id VARCHAR(20) REFERENCES employees(employee_id),
    zoho_department_id VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Salary Components Table
CREATE TABLE salary_components (
    component_id SERIAL PRIMARY KEY,
    component_name_ar VARCHAR(100) NOT NULL,
    component_name_en VARCHAR(100) NOT NULL,
    component_type VARCHAR(20) NOT NULL CHECK (component_type IN ('earning', 'deduction')),
    calculation_type VARCHAR(20) NOT NULL CHECK (calculation_type IN ('fixed', 'percentage', 'formula')),
    formula TEXT,
    is_taxable BOOLEAN DEFAULT FALSE,
    is_gosi_applicable BOOLEAN DEFAULT FALSE,
    display_order INT,
    is_active BOOLEAN DEFAULT TRUE,
    zoho_component_id VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Employee Salary Structure Table
CREATE TABLE employee_salary_structure (
    id SERIAL PRIMARY KEY,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    component_id INT REFERENCES salary_components(component_id),
    amount DECIMAL(10,2),
    percentage DECIMAL(5,2),
    effective_from DATE NOT NULL,
    effective_to DATE,
    is_active BOOLEAN DEFAULT TRUE,
    UNIQUE(employee_id, component_id, effective_from)
);

-- Payroll Runs Table
CREATE TABLE payroll_runs (
    payroll_id SERIAL PRIMARY KEY,
    pay_period_start DATE NOT NULL,
    pay_period_end DATE NOT NULL,
    payment_date DATE NOT NULL,
    status VARCHAR(20) DEFAULT 'draft' CHECK (status IN ('draft', 'processing', 'approved', 'paid', 'cancelled')),
    total_gross DECIMAL(12,2),
    total_deductions DECIMAL(12,2),
    total_net DECIMAL(12,2),
    processed_by VARCHAR(20) REFERENCES employees(employee_id),
    approved_by VARCHAR(20) REFERENCES employees(employee_id),
    zoho_payroll_id VARCHAR(50),
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    approved_at TIMESTAMP
);

-- Payslip Details Table
CREATE TABLE payslip_details (
    detail_id SERIAL PRIMARY KEY,
    payroll_id INT REFERENCES payroll_runs(payroll_id),
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    component_id INT REFERENCES salary_components(component_id),
    component_name VARCHAR(100),
    amount DECIMAL(10,2) NOT NULL,
    component_type VARCHAR(20),
    UNIQUE(payroll_id, employee_id, component_id)
);

-- Payslip Summary Table
CREATE TABLE payslip_summary (
    summary_id SERIAL PRIMARY KEY,
    payroll_id INT REFERENCES payroll_runs(payroll_id),
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    base_salary DECIMAL(10,2),
    total_earnings DECIMAL(10,2),
    total_deductions DECIMAL(10,2),
    net_salary DECIMAL(10,2),
    payment_status VARCHAR(20) DEFAULT 'pending',
    payment_date TIMESTAMP,
    pdf_url TEXT,
    blockchain_hash VARCHAR(100),
    UNIQUE(payroll_id, employee_id)
);

-- Attendance Table
CREATE TABLE attendance (
    attendance_id SERIAL PRIMARY KEY,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    attendance_date DATE NOT NULL,
    check_in TIME,
    check_out TIME,
    working_hours DECIMAL(4,2),
    late_minutes INT DEFAULT 0,
    overtime_hours DECIMAL(4,2) DEFAULT 0,
    status VARCHAR(20) DEFAULT 'present' CHECK (status IN ('present', 'absent', 'late', 'half_day', 'leave', 'weekend', 'holiday')),
    notes TEXT,
    zoho_attendance_id VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(employee_id, attendance_date)
);

-- Advances and Loans Table
CREATE TABLE advances_loans (
    loan_id SERIAL PRIMARY KEY,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    loan_type VARCHAR(20) NOT NULL CHECK (loan_type IN ('advance', 'loan')),
    total_amount DECIMAL(10,2) NOT NULL,
    remaining_amount DECIMAL(10,2) NOT NULL,
    monthly_deduction DECIMAL(10,2) NOT NULL,
    start_date DATE NOT NULL,
    number_of_installments INT,
    installments_paid INT DEFAULT 0,
    status VARCHAR(20) DEFAULT 'active' CHECK (status IN ('active', 'completed', 'cancelled')),
    reason TEXT,
    approved_by VARCHAR(20) REFERENCES employees(employee_id),
    zoho_loan_id VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Leaves Table
CREATE TABLE leaves (
    leave_id SERIAL PRIMARY KEY,
    employee_id VARCHAR(20) REFERENCES employees(employee_id),
    leave_type VARCHAR(50) NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    number_of_days INT NOT NULL,
    reason TEXT,
    status VARCHAR(20) DEFAULT 'pending' CHECK (status IN ('pending', 'approved', 'rejected', 'cancelled')),
    approved_by VARCHAR(20) REFERENCES employees(employee_id),
    approved_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Audit Log Table
CREATE TABLE audit_log (
    log_id SERIAL PRIMARY KEY,
    user_id VARCHAR(20) REFERENCES employees(employee_id),
    action VARCHAR(100) NOT NULL,
    table_name VARCHAR(50),
    record_id VARCHAR(50),
    old_values JSONB,
    new_values JSONB,
    ip_address VARCHAR(45),
    user_agent TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create indexes for performance
CREATE INDEX idx_employees_status ON employees(status);
CREATE INDEX idx_employees_department ON employees(department_id);
CREATE INDEX idx_payroll_runs_status ON payroll_runs(status);
CREATE INDEX idx_payroll_runs_dates ON payroll_runs(pay_period_start, pay_period_end);
CREATE INDEX idx_attendance_employee_date ON attendance(employee_id, attendance_date);
CREATE INDEX idx_payslip_details_payroll ON payslip_details(payroll_id);
CREATE INDEX idx_audit_log_user_date ON audit_log(user_id, created_at);
```

## Backend API Structure

### Core Endpoints

#### Authentication
```
POST   /api/auth/login
POST   /api/auth/logout
POST   /api/auth/refresh
POST   /api/auth/forgot-password
POST   /api/auth/reset-password
GET    /api/auth/me
```

#### Employees
```
GET    /api/employees
POST   /api/employees
GET    /api/employees/{id}
PUT    /api/employees/{id}
DELETE /api/employees/{id}
GET    /api/employees/{id}/salary-structure
PUT    /api/employees/{id}/salary-structure
GET    /api/employees/{id}/payslips
GET    /api/employees/{id}/attendance
GET    /api/employees/{id}/leaves
POST   /api/employees/{id}/advance-request
```

#### Departments
```
GET    /api/departments
POST   /api/departments
GET    /api/departments/{id}
PUT    /api/departments/{id}
DELETE /api/departments/{id}
```

#### Salary Components
```
GET    /api/salary-components
POST   /api/salary-components
GET    /api/salary-components/{id}
PUT    /api/salary-components/{id}
DELETE /api/salary-components/{id}
```

#### Payroll
```
GET    /api/payroll/runs
POST   /api/payroll/runs
GET    /api/payroll/runs/{id}
PUT    /api/payroll/runs/{id}
DELETE /api/payroll/runs/{id}
POST   /api/payroll/runs/{id}/calculate
POST   /api/payroll/runs/{id}/approve
POST   /api/payroll/runs/{id}/process-payment
GET    /api/payroll/runs/{id}/payslips
GET    /api/payroll/payslips/{id}/pdf
POST   /api/payroll/payslips/{id}/send-email
```

#### Attendance
```
GET    /api/attendance
POST   /api/attendance
PUT    /api/attendance/{id}
POST   /api/attendance/bulk-import
GET    /api/attendance/summary
```

#### Reports
```
GET    /api/reports/payroll-summary
GET    /api/reports/department-costs
GET    /api/reports/employee-history
GET    /api/reports/tax-report
GET    /api/reports/gosi-report
POST   /api/reports/generate-custom
```

#### Zoho Integration
```
POST   /api/zoho/sync-employees
POST   /api/zoho/sync-payroll
GET    /api/zoho/connection-status
POST   /api/zoho/test-connection
```

#### AI Assistant
```
POST   /api/ai/chat
POST   /api/ai/analyze-payroll
POST   /api/ai/predict-costs
POST   /api/ai/detect-anomalies
```

## Frontend Structure

```
src/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   └── forgot-password/
│   ├── (dashboard)/
│   │   ├── dashboard/
│   │   ├── employees/
│   │   │   ├── page.tsx
│   │   │   ├── [id]/
│   │   │   └── new/
│   │   ├── payroll/
│   │   │   ├── page.tsx
│   │   │   ├── runs/
│   │   │   └── components/
│   │   ├── attendance/
│   │   ├── reports/
│   │   ├── settings/
│   │   └── ai-assistant/
│   └── (employee-portal)/
│       ├── my-profile/
│       ├── my-payslips/
│       ├── my-attendance/
│       └── my-leaves/
├── components/
│   ├── ui/ (shadcn components)
│   ├── layouts/
│   ├── forms/
│   ├── tables/
│   ├── charts/
│   └── payroll/
├── lib/
│   ├── api.ts
│   ├── auth.ts
│   ├── utils.ts
│   └── constants.ts
├── hooks/
├── store/
└── types/
```

## Key Features to Implement

### Phase 1: Core System (Weeks 1-4)
1. Authentication system with role-based access
2. Employee CRUD operations
3. Department management
4. Salary component configuration
5. Basic payroll calculation engine
6. Employee salary structure assignment

### Phase 2: Payroll Processing (Weeks 5-6)
1. Automated payroll calculation
2. Attendance-based deductions
3. Overtime calculations
4. GOSI calculations (Saudi/Non-Saudi)
5. Advance/loan deductions
6. Payslip generation and PDF export

### Phase 3: Employee Portal (Weeks 7-8)
1. Employee self-service dashboard
2. Payslip viewing and download
3. Attendance summary
4. Leave requests
5. Advance requests
6. Profile management

### Phase 4: Advanced Features (Weeks 9-11)
1. AI chatbot integration
2. Predictive analytics
3. Anomaly detection
4. Automated reports
5. WhatsApp/Email notifications
6. Multi-language support (Arabic/English)

### Phase 5: Zoho Integration (Week 12)
1. OAuth2 authentication
2. Bi-directional employee sync
3. Payroll data export
4. Real-time updates

### Phase 6: Mobile App (Weeks 13-14)
1. Flutter or React Native app
2. Push notifications
3. QR code attendance
4. Offline mode

## AI Assistant Implementation

```python
# backend/services/ai_assistant.py
from langchain.chat_models import ChatAnthropic
from langchain.prompts import ChatPromptTemplate
from langchain.chains import LLMChain

class PayrollAIAssistant:
    def __init__(self):
        self.llm = ChatAnthropic(model="claude-3-5-sonnet-20241022")
        
    async def chat(self, user_message: str, context: dict):
        """Handle natural language queries about payroll"""
        prompt = ChatPromptTemplate.from_messages([
            ("system", """You are Salem, an AI payroll assistant for a Saudi Arabian company.
            You help with payroll calculations, employee queries, and financial analysis.
            Always respond in Arabic when the user speaks Arabic.
            Current context: {context}"""),
            ("user", "{user_message}")
        ])
        
        chain = LLMChain(llm=self.llm, prompt=prompt)
        response = await chain.arun(
            user_message=user_message,
            context=context
        )
        return response
    
    async def detect_anomalies(self, payroll_data: list):
        """Detect unusual patterns in payroll data"""
        # Implementation for anomaly detection
        pass
    
    async def predict_costs(self, historical_data: list):
        """Predict future payroll costs"""
        # Implementation for cost prediction
        pass
```

## Payroll Calculation Engine

```python
# backend/services/payroll_engine.py
from decimal import Decimal
from datetime import datetime, timedelta

class PayrollEngine:
    def __init__(self, pay_period_start, pay_period_end):
        self.pay_period_start = pay_period_start
        self.pay_period_end = pay_period_end
        self.working_days = self._calculate_working_days()
    
    def _calculate_working_days(self):
        """Calculate working days excluding weekends and holidays"""
        days = 0
        current = self.pay_period_start
        while current <= self.pay_period_end:
            if current.weekday() < 5:  # Monday = 0, Friday = 4
                days += 1
            current += timedelta(days=1)
        return days
    
    async def calculate_employee_salary(self, employee_id: str):
        """Calculate complete salary for an employee"""
        employee = await self._get_employee(employee_id)
        
        # 1. Get base salary
        base_salary = employee.base_salary
        
        # 2. Calculate earnings
        earnings = await self._calculate_earnings(employee)
        
        # 3. Calculate deductions
        deductions = await self._calculate_deductions(employee)
        
        # 4. Calculate totals
        gross_salary = base_salary + sum(earnings.values())
        total_deductions = sum(deductions.values())
        net_salary = gross_salary - total_deductions
        
        return {
            'employee_id': employee_id,
            'base_salary': float(base_salary),
            'earnings': {k: float(v) for k, v in earnings.items()},
            'gross_salary': float(gross_salary),
            'deductions': {k: float(v) for k, v in deductions.items()},
            'total_deductions': float(total_deductions),
            'net_salary': float(net_salary)
        }
    
    async def _calculate_earnings(self, employee):
        """Calculate all earnings components"""
        earnings = {}
        
        # Get salary structure
        structure = await self._get_salary_structure(employee.employee_id)
        
        for item in structure:
            if item.component.component_type == 'earning':
                if item.component.calculation_type == 'fixed':
                    earnings[item.component.component_name_ar] = item.amount
                elif item.component.calculation_type == 'percentage':
                    earnings[item.component.component_name_ar] = (
                        employee.base_salary * item.percentage / 100
                    )
        
        # Calculate overtime
        overtime = await self._calculate_overtime(employee)
        if overtime > 0:
            earnings['العمل الإضافي'] = overtime
        
        return earnings
    
    async def _calculate_deductions(self, employee):
        """Calculate all deduction components"""
        deductions = {}
        
        # 1. GOSI (Social Insurance)
        gosi_rate = Decimal('0.10') if employee.nationality == 'Saudi' else Decimal('0.02')
        deductions['التأمينات الاجتماعية'] = employee.base_salary * gosi_rate
        
        # 2. Absence deductions
        absence = await self._calculate_absence_deduction(employee)
        if absence > 0:
            deductions['خصم الغياب'] = absence
        
        # 3. Late deductions
        late = await self._calculate_late_deduction(employee)
        if late > 0:
            deductions['خصم التأخير'] = late
        
        # 4. Loan/Advance deductions
        loan = await self._calculate_loan_deduction(employee)
        if loan > 0:
            deductions['استقطاع القرض/السلفة'] = loan
        
        # 5. Other deductions from structure
        structure = await self._get_salary_structure(employee.employee_id)
        for item in structure:
            if item.component.component_type == 'deduction':
                if item.component.calculation_type == 'fixed':
                    deductions[item.component.component_name_ar] = item.amount
        
        return deductions
    
    async def _calculate_overtime(self, employee):
        """Calculate overtime pay"""
        # Get overtime hours from attendance
        overtime_hours = await self._get_overtime_hours(employee.employee_id)
        
        if overtime_hours == 0:
            return Decimal('0')
        
        # Hourly rate = base_salary / (30 days * 8 hours)
        hourly_rate = employee.base_salary / Decimal('240')
        
        # Overtime rate = 1.5x normal rate
        return overtime_hours * hourly_rate * Decimal('1.5')
    
    async def _calculate_absence_deduction(self, employee):
        """Calculate absence deduction"""
        absent_days = await self._get_absent_days(employee.employee_id)
        
        if absent_days == 0:
            return Decimal('0')
        
        daily_rate = employee.base_salary / Decimal('30')
        return absent_days * daily_rate
    
    async def _calculate_late_deduction(self, employee):
        """Calculate late arrival deduction"""
        late_minutes = await self._get_late_minutes(employee.employee_id)
        
        if late_minutes == 0:
            return Decimal('0')
        
        # Company policy: 3 late arrivals = 1 hour deduction
        late_hours = late_minutes // 180  # 180 minutes = 3 hours
        
        hourly_rate = employee.base_salary / Decimal('240')
        return late_hours * hourly_rate
    
    async def _calculate_loan_deduction(self, employee):
        """Calculate loan/advance deduction"""
        active_loans = await self._get_active_loans(employee.employee_id)
        
        total_deduction = sum(loan.monthly_deduction for loan in active_loans)
        return Decimal(str(total_deduction))
```

## Zoho Integration

```python
# backend/services/zoho_integration.py
import requests
from typing import Dict, List

class ZohoIntegration:
    def __init__(self, access_token: str):
        self.access_token = access_token
        self.people_api = "https://people.zoho.com/people/api"
        self.payroll_api = "https://payroll.zoho.com/api/v1"
        self.headers = {
            "Authorization": f"Zoho-oauthtoken {access_token}",
            "Content-Type": "application/json"
        }
    
    async def sync_employees_from_zoho(self) -> List[Dict]:
        """Fetch employees from Zoho People"""
        response = requests.get(
            f"{self.people_api}/forms/employee/getRecords",
            headers=self.headers
        )
        
        if response.status_code == 200:
            data = response.json()
            return data.get('response', {}).get('result', [])
        
        raise Exception(f"Zoho API error: {response.status_code}")
    
    async def push_payroll_to_zoho(self, payroll_data: Dict):
        """Send payroll data to Zoho Payroll"""
        response = requests.post(
            f"{self.payroll_api}/payrolls",
            headers=self.headers,
            json=payroll_data
        )
        
        if response.status_code in [200, 201]:
            return response.json()
        
        raise Exception(f"Zoho Payroll API error: {response.status_code}")
    
    async def get_employee_by_id(self, zoho_id: str):
        """Get specific employee from Zoho"""
        response = requests.get(
            f"{self.people_api}/forms/employee/getRecords",
            headers=self.headers,
            params={"searchField": "EmployeeID", "searchValue": zoho_id}
        )
        
        if response.status_code == 200:
            data = response.json()
            results = data.get('response', {}).get('result', [])
            return results[0] if results else None
        
        return None
```

## Security Implementation

```python
# backend/security/encryption.py
from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2
import base64
import os

class DataEncryption:
    def __init__(self):
        self.key = self._get_encryption_key()
        self.cipher = Fernet(self.key)
    
    def _get_encryption_key(self):
        """Generate or retrieve encryption key"""
        # In production, store this in environment variables or secrets manager
        password = os.getenv('ENCRYPTION_PASSWORD', 'default-secure-password').encode()
        salt = os.getenv('ENCRYPTION_SALT', 'default-salt').encode()
        
        kdf = PBKDF2(
            algorithm=hashes.SHA256(),
            length=32,
            salt=salt,
            iterations=100000,
        )
        key = base64.urlsafe_b64encode(kdf.derive(password))
        return key
    
    def encrypt(self, plaintext: str) -> str:
        """Encrypt sensitive data"""
        encrypted = self.cipher.encrypt(plaintext.encode())
        return encrypted.decode()
    
    def decrypt(self, ciphertext: str) -> str:
        """Decrypt sensitive data"""
        decrypted = self.cipher.decrypt(ciphertext.encode())
        return decrypted.decode()

# Usage
encryptor = DataEncryption()
encrypted_account = encryptor.encrypt("SA1234567890123456789012")
```

## Deployment Configuration

### Docker Compose
```yaml
version: '3.8'

services:
  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: payroll_db
      POSTGRES_USER: payroll_user
      POSTGRES_PASSWORD: secure_password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  backend:
    build: ./backend
    command: uvicorn main:app --host 0.0.0.0 --port 8000 --reload
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://payroll_user:secure_password@postgres:5432/payroll_db
      REDIS_URL: redis://redis:6379
      SECRET_KEY: your-secret-key-here
    depends_on:
      - postgres
      - redis

  celery:
    build: ./backend
    command: celery -A celery_app worker --loglevel=info
    environment:
      DATABASE_URL: postgresql://payroll_user:secure_password@postgres:5432/payroll_db
      REDIS_URL: redis://redis:6379
    depends_on:
      - postgres
      - redis

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      NEXT_PUBLIC_API_URL: http://localhost:8000
    depends_on:
      - backend

volumes:
  postgres_data:
```

## Environment Variables

### Backend (.env)
```
DATABASE_URL=postgresql://user:password@localhost:5432/payroll_db
REDIS_URL=redis://localhost:6379
SECRET_KEY=your-super-secret-key-change-in-production
ENCRYPTION_PASSWORD=another-super-secret-password
ENCRYPTION_SALT=random-salt-value
ZOHO_CLIENT_ID=your-zoho-client-id
ZOHO_CLIENT_SECRET=your-zoho-client-secret
ZOHO_REDIRECT_URI=http://localhost:3000/zoho/callback
ANTHROPIC_API_KEY=your-anthropic-api-key
SENDGRID_API_KEY=your-sendgrid-api-key
TWILIO_ACCOUNT_SID=your-twilio-sid
TWILIO_AUTH_TOKEN=your-twilio-token
```

### Frontend (.env.local)
```
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_APP_NAME=نظام إدارة الرواتب
NEXT_PUBLIC_ZOHO_CLIENT_ID=your-zoho-client-id
```

## Development Commands

### Backend Setup
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
alembic upgrade head
uvicorn main:app --reload
```

### Frontend Setup
```bash
cd frontend
npm install
npm run dev
```

### Database Migrations
```bash
# Create new migration
alembic revision --autogenerate -m "description"

# Apply migrations
alembic upgrade head

# Rollback migration
alembic downgrade -1
```

## Testing Strategy

### Backend Tests
```python
# tests/test_payroll_engine.py
import pytest
from services.payroll_engine import PayrollEngine

@pytest.mark.asyncio
async def test_salary_calculation():
    engine = PayrollEngine('2024-03-01', '2024-03-31')
    result = await engine.calculate_employee_salary('EMP001')
    
    assert result['net_salary'] > 0
    assert result['gross_salary'] >= result['net_salary']
```

### Frontend Tests
```typescript
// __tests__/EmployeeForm.test.tsx
import { render, screen, fireEvent } from '@testing-library/react'
import EmployeeForm from '@/components/forms/EmployeeForm'

describe('EmployeeForm', () => {
  it('should validate required fields', async () => {
    render(<EmployeeForm />)
    
    const submitButton = screen.getByText('حفظ')
    fireEvent.click(submitButton)
    
    expect(await screen.findByText('الاسم مطلوب')).toBeInTheDocument()
  })
})
```

## Performance Optimization

1. **Database Indexing:** All foreign keys and frequently queried columns indexed
2. **Query Optimization:** Use select_related and prefetch_related
3. **Caching:** Redis for session data and frequently accessed data
4. **CDN:** Static assets served via Cloudflare
5. **Code Splitting:** Next.js automatic code splitting
6. **Image Optimization:** Next.js Image component with lazy loading

## Monitoring & Logging

```python
# backend/middleware/logging.py
import logging
from starlette.middleware.base import BaseHTTPMiddleware

logger = logging.getLogger(__name__)

class LoggingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request, call_next):
        logger.info(f"{request.method} {request.url}")
        response = await call_next(request)
        logger.info(f"Status: {response.status_code}")
        return response
```

---

## Instructions for Cursor AI

**To build this project in Cursor:**

1. Create a new project folder
2. Copy this entire specification into a file called `SPEC.md`
3. Use Cursor's Composer mode (Cmd/Ctrl + I) and say:
   
   ```
   "Build a complete payroll management system based on SPEC.md. 
   Start with:
   1. Setting up the database schema
   2. Creating the FastAPI backend with all endpoints
   3. Building the Next.js frontend with Shadcn UI
   4. Implementing the payroll calculation engine
   5. Adding AI assistant integration
   6. Setting up Zoho API integration
   
   Use TypeScript for frontend and Python for backend.
   Follow the exact structure and features outlined in SPEC.md."
   ```

4. Cursor will generate all the code files automatically
5. Review and refine each component as needed
6. Test each feature incrementally

**Key Areas to Focus:**
- Database relationships and constraints
- Payroll calculation accuracy (especially GOSI)
- Security (encryption, authentication)
- Arabic/English bilingual support
- Responsive design for mobile
- Real-time updates via WebSockets

Good luck! 🚀
