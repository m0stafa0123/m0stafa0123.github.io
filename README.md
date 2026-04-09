# 🌟 GitHub Spec Kit — الدليل الكامل

> دليل شامل لـ Spec-Driven Development باستخدام GitHub Spec Kit
> مكتوب بالعربي من الصفر للي مش عارف عنه حاجة

---

## 📌 الفهرس

- [[#إيه هو Spec-Driven Development؟]]
- [[#المشكلة اللي بيحلها]]
- [[#إيه هو Spec Kit؟]]
- [[#الذاكرة القصيرة vs الذاكرة الطويلة]]
- [[#التثبيت]]
- [[#الـ Commands بالترتيب]]
- [[#الـ Flow الكامل]]
- [[#الـ Extra Files]]
- [[#هيكل المشروع]]
- [[#نصايح لتقليل الـ Tokens]]
- [[#امتى تستخدم Spec Kit وامتى لأ]]

---

## 🧠 إيه هو Spec-Driven Development؟

الـ SDD أو Spec-Driven Development هو أسلوب في البرمجة بتحط فيه كل تفاصيل المشروع في ملفات منظمة **قبل** ما تبدأ الكود.

بدل ما تقول للـ AI "اعملي login screen" وتتفرج على إيه اللي هيطلع — بتكتب spec واضحة وهو بيبني عليها بالظبط.

> الفكرة الأساسية: الـ Specification هي المصدر الحقيقي، والكود هو اللي بيتولد منها.

---

## ❌ المشكلة اللي بيحلها

### مشكلة الـ Vibe Coding
لما بتكتب prompt عشوائي للـ AI:
- بيطلع كود يبان صح بس مش شغال
- مش بيتبع الـ architecture بتاعتك
- كل conversation جديدة بتبدأ من الصفر
- لازم تكرر نفس المعلومات كل مرة

### الحل
تكتب كل المعلومات مرة واحدة في ملفات — والـ AI بيقرأها في كل مرة لوحده.

---

## ✅ إيه هو Spec Kit؟

Spec Kit هو toolkit مفتوح المصدر من GitHub نفسه.

وظيفته الأساسية: **يعمل نظام ذاكرة طويلة للـ AI بتاعك.**

- يشتغل مع: GitHub Copilot، Claude Code، Gemini CLI، Cursor
- مبني على فكرة الـ md files اللي بتتحفظ على الـ disk
- الإصدار الحالي: v0.5.1

---

## 🧠 الذاكرة القصيرة vs الذاكرة الطويلة

| | الذاكرة القصيرة | الذاكرة الطويلة |
|---|---|---|
| **أين؟** | الـ Chat | الـ md files على الـ disk |
| **بتتمسح؟** | آه، في كل conversation | لأ، موجودة دايماً |
| **الـ AI بيقرأها؟** | بس في نفس الـ conversation | في كل مرة لوحده |
| **مثال** | سؤال في ChatGPT | constitution.md |

> **الخلاصة:** الـ md files = ذاكرة دائمة للـ AI — بتكتب المعلومات مرة واحدة وبعدين الـ AI بيرجعلها لوحده في كل task.

---

## 📦 التثبيت

### المتطلبات
- Python مثبت على الجهاز
- Git مثبت
- VS Code مع Copilot extension

### الخطوات

**الخطوة 1 — تثبيت uv**
```bash
pip install uv
```

> uv هو package manager بيشتغل زي pip بس أسرع — بنستخدمه عشان ينزل Spec Kit

**الخطوة 2 — تثبيت Spec Kit**
```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

**الخطوة 3 — تحديث الـ PATH**
```bash
uv tool update-shell
```

> أغلق الـ terminal وافتحه تاني بعد الأمر ده

**الخطوة 4 — التحقق من التثبيت**
```bash
specify --help
```

لو ظهرت قائمة commands — التثبيت تم بنجاح ✅

**الخطوة 5 — إنشاء المشروع**
```bash
specify init اسم_المشروع --ai copilot
```

اختار `ps` لو على Windows، أو `sh` لو على Mac/Linux

---

## ⚡ الـ Commands بالترتيب

### 🏛️ 1. `/speckit.constitution`
**بتستخدمه:** مرة واحدة للمشروع كله
**بيعمل إيه:** بيكتب القواعد الثابتة للمشروع

```
/speckit.constitution
```

بيسألك عن:
- الـ tech stack
- الـ architecture
- الـ theme
- الـ coding rules

**مثال للمحتوى:**
```markdown
- Tech stack: Flutter, Dart, Bloc, GetIt, Dio
- Theme: dark background #121212, primary color #39E07A
- Architecture: Feature-based, Clean Architecture
- State management: Bloc only, no setState
- DI: GetIt service locator
- No hardcoded strings
```

---

### 📋 2. `/speckit.specify`
**بتستخدمه:** مرة للـ app كله + مرة لكل feature
**بيعمل إيه:** بيوصف إيه اللي هتبنيه

**للـ app كله (مرة واحدة):**
```
/speckit.specify
Mala3bna is a sports venue booking app for padel and football 
courts in Egypt. Users can browse courts, book time slots, 
and manage their reservations.
Features: auth, home, court listing, booking flow, profile
```

**لكل feature:**
```
/speckit.specify
Auth feature: login and register screens with token storage,
splash screen routing, and error handling
```

---

### 🗺️ 3. `/speckit.plan`
**بتستخدمه:** لكل feature لوحدها
**بيعمل إيه:** بيعمل technical plan مفصل

```
/speckit.plan
Plan the auth feature for Mala3bna with:
- AuthCubit with sealed states
- AuthRepo registered in GetIt as abstract type
- Login and register API calls using Dio
- Save token using flutter_secure_storage after login
- On app start check token in SplashScreen
- Show loading inside button not full screen
- Show error as snackbar not dialog
- After login navigate to HomeScreen and clear back stack
```

**اللي بيطلع:**
```markdown
## Auth Feature Plan

### Screens
- LoginScreen
- RegisterScreen

### Cubits & States
- AuthCubit
  - AuthInitial
  - AuthLoading
  - AuthSuccess
  - AuthFailure

### Repos
- AuthRepo (abstract)
- AuthRepoImp
  - login(email, password)
  - register(name, email, password, phone)

### Navigation Flow
- App opens → check token
- Token exists → HomeScreen
- No token → LoginScreen
```

---

### ✅ 4. `/speckit.tasks`
**بتستخدمه:** بعد الـ plan مباشرة
**بيعمل إيه:** بيقسم الـ plan لـ tasks صغيرة

```
/speckit.tasks
```

**لو عايز تتحكم في الترتيب:**
```
/speckit.tasks
Split tasks in this order:
1. First create the repo and API layer
2. Then create the Cubit and states
3. Then build the UI
4. Finally connect UI with Cubit
```

---

### 🚀 5. `/speckit.implement`
**بتستخدمه:** بعد الـ tasks
**بيعمل إيه:** بيكتب الكود الفعلي

```
/speckit.implement
```

**لو عايز تحدد أولويات:**
```
/speckit.implement
Start with task 1 only, wait for my approval before continuing
```

**لو عايز تدي UI design:**
```
/speckit.implement
Use this design for the UI:
[صورة الـ screen]
```

---

### 🔍 Optional Commands

| Command | بيعمل إيه | امتى تستخدمه |
|---|---|---|
| `/speckit.clarify` | بيسألك أسئلة توضيحية | قبل الـ plan لو في حاجات مش واضحة |
| `/speckit.analyze` | بيتأكد إن كل الـ artifacts متسقة | بعد الـ tasks وقبل الـ implement |
| `/speckit.checklist` | بيعمل quality checklist | بعد الـ plan للتأكد من الجودة |

---

## 🔄 الـ Flow الكامل

```
المشروع كله (مرة واحدة)
├── /speckit.constitution  ← قواعد ثابتة
└── /speckit.specify       ← وصف الـ app كله

كل feature
├── /speckit.specify       ← وصف الـ feature
├── /speckit.clarify       ← (اختياري) توضيح
├── /speckit.plan          ← تخطيط تقني
├── /speckit.checklist     ← (اختياري) جودة
├── /speckit.tasks         ← تقسيم مهام
├── /speckit.analyze       ← (اختياري) تحقق
└── /speckit.implement     ← كتابة الكود ✅
```

---

## 📁 الـ Extra Files

### `.specify/memory/api.md`
**الهدف:** يعرّف الـ AI بالـ API endpoints عشان ميخمنش

```markdown
## Auth
POST /auth/login → { email, password } → { token }
POST /auth/register → { name, email, password, phone } → { token }

## Courts
GET /courts → { list of courts }
GET /courts/:id → { court details }

## Booking
POST /booking → { court_id, date, time_slot }
GET /booking/my → { list of bookings }
```

> **عام ولا لكل feature؟** اعمل فايل عام واحد فيه كل الـ endpoints — لأن الـ AI ممكن يحتاج endpoint من feature تانية.

---

### `.specify/memory/codebase.md`
**الهدف:** يعرّف الـ AI بالـ codebase الموجود عشان يبني عليه

```markdown
## Folder Structure
lib/
  core/
    di/injection_container.dart
    network/dio_client.dart
    routing/app_routes.dart
  features/
    auth/
      data/repos/
      domain/
      presentation/cubit/

## Existing Packages
- flutter_bloc
- get_it
- dio
- flutter_secure_storage

## Naming Conventions
- Cubits: AuthCubit, BookingCubit
- States: AuthState (sealed)
- Repos: AuthRepo (abstract), AuthRepoImp
```

---

### `designs/` فولدر
**الهدف:** صور الـ UI designs عشان الـ AI يبني عليها

```
designs/
  auth/
    login.png
    register.png
  home/
    home.png
  booking/
    booking.png
```

في الـ plan بتعمل reference:
```markdown
Design reference: ./designs/auth/login.png
```

> **PNG أحسن من HTML** لأن الـ AI بيشوف الصورة مباشرة وبـ tokens أقل

---

## 🗂️ هيكل المشروع الكامل

```
your_project/
├── .specify/
│   ├── memory/
│   │   ├── constitution.md   ← ثوابت المشروع (مرة واحدة)
│   │   ├── spec.md           ← وصف الـ app (مرة واحدة)
│   │   ├── plan.md           ← plan الـ feature الحالية
│   │   ├── tasks.md          ← tasks الـ feature الحالية
│   │   ├── codebase.md       ← الكود الموجود
│   │   └── api.md            ← الـ API endpoints
│   ├── integrations/
│   ├── scripts/
│   └── templates/
├── designs/
│   ├── auth/
│   ├── home/
│   └── booking/
└── lib/                      ← Flutter project
```

---

## ⚡ نصايح لتقليل الـ Tokens وزيادة الـ Performance

**1. كن محدد في الـ plan**
```
❌ plan the auth feature
✅ plan login screen with AuthCubit, sealed states, GetIt registration
```

**2. حط الـ API في `api.md`**
الـ AI مش هيخمن الـ endpoints

**3. حط الـ folder structure في `codebase.md`**
الـ AI مش هيستكشف المشروع لوحده

**4. حط صور الـ UI في `designs/`**
الـ AI مش هيخترع تصميم من عنده

**5. امسح الـ plan والـ tasks بعد كل feature**
وابدأ بـ plan جديد للـ feature الجديدة عشان معلومات قديمة ماتاكلش tokens

---

## 🎯 امتى تستخدم Spec Kit وامتى لأ؟

### استخدم Spec Kit لما:
- مشروع كبير فيه features كتير
- هتشتغل على نفس المشروع لفترة طويلة
- فريق أكتر من شخص
- عايز consistency في كل الكود

### متستخدمش لما:
- feature صغيرة سريعة
- تجربة أو prototype
- سؤال بسيط — اسأل في الـ chat على طول

---

## 💡 VS Code ولا Chat؟

لو مش عندك Sonnet أو Opus في Copilot — الحل الأذكى:

| الخطوة | الأداة |
|---|---|
| constitution + specify + plan | Claude Chat (أحسن موديل) |
| حط الـ output في الـ md files | يدوياً |
| tasks + implement | VS Code Copilot |

كده بتاخد أحسن ما عند Claude وأحسن ما عند Copilot في نفس الوقت 🎯

---

## 🔗 روابط مفيدة

- [GitHub Spec Kit الرسمي](https://github.com/github/spec-kit)
- [GitHub Blog — Spec-Driven Development](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
- [Spec Kit Website](https://speckit.org)

---

> **آخر تحديث:** أبريل 2026
> **الإصدار:** Spec Kit v0.5.1
