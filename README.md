# EmailGen — 个性化招聘邮件生成器

一个本地运行的 Web 工具，帮助 HR、面试官、内推人快速生成让候选人感受到被认真对待的个性化招聘邮件。

---

## 功能特性

- **智能个性化** — AI 自动引用候选人简历中的具体细节（项目、成就、技能），而非套话模板
- **文件解析** — 上传 PDF、Word (.docx)、图片（JPG/PNG），自动 OCR 提取文本
- **4 种邮件风格** — 专业正式 / 温暖亲切 / 简洁直接 / 讲故事，适配不同岗位文化
- **多语言支持** — 中文、English、日本語、한국어、Français、Español
- **发件人 Profile 管理** — 保存多个发件人身份（HR / 面试官 / 高管 / 内推人），一键切换
- **批量生成** — 一次生成 1~3 封邮件，每封角度各异
- **一键翻译** — 单封翻译或全部批量翻译到目标语言
- **可直接编辑** — 生成结果可在线修改，一键复制标题或全文

---

## 技术栈

| 层 | 技术 |
|---|---|
| 前端 | React 18 + Vite + Tailwind CSS + Zustand + React Query |
| 后端 | Node.js + Express + TypeScript |
| AI | Qwen Plus（文字生成）+ Qwen VL Plus（图片 OCR）via DashScope |
| 存储 | JSON 文件（无数据库依赖） |

---

## 快速开始

### 1. 克隆项目

```bash
git clone https://github.com/hxu911-bot/UNeverAlone-2.0.git
cd UNeverAlone-2.0
```

### 2. 配置环境变量

```bash
cp backend/.env.example backend/.env
```

编辑 `backend/.env`，填入你的 DashScope API Key：

```
DASHSCOPE_API_KEY=sk-your-key-here
```

> 在 [DashScope 控制台](https://dashscope.console.aliyun.com/) 注册并获取 API Key，新用户有免费额度。

### 3. 安装依赖并启动

**终端 1 — 后端（端口 3100）：**
```bash
cd backend
npm install
npm run dev
```

**终端 2 — 前端（端口 5200）：**
```bash
cd frontend
npm install
npm run dev
```

### 4. 打开浏览器

访问 **http://localhost:5200**

---

## 使用流程

```
Step 1: 候选人信息
  ├── 上传简历（PDF / Word / 图片）→ 自动解析
  └── 或直接粘贴候选人背景文字

Step 2: 邮件设置
  ├── 选择发件人 Profile（可新建/编辑）
  ├── 选择邮件风格（4种）
  ├── 选择目标语言
  └── 选择生成数量（1~3封）

Step 3: 生成结果
  ├── 在线编辑邮件内容
  ├── 一键复制标题 / 全文
  └── 翻译（单封或批量）
```

---

## 项目结构

```
email-gen/
├── backend/
│   ├── src/
│   │   ├── modules/
│   │   │   ├── profiles/     # 发件人 Profile CRUD
│   │   │   ├── parse/        # 文件解析（PDF/DOCX/图片OCR）
│   │   │   └── generate/     # AI 邮件生成 & 翻译
│   │   ├── shared/           # Zod 校验 & 错误类
│   │   ├── data/profiles.json  # 本地 Profile 存储
│   │   └── index.ts          # 启动入口（:3100）
│   └── .env.example
└── frontend/
    └── src/
        ├── components/
        │   ├── wizard/       # 三步向导（Step1/2/3 + StepIndicator）
        │   ├── profile/      # Profile 管理抽屉
        │   ├── candidate/    # 文件拖拽上传
        │   └── email/        # 邮件卡片（编辑/复制/翻译）
        ├── store/wizard.ts   # Zustand 向导状态
        ├── api/client.ts     # Axios API 封装
        └── lib/styleConfig.ts  # 风格 & 语言配置
```

---

## API 端点

| 方法 | 路径 | 说明 |
|---|---|---|
| GET | `/health` | 健康检查 |
| GET | `/api/profiles` | 获取所有 Profile |
| POST | `/api/profiles` | 创建 Profile |
| PUT | `/api/profiles/:id` | 更新 Profile |
| DELETE | `/api/profiles/:id` | 删除 Profile |
| POST | `/api/parse` | 解析文件（返回纯文本） |
| POST | `/api/generate` | 生成邮件 |
| POST | `/api/generate/translate` | 翻译邮件 |
