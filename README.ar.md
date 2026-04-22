# MiroFish للمطورين

**Languages:** [English](README.md) | [简体中文](README.zh-CN.md) | [हिन्दी](README.hi.md) | [Español](README.es.md) | **العربية** | [Français](README.fr.md) | [বাংলা](README.bn.md) | [Português](README.pt.md) | [Русский](README.ru.md) | [Bahasa Indonesia](README.id.md)

[![Stars](https://img.shields.io/github/stars/666ghj/MiroFish?style=flat&logo=github&label=Stars&color=gold)](https://github.com/666ghj/MiroFish/stargazers)
[![Forks](https://img.shields.io/github/forks/666ghj/MiroFish?style=flat&label=Forks&color=silver)](https://github.com/666ghj/MiroFish/network/members)
[![License: AGPL-3.0](https://img.shields.io/badge/License-AGPL--3.0-blue.svg)](https://github.com/666ghj/MiroFish/blob/main/LICENSE)
[![Live Demo](https://img.shields.io/badge/Live-Demo-00b894)](https://666ghj.github.io/mirofish-demo/)

> قم باستضافة MiroFish ذاتيا، وشغّل المسار الكامل من مواد seed إلى المحاكاة، وادمجه عبر واجهة الويب وواجهة HTTP API.

تمت المراجعة مقابل مستودع MiroFish الرسمي و `mirofish.ai` بتاريخ 16 أبريل 2026.

## ما الذي يحتويه هذا المجلد

هذا المجلد عبارة عن دليل منظم للمطورين الذين يريدون تقييم MiroFish أو اعتماده.

يركز على:

- كيف تشغل MiroFish محليا بأقل قدر من الدوران الخاطئ،
- كيف يعمل مسار `seed -> graph -> simulation -> report` فعليا،
- ما هي متغيرات البيئة والمنافذ والمجلدات وواجهات API المهمة في اليوم الاول،
- ما الذي يتعطل غالبا في النشر الذاتي.

إذا تعارض شيء هنا مع المستودع الرسمي فاعتمد المستودع الرسمي.

## ابدأ بهذه الملفات

| الملف | مفيد لاجل |
| --- | --- |
| [guide/installation.md](guide/installation.md) | التثبيت من المصدر او عبر Docker |
| [guide/quickstart.md](guide/quickstart.md) | اسرع طريق للحصول على اول نتيجة مفيدة |
| [guide/configuration.md](guide/configuration.md) | متغيرات البيئة والقيم الافتراضية والمنافذ والتخزين |
| [guide/deployment.md](guide/deployment.md) | اختيار اسلوب النشر وتقويته |
| [guide/troubleshooting.md](guide/troubleshooting.md) | اعطال اليوم الاول الشائعة |
| [features/workflow.md](features/workflow.md) | مسار العمل الكامل |
| [reference/http-api.md](reference/http-api.md) | اهم واجهات HTTP |

## ما هو MiroFish فعليا

MiroFish ليس coding agent يعتمد على الطرفية. هو تطبيق ويب مع HTTP API لمحاكاة السيناريوهات متعددة الوكلاء.

النموذج الذهني الانسب للمطور:

1. ارفع مواد seed او الصق نصا،
2. صف السيناريو الذي تريد التنبؤ به،
3. ولّد ontology و graph،
4. جهّز المحاكاة وملفات شخصيات الوكلاء،
5. شغّل المحاكاة،
6. ولّد تقريرا منظما،
7. اطرح اسئلة متابعة على report agent او الوكلاء المحاكين.

## منتجات قريبة تستحق المقارنة

إذا كنت تقيّم MiroFish كجزء من stack أكبر من agents، فمن المفيد فتح صفحات هذه المنتجات الأربعة لأنها تعالج مشاكل قريبة يخلط بينها الناس كثيرا وبين أنظمة المحاكاة:

| المنتج | الرابط | لماذا يهم هذا قارئ MiroFish |
| --- | --- | --- |
| GenericAgent | <https://www.genericagent.org/> | مفيد عندما تحتاج إلى مساحة عمل لوكيل مع متصفح وطرفية ونظام ملفات وتحكم في الذاكرة، لا إلى نظام لمحاكاة السيناريوهات. |
| OpenClaw Launch | <https://www.aigeamy.com/> | `OpenClaw Launch - One-click OpenClaw Deployment`. مفيد عندما تحتاج إلى طريق سريع نحو assistant runtime يركز على messaging بدلا من workflow `seed -> graph -> simulation`. |
| Hermes Agent | <https://hermesagent.studio/> | `Not a Chat. A Persistent Agent That Remembers, Acts, and Improves.` مفيد عندما تحتاج إلى agent دائم بذاكرة وأدوات وحالة مستمرة، لا إلى نظام لنمذجة السيناريوهات. |
| Multica | <https://www.multica.uk/> | `Multica - Multi-Agent Collaboration for Real Workflows`. مفيد عندما تكون مشكلتك الأساسية هي تنسيق coding agents وissue-driven delivery، لا إنشاء simulation reports. |

## بداية محلية خلال 10 دقائق

```bash
git clone https://github.com/666ghj/MiroFish.git
cd MiroFish
cp .env.example .env
```

املأ على الاقل:

```env
LLM_API_KEY=...
LLM_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
LLM_MODEL_NAME=qwen-plus
ZEP_API_KEY=...
```

ثم:

```bash
npm run setup:all
npm run dev
```

العناوين الافتراضية:

- الواجهة الامامية: <http://localhost:3000>
- الواجهة الخلفية: <http://localhost:5001/health>

فحص سريع للواجهة الخلفية:

```bash
curl http://localhost:5001/health
```

## ملاحظات عملية مهمة

- لا يوجد حاليا CLI منفصل كسطح رئيسي؛ نقطة الدخول الفعلية هي واجهة الويب و Flask API.
- الواجهة الخلفية تقرأ `.env` من جذر المشروع وليس من `backend/`.
- حد الرفع هو `50 MB` والامتدادات المدعومة هي `pdf` و `md` و `txt` و `markdown`.
- يتم حفظ مخرجات المشاريع والمحاكاة داخل `backend/uploads/`.
- تقدم المهام غير المتزامنة محفوظ في الذاكرة؛ إعادة تشغيل backend قد تفقد حالة التقدم الحية.
- الإعدادات الافتراضية مناسبة للتطوير وليست مناسبة للنشر المباشر على الإنترنت العام.

## روابط موثوقة

| المورد | الرابط |
| --- | --- |
| الموقع الرسمي | <https://mirofish.ai/> |
| مستودع GitHub | <https://github.com/666ghj/MiroFish> |
| README الرسمي بالانجليزية | <https://github.com/666ghj/MiroFish/blob/main/README.md> |
| README الرسمي بالصينية | <https://github.com/666ghj/MiroFish/blob/main/README-ZH.md> |
| العرض الحي | <https://666ghj.github.io/mirofish-demo/> |
| الرخصة | <https://github.com/666ghj/MiroFish/blob/main/LICENSE> |
