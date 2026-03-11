# 🎨 Figma Design Specifications - Payroll System
## مواصفات تصميم واجهات نظام الرواتب

**Version:** 1.0  
**Design System:** Modern Arabic RTL Interface  
**Figma File:** Create new Figma file and follow these specs

---

## 📐 Design System Foundation

### Color Palette

```css
/* Primary Colors */
--primary-50: #EEF2FF;
--primary-100: #E0E7FF;
--primary-500: #6366F1;  /* Main Primary */
--primary-600: #4F46E5;
--primary-700: #4338CA;

/* Success (Green) */
--success-50: #ECFDF5;
--success-500: #10B981;
--success-600: #059669;

/* Error (Red) */
--error-50: #FEF2F2;
--error-500: #EF4444;
--error-600: #DC2626;

/* Warning (Orange) */
--warning-50: #FFF7ED;
--warning-500: #F97316;
--warning-600: #EA580C;

/* Neutrals (Gray) */
--gray-50: #F9FAFB;
--gray-100: #F3F4F6;
--gray-200: #E5E7EB;
--gray-300: #D1D5DB;
--gray-400: #9CA3AF;
--gray-500: #6B7280;
--gray-600: #4B5563;
--gray-700: #374151;
--gray-800: #1F2937;
--gray-900: #111827;

/* Background */
--bg-primary: #FFFFFF;
--bg-secondary: #F9FAFB;
--bg-tertiary: #F3F4F6;

/* Text */
--text-primary: #111827;
--text-secondary: #6B7280;
--text-tertiary: #9CA3AF;
```

### Typography (Arabic-First)

```
Font Family: 
  - Primary: 'Tajawal' (Google Fonts - Arabic)
  - Secondary: 'Inter' (English fallback)
  - Monospace: 'IBM Plex Mono' (for numbers)

Font Sizes:
  - Display Large: 48px / 3rem
  - Display: 36px / 2.25rem
  - Heading 1: 30px / 1.875rem
  - Heading 2: 24px / 1.5rem
  - Heading 3: 20px / 1.25rem
  - Heading 4: 18px / 1.125rem
  - Body Large: 16px / 1rem
  - Body: 14px / 0.875rem
  - Body Small: 12px / 0.75rem
  - Caption: 11px / 0.6875rem

Font Weights:
  - Regular: 400
  - Medium: 500
  - Semibold: 600
  - Bold: 700

Line Heights:
  - Tight: 1.2
  - Normal: 1.5
  - Relaxed: 1.75
```

### Spacing Scale

```
Space-2: 2px
Space-4: 4px
Space-8: 8px
Space-12: 12px
Space-16: 16px
Space-20: 20px
Space-24: 24px
Space-32: 32px
Space-40: 40px
Space-48: 48px
Space-64: 64px
Space-80: 80px
```

### Border Radius

```
Radius-SM: 4px
Radius-MD: 8px
Radius-LG: 12px
Radius-XL: 16px
Radius-2XL: 24px
Radius-Full: 9999px
```

### Shadows

```
Shadow-SM: 0 1px 2px rgba(0, 0, 0, 0.05)
Shadow-MD: 0 4px 6px rgba(0, 0, 0, 0.07)
Shadow-LG: 0 10px 15px rgba(0, 0, 0, 0.1)
Shadow-XL: 0 20px 25px rgba(0, 0, 0, 0.15)
```

---

## 🖼️ Screen Designs (All Screens)

### 1. Login Page (صفحة تسجيل الدخول)

**Canvas Size:** 1920 x 1080 (Desktop)

**Layout:**
```
┌─────────────────────────────────────────┐
│                                         │
│  ┌───────────────────────────────────┐ │
│  │                                   │ │
│  │         [Company Logo]            │ │
│  │                                   │ │
│  │   نظام إدارة الرواتب والموارد   │ │
│  │      Payroll Management System    │ │
│  │                                   │ │
│  │   ┌─────────────────────────┐    │ │
│  │   │ 📧 البريد الإلكتروني  │    │ │
│  │   │ [Input Field]           │    │ │
│  │   └─────────────────────────┘    │ │
│  │                                   │ │
│  │   ┌─────────────────────────┐    │ │
│  │   │ 🔒 كلمة المرور        │    │ │
│  │   │ [Input Field]           │    │ │
│  │   └─────────────────────────┘    │ │
│  │                                   │ │
│  │   ☐ تذكرني    نسيت كلمة المرور؟ │ │
│  │                                   │ │
│  │   ┌─────────────────────────┐    │ │
│  │   │      [تسجيل الدخول]     │    │ │
│  │   │      (Primary Button)    │    │ │
│  │   └─────────────────────────┘    │ │
│  │                                   │ │
│  │   Powered by [Company Name]       │ │
│  └───────────────────────────────────┘ │
│                                         │
└─────────────────────────────────────────┘
```

**Components:**
- Card: 400px width, centered, shadow-xl, radius-xl
- Logo: 80px height
- Heading: "نظام إدارة الرواتب" (Heading 2, primary color)
- Input Fields: 
  - Height: 48px
  - Border: 1px gray-300
  - Focus: border-primary-500, shadow
  - RTL placeholder text
- Button:
  - Primary color background
  - White text
  - Height: 48px
  - Full width
  - Hover: darken 10%
- Links: primary-500 color, 14px

---

### 2. Dashboard (لوحة التحكم)

**Canvas Size:** 1920 x 1080 (Desktop)

**Layout:**
```
┌─────────────────────────────────────────────────────────┐
│ [Logo]  نظام الرواتب        🔔 👤 محمد أحمد ▼        │ Header (h-16)
├───┬─────────────────────────────────────────────────────┤
│   │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│   │  │ 👥      │ │ 💰      │ │ 📊      │ │ 📅      │ │
│   │  │ الموظفين│ │ الرواتب │ │ التقارير│ │ الحضور  │ │
│ S │  │ 234     │ │ 2.5M SAR│ │ 15      │ │ 95%     │ │
│ i │  └─────────┘ └─────────┘ └─────────┘ └─────────┘ │
│ d │                                                    │
│ e │  ┌──────────────────────────────────────────────┐ │
│ b │  │ 📈 ملخص الرواتب الشهري                     │ │
│ a │  │                                              │ │
│ r │  │  [Line Chart: 6 months trend]               │ │
│   │  │  يناير فبراير مارس أبريل مايو يونيو        │ │
│   │  └──────────────────────────────────────────────┘ │
│   │                                                    │
│   │  ┌──────────────────┐  ┌──────────────────────┐  │
│   │  │ آخر العمليات    │  │ الموظفون الجدد      │  │
│   │  │                  │  │                      │  │
│   │  │ ✓ راتب يناير    │  │ • أحمد علي         │  │
│   │  │ ⏳ إجازة محمد   │  │ • فاطمة سعيد        │  │
│   │  │ ✓ حضور اليوم     │  │ • خالد يوسف        │  │
│   │  └──────────────────┘  └──────────────────────┘  │
│   │                                                    │
└───┴────────────────────────────────────────────────────┘
```

**Sidebar (w-64):**
```
┌────────────────┐
│ 🏠 الرئيسية   │ (active: bg-primary-50, text-primary-600)
│ 👥 الموظفين    │
│ 💰 الرواتب     │
│ 📅 الحضور      │
│ 🏖️ الإجازات    │
│ 💳 السلف       │
│ 📊 التقارير    │
│ ⚙️ الإعدادات   │
│                │
│ 🚪 تسجيل خروج  │
└────────────────┘
```

**Stats Cards:**
- Width: Auto (grid-cols-4)
- Height: 120px
- Background: white
- Border: 1px gray-200
- Radius: lg
- Shadow: sm
- Icon: 40px, primary-500 background circle
- Title: Body, gray-600
- Value: Heading 2, gray-900

**Chart Card:**
- Height: 400px
- Recharts Line Chart
- Primary color line
- Gray grid
- Smooth curves

**Activity Lists:**
- Card with list items
- Each item: icon + text
- Spacing: 12px between items
- Height: 300px
- Scrollable

---

### 3. Employees List (قائمة الموظفين)

**Canvas Size:** 1920 x 1080

**Layout:**
```
┌─────────────────────────────────────────────────────────┐
│ Header + Sidebar (same as dashboard)                    │
├───┬─────────────────────────────────────────────────────┤
│   │ ┌─────────────────────────────────────────────────┐│
│   │ │ 👥 الموظفين              [+ إضافة موظف]      ││
│   │ └─────────────────────────────────────────────────┘│
│   │                                                     │
│ S │ ┌─────────────────────────────────────────────────┐│
│ i │ │ 🔍 بحث...    [القسم ▼] [الحالة ▼] [تصدير ⬇] ││
│ d │ └─────────────────────────────────────────────────┘│
│ e │                                                     │
│ b │ ┌─────────────────────────────────────────────────┐│
│ a │ │ رقم الموظف│الاسم   │القسم│المسمى│الراتب│الحالة││
│ r │ │─────────────────────────────────────────────────││
│   │ │ EMP001│محمد أحمد│IT │مطور│12,000│●نشط    ││
│   │ │ EMP002│سارة علي│HR │محاسب│10,000│●نشط    ││
│   │ │ EMP003│خالد سعد│FIN│مدير│20,000│●نشط    ││
│   │ │ ...                                           ││
│   │ └─────────────────────────────────────────────────┘│
│   │ عرض 1-10 من 234           [◀ 1 2 3 4 ... ▶]     │
└───┴─────────────────────────────────────────────────────┘
```

**Page Header:**
- Flex justify-between
- Left: Icon + Title (Heading 2)
- Right: Primary button "+ إضافة موظف"

**Filters Row:**
- Background: white
- Padding: 16px
- Border radius: lg
- Shadow: sm
- Search input: 300px width
- Dropdowns: 150px width each
- Export button: secondary style

**Table:**
- Background: white
- Border: 1px gray-200
- Header: bg-gray-50, font-medium
- Rows: hover bg-gray-50
- Borders between rows: gray-100
- RTL alignment
- Status badges:
  - نشط: green badge
  - غير نشط: gray badge
  - مفصول: red badge

**Pagination:**
- Centered
- Buttons: border, hover effect
- Current page: primary background

---

### 4. Add Employee Form (نموذج إضافة موظف)

**Canvas Size:** 1920 x 1080

**Layout:**
```
┌─────────────────────────────────────────────────────────┐
│ Header + Sidebar                                        │
├───┬─────────────────────────────────────────────────────┤
│   │ ┌─────────────────────────────────────────────────┐│
│   │ │ ← الرجوع     إضافة موظف جديد                  ││
│   │ └─────────────────────────────────────────────────┘│
│   │                                                     │
│ S │ ┌─────────────────────────────────────────────────┐│
│ i │ │ ① البيانات الأساسية ② التوظيف ③ الراتب ④ البنك││
│ d │ │ ═══════════════════                             ││
│ e │ └─────────────────────────────────────────────────┘│
│ b │                                                     │
│ a │ ┌─────────────────────────────────────────────────┐│
│ r │ │ الاسم الكامل (عربي) *                          ││
│   │ │ [Input]                                         ││
│   │ │                                                 ││
│   │ │ الاسم الكامل (إنجليزي)                        ││
│   │ │ [Input]                                         ││
│   │ │                                                 ││
│   │ │ رقم الهوية/الإقامة *      الجنسية *           ││
│   │ │ [Input]                    [Dropdown]           ││
│   │ │                                                 ││
│   │ │ تاريخ الميلاد *            الجنس *             ││
│   │ │ [Date Picker]              [○ ذكر ○ أنثى]     ││
│   │ │                                                 ││
│   │ │ البريد الإلكتروني *       رقم الهاتف *        ││
│   │ │ [Input]                    [Input]              ││
│   │ │                                                 ││
│   │ │              [إلغاء]  [التالي ◀]               ││
│   │ └─────────────────────────────────────────────────┘│
└───┴─────────────────────────────────────────────────────┘
```

**Step Indicator:**
- Horizontal tabs
- Active step: primary color + underline
- Completed: checkmark
- Inactive: gray

**Form Fields:**
- Label: Body, gray-700, margin-bottom: 8px
- Required fields: red asterisk
- Input height: 44px
- Border: 1px gray-300
- Focus: border-primary-500 + shadow
- Error state: border-red-500 + red text below

**Buttons:**
- Cancel: Secondary (gray border)
- Next/Save: Primary
- Alignment: left (in RTL)
- Spacing: 12px between

---

### 5. Payroll Run Page (مسير الرواتب)

**Canvas Size:** 1920 x 1080

**Layout:**
```
┌─────────────────────────────────────────────────────────┐
│ Header + Sidebar                                        │
├───┬─────────────────────────────────────────────────────┤
│   │ ┌─────────────────────────────────────────────────┐│
│   │ │ 💰 مسيرات الرواتب         [+ مسير جديد]       ││
│   │ └─────────────────────────────────────────────────┘│
│   │                                                     │
│ S │ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌─────────┐│
│ i │ │إجمالي    │ │الاستحقاقات│ │الاستقطاعات│ │الصافي  ││
│ d │ │الموظفين │ │          │ │          │ │        ││
│ e │ │  234     │ │ 3.2M SAR │ │ 320K SAR │ │2.88M SAR││
│ b │ └──────────┘ └──────────┘ └──────────┘ └─────────┘│
│ a │                                                     │
│ r │ ┌─────────────────────────────────────────────────┐│
│   │ │ المسير          │ الفترة      │الموظفين│الحالة ││
│   │ │─────────────────────────────────────────────────││
│   │ │ رواتب يناير 2026│1-31 يناير │234│🟢 مكتمل   ││
│   │ │ رواتب ديسمبر   │1-31 ديسمبر│230│🟢 مدفوع   ││
│   │ │ رواتب نوفمبر   │1-30 نوفمبر│228│🟢 مدفوع   ││
│   │ │ ...                                           ││
│   │ └─────────────────────────────────────────────────┘│
└───┴─────────────────────────────────────────────────────┘
```

**Status Badges:**
- مسودة (Draft): gray badge
- جاري الحساب (Calculating): blue badge with spinner
- محسوب (Calculated): yellow badge
- معتمد (Approved): green badge
- مدفوع (Paid): dark green badge

---

### 6. Payslip View (قسيمة الراتب)

**Canvas Size:** A4 (210mm x 297mm)

**Layout:**
```
┌─────────────────────────────────────────────┐
│ [Company Logo]        قسيمة راتب           │
│                                             │
│ اسم الشركة: شركة XYZ المحدودة              │
│ الفترة: يناير 2026                         │
│ تاريخ الدفع: 2026/01/31                    │
│                                             │
│─────────────────────────────────────────────│
│                                             │
│ معلومات الموظف                             │
│ رقم الموظف: EMP001                         │
│ الاسم: محمد أحمد علي                       │
│ القسم: تقنية المعلومات                     │
│ المسمى الوظيفي: مطور برمجيات               │
│                                             │
│─────────────────────────────────────────────│
│                                             │
│ الاستحقاقات            المبلغ (ريال)       │
│ الراتب الأساسي         10,000.00           │
│ بدل السكن              2,500.00            │
│ بدل النقل              500.00              │
│ العمل الإضافي          300.00              │
│                        ─────────            │
│ إجمالي الاستحقاقات     13,300.00           │
│                                             │
│─────────────────────────────────────────────│
│                                             │
│ الاستقطاعات            المبلغ (ريال)       │
│ التأمينات الاجتماعية   1,250.00            │
│ الغياب                 0.00                │
│ السلفة                 500.00              │
│                        ─────────            │
│ إجمالي الاستقطاعات     1,750.00            │
│                                             │
│─────────────────────────────────────────────│
│                                             │
│ صافي الراتب المستحق     11,550.00 ريال     │
│                                             │
│─────────────────────────────────────────────│
│                                             │
│ ملخص الحضور                                │
│ أيام العمل: 22   الحضور: 20   الغياب: 0   │
│ التأخير: 2      العمل الإضافي: 2 ساعة     │
│                                             │
│─────────────────────────────────────────────│
│                                             │
│ تم إنشاء هذه القسيمة تلقائياً              │
│ 2026/01/31 10:30 AM                        │
│                                             │
└─────────────────────────────────────────────┘
```

**Styling:**
- Font: Tajawal (Arabic)
- Header: primary color background, white text
- Sections: bordered boxes
- Numbers: IBM Plex Mono, left-aligned (LTR)
- Totals: bold, larger font
- Net salary: highlighted box, primary color

---

### 7. Mobile App Screens (تطبيق الموبايل)

**Canvas Size:** 375 x 812 (iPhone X)

#### Home Screen
```
┌─────────────────────┐
│ مرحباً، محمد أحمد   │ Header
│ EMP001             │
├─────────────────────┤
│                     │
│ ┌─────────────────┐ │
│ │ الراتب القادم   │ │
│ │ 11,550 ريال     │ │ Card
│ │ 📅 31 يناير 2026│ │
│ └─────────────────┘ │
│                     │
│ ┌─────────────────┐ │
│ │ 📄 قسائم الرواتب│ │
│ │ [عرض الكل ◀]   │ │
│ │                 │ │
│ │ • يناير 2026    │ │ List
│ │   11,550 ريال   │ │
│ │                 │ │
│ │ • ديسمبر 2025   │ │
│ │   11,200 ريال   │ │
│ └─────────────────┘ │
│                     │
│ ┌─────────────────┐ │
│ │ 📊 رصيد الإجازات│ │
│ │ 15 يوم متبقي    │ │
│ └─────────────────┘ │
│                     │
├─────────────────────┤
│ [🏠] [📄] [📅] [👤]│ Bottom Nav
└─────────────────────┘
```

---

## 🎯 Figma Design Instructions

### Step 1: Setup Figma File
1. Create new Figma file: "Payroll System Design"
2. Create pages:
   - 🎨 Design System
   - 🖥️ Desktop Screens
   - 📱 Mobile Screens
   - 🧩 Components Library

### Step 2: Create Design System
1. **Colors:**
   - Create color styles for all colors above
   - Name format: "Primary/500", "Gray/100", etc.

2. **Typography:**
   - Import Tajawal font (Google Fonts)
   - Create text styles for each size
   - Name format: "Heading/H1", "Body/Regular", etc.

3. **Components:**
   - Button (Primary, Secondary, Ghost, Icon)
   - Input (Default, Focus, Error, Disabled)
   - Card
   - Badge (Success, Warning, Error, Info)
   - Table Row
   - Sidebar Item
   - Stats Card

### Step 3: Design Screens
Follow the layouts above for each screen.

**Desktop Screens to Create:**
1. Login Page
2. Dashboard
3. Employees List
4. Add Employee Form (4 steps)
5. Employee Details
6. Payroll Run List
7. Create Payroll Run
8. Payroll Run Details
9. Payslip View (PDF format)
10. Reports Page

**Mobile Screens to Create:**
1. Login
2. Home
3. Payslips List
4. Payslip Detail
5. Leave Request
6. Profile

### Step 4: Create Prototypes
1. Link screens with interactions
2. Add hover states
3. Add transitions (ease-in-out, 200ms)

### Step 5: Export Assets
- Export icons as SVG
- Export logos in multiple sizes
- Create design specs document

---

## 📋 Design Checklist

- [ ] All colors from design system used
- [ ] Arabic (RTL) layout correct
- [ ] Font: Tajawal for Arabic text
- [ ] All buttons have hover states
- [ ] Forms have focus and error states
- [ ] Mobile screens responsive (375-428px width)
- [ ] Accessibility: 
  - [ ] Color contrast ratio > 4.5:1
  - [ ] Touch targets > 44px
  - [ ] Clear focus indicators
- [ ] Icons consistent (use Lucide or Heroicons)
- [ ] Spacing follows 8px grid
- [ ] Components reusable
- [ ] Dark mode variants (optional)

---

## 🔗 Figma Resources

**Free Figma Resources:**
- Tajawal Font: https://fonts.google.com/specimen/Tajawal
- Heroicons: https://www.figma.com/community/file/1143911270904274171
- Lucide Icons: https://www.figma.com/community/file/1052616336809334851
- shadcn/ui Figma Kit: https://www.figma.com/community/file/1203061493325953101

**Design Inspiration:**
- Linear App: https://linear.app
- Notion: https://notion.so
- Gusto Payroll: https://gusto.com

---

## 📤 Handoff to Developers

**Export for Development:**
1. Design specs: Measure tool (Ctrl/Cmd + M)
2. CSS code: Copy as CSS
3. Assets: Export 1x, 2x, 3x for mobile
4. Prototype link: Share with "Can view prototype"

**Developer Notes:**
- All measurements in px (will be converted to rem)
- Colors as CSS variables
- Arabic text direction: RTL
- Use Tailwind CSS utility classes
- Mobile-first responsive design

---

**Ready to design in Figma! 🎨**

Use this document as your complete guide to create professional UI designs for the payroll system.