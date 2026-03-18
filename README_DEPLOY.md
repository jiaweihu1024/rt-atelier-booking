# RT Atelier 舞蹈课程预订应用部署指南

本指南将帮助您将RT Atelier舞蹈课程预订应用部署为一个可以供多人使用的在线应用。

## 目录
1. [部署概述](#部署概述)
2. [准备工作](#准备工作)
3. [后端服务设置](#后端服务设置)
4. [静态网站托管](#静态网站托管)
5. [整合前后端](#整合前后端)
6. [添加用户认证](#添加用户认证)
7. [设置数据库](#设置数据库)
8. [部署后的维护](#部署后的维护)

## 部署概述

要将应用变为多人可用的在线应用，我们需要：
1. 一个后端服务来存储数据和处理用户认证
2. 一个静态网站托管服务来托管前端代码
3. 将两者整合起来，使应用能够正常工作

## 准备工作

### 步骤 1: 创建项目仓库

1. 在代码托管平台创建一个新的仓库
2. 将您的项目代码上传到这个仓库

```bash
# 初始化git仓库
git init

# 添加所有文件
git add .

# 提交更改
git commit -m "初始提交"

# 添加远程仓库
git remote add origin 您的仓库URL

# 推送到远程仓库
git push -u origin main
```

### 步骤 2: 准备项目文件

确保您的项目结构清晰，包含以下文件：
- `index.html`: 主页面
- `assets/`: 资源文件夹
  - `js/`: JavaScript文件
  - `css/`: CSS文件
  - `img/`: 图片资源

## 后端服务设置

### 步骤 1: 注册并创建项目

1. 注册一个账号
2. 创建一个新的项目
3. 记录项目配置信息

### 步骤 2: 设置数据库

1. 在控制台中创建一个实时数据库
2. 设置适当的安全规则
3. 导入初始数据结构

### 步骤 3: 配置用户认证

1. 启用电子邮件/密码登录方式
2. 配置授权域名
3. 设置重定向URL

## 静态网站托管

### 步骤 1: 注册并连接仓库

1. 注册一个账号
2. 连接您的代码仓库
3. 配置构建设置

### 步骤 2: 配置环境变量

1. 在设置中添加环境变量
2. 包含您的后端服务配置信息
3. 保存配置

### 步骤 3: 部署网站

1. 点击部署按钮
2. 等待部署完成
3. 获取部署后的URL

## 整合前后端

### 步骤 1: 添加SDK到项目

在`index.html`文件中添加相关SDK：

```html
<!-- 在<head>部分添加 -->
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js"></script>
```

### 步骤 2: 初始化连接

在JavaScript文件中添加初始化代码：

```javascript
// 配置
const config = {
  apiKey: "您的API密钥",
  authDomain: "您的认证域名",
  databaseURL: "您的数据库URL",
  projectId: "您的项目ID",
  storageBucket: "您的存储桶",
  messagingSenderId: "您的消息发送者ID",
  appId: "您的应用ID"
};

// 初始化
firebase.initializeApp(config);
const auth = firebase.auth();
const database = firebase.database();
```

## 添加用户认证

### 步骤 1: 创建登录界面

在`index.html`中添加登录和注册模态框：

```html
<!-- 登录模态框 -->
<div id="login-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
  <div class="bg-white rounded-lg shadow-lg w-full max-w-md mx-4">
    <div class="p-4 border-b flex justify-between items-center">
      <h3 class="text-lg font-semibold text-primary">登录</h3>
      <button id="close-login-modal" class="text-gray-500 hover:text-gray-700">
        <i class="fa fa-times"></i>
      </button>
    </div>
    <div class="p-4">
      <form id="login-form">
        <div class="mb-4">
          <label class="block text-sm font-medium text-gray-700 mb-1">电子邮箱</label>
          <input type="email" id="login-email" class="w-full border rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-secondary" required>
        </div>
        <div class="mb-4">
          <label class="block text-sm font-medium text-gray-700 mb-1">密码</label>
          <input type="password" id="login-password" class="w-full border rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-secondary" required>
        </div>
        <div class="flex justify-end space-x-3">
          <button type="button" id="cancel-login" class="px-4 py-2 border rounded-lg text-gray-700 hover:bg-gray-100">取消</button>
          <button type="submit" class="btn-primary">登录</button>
        </div>
      </form>
    </div>
  </div>
</div>
```

### 步骤 2: 添加认证逻辑

在JavaScript文件中添加认证逻辑：

```javascript
// 登录功能
const loginForm = document.getElementById('login-form');
loginForm.addEventListener('submit', (e) => {
  e.preventDefault();
  const email = document.getElementById('login-email').value;
  const password = document.getElementById('login-password').value;
  
  auth.signInWithEmailAndPassword(email, password)
    .then((userCredential) => {
      // 登录成功
      const user = userCredential.user;
      console.log('登录成功:', user);
      document.getElementById('login-modal').classList.add('hidden');
      showSuccessMessage('登录成功!');
      updateUserUI(user);
    })
    .catch((error) => {
      // 登录失败
      console.error('登录失败:', error.message);
      alert('登录失败: ' + error.message);
    });
});

// 监听用户认证状态变化
auth.onAuthStateChanged((user) => {
  if (user) {
    // 用户已登录
    console.log('用户已登录:', user);
    updateUserUI(user);
  } else {
    // 用户未登录
    console.log('用户未登录');
    updateUserUI(null);
  }
});
```

## 设置数据库

### 步骤 1: 设计数据结构

```
RT-Atelier/
  ├── courses/
  │   ├── course1/
  │   │   ├── name: "芭蕾基础班"
  │   │   ├── type: "ballet"
  │   │   ├── description: "适合初学者的芭蕾舞基础课程"
  │   │   └── ...
  │   └── ...
  ├── bookings/
  │   ├── booking1/
  │   │   ├── userId: "user123"
  │   │   ├── courseId: "course1"
  │   │   └── ...
  │   └── ...
  └── users/
      ├── user123/
      │   ├── name: "张三"
      │   ├── email: "zhangsan@example.com"
      │   └── ...
      └── ...
```

### 步骤 2: 添加数据库操作代码

```javascript
// 获取课程数据
function fetchCourses() {
  database.ref('courses').once('value')
    .then((snapshot) => {
      const coursesData = snapshot.val();
      if (coursesData) {
        renderCourses(Object.values(coursesData));
      }
    })
    .catch((error) => {
      console.error('获取课程失败:', error);
    });
}

// 创建新预订
function createBooking(bookingData) {
  const user = auth.currentUser;
  if (!user) {
    alert('请先登录');
    return;
  }
  
  bookingData.userId = user.uid;
  
  database.ref('bookings').push(bookingData)
    .then(() => {
      console.log('预订成功');
      showSuccessMessage('预订成功!');
    })
    .catch((error) => {
      console.error('预订失败:', error);
      alert('预订失败: ' + error.message);
    });
}
```

## 部署后的维护

### 更新应用

1. 推送代码到仓库
2. 自动触发部署
3. 验证更新是否成功

### 监控和分析

1. 设置分析工具
2. 监控应用性能
3. 收集用户反馈

### 安全考虑

1. 定期更新依赖包
2. 审查安全规则
3. 实施访问限制
4. 定期备份数据

## 进一步改进

1. 添加推送通知功能
2. 实现课程评价系统
3. 开发管理员控制面板
4. 优化移动端体验

---

通过以上步骤，您可以成功将RT Atelier舞蹈课程预订应用部署为一个功能完整的在线应用。如果您在部署过程中遇到任何问题，请参考相关平台的官方文档或寻求专业帮助。