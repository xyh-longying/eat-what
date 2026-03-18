# 今天吃什么？ - Eat What

## 项目介绍
一个帮助你决定今天吃什么的智能Web应用，支持随机选择、菜品管理、附近美食推荐和AI助手功能。

## 功能特性

### 1. 随机选择美食
- 支持按心情筛选（随便、想吃饭、想吃面、想吃小吃、想喝汤、西餐）
- 动态抽奖动画效果
- 详细的结果展示（包含评分、价格、标签等信息）
- 天气和节日影响推荐

### 2. 菜品管理
- 添加、编辑、删除菜品
- 支持搜索和分类筛选
- 详细的菜品信息（名称、分类、价格、用餐方式、评分、图标、店铺、简介、标签）

### 3. 附近美食
- 通过高德地图API获取附近餐饮信息
- 支持将附近美食加入随机池
- 实时位置定位

### 4. AI 美食助手
- 基于大模型的智能对话
- 提供美食推荐和建议
- 支持自定义API配置

### 5. 设置
- AI大模型配置（API地址、API Key、模型名称）
- 智能推荐设置（天气影响、节日影响）
- 历史记录管理

## 技术栈

- **前端**：纯HTML5 + CSS3 + JavaScript
- **地图服务**：高德地图API
- **数据存储**：Supabase
- **AI服务**：支持DeepSeek等大模型

## 项目结构

```
├── index.html          # 主页面
└── README.md           # 项目说明文档
```

## 快速开始

### 1. 克隆项目

```bash
git clone <repository-url>
cd eat-what
```

### 2. 配置API Keys

创建 `.env` 文件并配置以下API密钥：

```
# API Keys
AMAP_KEY=your_amap_api_key
SUPABASE_URL=your_supabase_url
SUPABASE_KEY=your_supabase_api_key
```

#### 2.1 获取高德地图API Key

1. 访问 [高德开放平台](https://lbs.amap.com/)
2. 注册/登录账号
3. 进入控制台，点击「应用管理」→「我的应用」→「创建新应用」
4. 填写应用名称，选择应用类型（选择「Web端(JS API)」）
5. 创建完成后，点击「添加Key」
6. 选择「Web服务」，输入Key名称，提交后即可获得API Key

#### 2.2 获取Supabase数据库API Key

1. 访问 [Supabase官网](https://supabase.com/)
2. 点击「Start your project」或「Sign In」登录
3. 点击「New Project」创建新项目
4. 填写项目信息：
   - **Name**：项目名称（如：eat-what）
   - **Database Password**：设置数据库密码（请妥善保存）
   - **Region**：选择离你最近的区域（如：Northeast Asia (Tokyo)）
5. 点击「Create new project」，等待项目创建完成（通常需要1-2分钟）
6. 项目创建完成后，进入项目仪表板
7. 点击左侧菜单的「Settings」→「API」
8. 复制以下信息到 `.env` 文件：
   - **Project URL**：对应 `SUPABASE_URL`
   - **anon public** key：对应 `SUPABASE_KEY`

#### 2.3 创建数据库表

在Supabase项目中创建以下表：

1. **foods 表**（存储菜品信息）
   - 点击左侧菜单的「Table Editor」→「Create a new table」
   - 表名：`foods`
   - 添加以下列：
     - `id`：int8, primary key
     - `name`：text, not null
     - `cat`：text（分类）
     - `emoji`：text
     - `description`：text
     - `tags`：text[]（数组）
     - `price`：numeric
     - `type`：text（用餐方式：dine-in/delivery）
     - `rating`：int（评分1-5）
     - `eat_count`：int（食用次数）
     - `shop`：text（店铺名称）
     - `created_at`：timestamptz, default: now()
     - `updated_at`：timestamptz

2. **settings 表**（存储用户设置）
   - 表名：`settings`
   - 添加以下列：
     - `id`：int8, primary key, default: 1
     - `weather_on`：boolean, default: true
     - `festival_on`：boolean, default: true
     - `nearby_on`：boolean, default: true
     - `ai_url`：text
     - `ai_key`：text
     - `ai_model`：text, default: 'deepseek-chat'
     - `created_at`：timestamptz, default: now()
     - `updated_at`：timestamptz

3. **history 表**（存储历史记录）
   - 表名：`history`
   - 添加以下列：
     - `id`：int8, primary key
     - `food_name`：text
     - `food_emoji`：text
     - `eaten_at`：timestamptz, default: now()

#### 2.4 配置大模型API

大模型配置不需要在 `.env` 文件中设置，用户可以在应用的「设置」页面手动配置：

1. 打开应用，进入「设置」页面
2. 在「AI大模型配置」部分填写：
   - **API地址**：如 `https://api.deepseek.com`
   - **API Key**：你的大模型API密钥
   - **模型名称**：如 `deepseek-chat`
3. 点击保存，配置会自动存储到数据库中

支持的大模型包括：
- DeepSeek
- OpenAI (GPT系列)
- 其他兼容OpenAI API格式的模型

注意：`.env` 文件已添加到 `.gitignore` 中，不会被提交到版本控制系统。

### 3. 启动应用

直接在浏览器中打开 `index.html` 文件即可开始使用。

## 核心功能说明

### 随机选择机制
1. 根据用户选择的心情筛选菜品
2. 考虑天气和节日因素进行智能推荐
3. 随机选择符合条件的菜品
4. 展示详细信息并记录历史

### 附近美食获取
1. 通过IP定位获取大致位置
2. 尝试使用浏览器地理位置获取精确位置
3. 调用高德地图API获取附近餐饮POI
4. 支持将附近美食添加到随机池中

### AI助手
1. 支持配置不同的大模型API
2. 基于用户输入提供智能美食建议
3. 记录对话历史

## 数据存储

项目使用Supabase进行数据存储，主要包含以下表：

- **foods**：存储菜品信息
- **settings**：存储用户设置
- **history**：存储历史记录

## 浏览器兼容性

- 支持所有现代浏览器（Chrome、Firefox、Safari、Edge）
- 响应式设计，适配移动端和桌面端

## 未来规划

- [ ] 添加用户账号系统
- [ ] 支持多人共享菜单
- [ ] 增加更多智能推荐算法
- [ ] 开发移动应用版本
- [ ] 集成更多地图和AI服务

## 贡献

欢迎提交Issue和Pull Request来改进这个项目！

## 许可证

MIT License

## 联系方式

- 项目地址：https://github.com/xyh-longying/eat-what
- 作者：xyh_longying
- 日期：2026-03-18