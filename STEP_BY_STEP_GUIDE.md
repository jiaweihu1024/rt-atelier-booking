# RT Atelier 舞蹈课程预订应用 - 详细部署指南

## 前言

本指南将提供**非常详细的步骤**，帮助您轻松部署RT Atelier舞蹈课程预订应用。每个步骤都配有清晰的说明和图片指引，确保您能顺利完成部署。

## 目录
1. [获取项目文件 (5分钟)](#获取项目文件-5分钟)
2. [创建GitHub仓库 (10分钟)](#创建github仓库-10分钟)
3. [设置Firebase后端 (20分钟)](#设置firebase后端-20分钟)
4. [部署到Vercel (10分钟)](#部署到vercel-10分钟)
5. [完成设置 (5分钟)](#完成设置-5分钟)
6. [验证部署 (5分钟)](#验证部署-5分钟)

## 获取项目文件 (5分钟)

### 步骤 1: 下载项目文件

1. 在当前页面，您应该能看到一个"下载项目"或类似的按钮
2. 点击该按钮下载项目文件的ZIP压缩包
3. 下载完成后，找到下载的ZIP文件（通常在您的"下载"文件夹中）

### 步骤 2: 解压项目文件

1. 右键点击下载的ZIP文件
2. 选择"解压到当前文件夹"或类似选项
3. 解压后，您将看到一个名为`rt-atelier`的文件夹，其中包含所有项目文件

## 创建GitHub仓库 (10分钟)

### 步骤 1: 注册GitHub账号（如果您还没有）

1. 打开浏览器，访问 [GitHub官网](https://github.com)
2. 点击右上角的"Sign up"按钮
3. 按照提示填写您的用户名、邮箱和密码
4. 完成注册并登录您的新账号

### 步骤 2: 创建新的GitHub仓库

1. 登录GitHub后，点击右上角的"+"图标，然后选择"New repository"
2. 在"Repository name"字段中输入：`rt-atelier-booking`
3. 在"Description"字段中输入：`RT Atelier舞蹈课程预订应用`
4. 确保选择"Public"（公开）选项
5. 勾选"Add a README file"选项
6. 最后点击"Create repository"按钮

### 步骤 3: 上传项目文件到GitHub

1. 创建仓库后，您会看到仓库的主页面
2. 点击页面上方的"Add file"按钮，然后选择"Upload files"
3. 在出现的页面中，您会看到一个上传区域，上面写着"Drag files here to add them to your repository"
4. 找到您之前解压的`rt-atelier`文件夹
5. 打开该文件夹，选择所有文件（按Ctrl+A或Command+A）
6. 将选中的文件拖拽到GitHub的上传区域
7. 等待文件上传完成（这可能需要一些时间，取决于文件大小和您的网络速度）
8. 上传完成后，滚动到页面底部
9. 在"Commit changes"部分，输入提交信息，如"上传RT Atelier项目文件"
10. 点击"Commit changes"按钮完成上传

## 设置Firebase后端 (20分钟)

### 步骤 1: 创建Firebase账号（如果您还没有）

1. 打开浏览器，访问 [Firebase官网](https://firebase.google.com)
2. 点击右上角的"Get started"按钮
3. 使用您的Google账号登录（如果没有Google账号，您需要先创建一个）

### 步骤 2: 创建Firebase项目

1. 登录后，您会看到Firebase控制台
2. 点击"Add project"或"创建项目"按钮
3. 在"Project name"字段中输入：`RT-Atelier`
4. 点击"Continue"（继续）
5. 在接下来的页面，您可以选择是否启用Google Analytics，建议选择"Don't set up Analytics for now"（暂时不设置Analytics）
6. 点击"Create project"（创建项目）按钮
7. 等待项目创建完成，然后点击"Continue"（继续）按钮

### 步骤 3: 设置Firebase实时数据库

1. 进入项目后，在左侧菜单中点击"Build"（构建），然后选择"Realtime Database"（实时数据库）
2. 点击"Create Database"（创建数据库）按钮
3. 在弹出的窗口中，选择一个地理位置（建议选择离您最近的位置，如"asia-east1"）
4. 选择"Start in test mode"（以测试模式启动）选项
5. 点击"Enable"（启用）按钮

### 步骤 4: 导入初始数据到Firebase

1. 数据库创建成功后，您会看到数据库的主页面
2. 点击右上角的三个点图标（更多选项），然后选择"Import JSON"（导入JSON）
3. 现在，您需要创建一个包含初始数据的JSON文件
4. 打开记事本或任何文本编辑器
5. 复制以下JSON内容并粘贴到文本编辑器中：

```json
{
  "courses": {
    "course1": {
      "capacity": 15,
      "currentBookings": 8,
      "description": "适合初学者的芭蕾舞基础课程",
      "instructor": "张老师",
      "level": "初级",
      "name": "芭蕾基础班",
      "schedule": [
        {"day": 1, "time": "18:00-19:30"},
        {"day": 3, "time": "18:00-19:30"}
      ],
      "type": "ballet"
    },
    "course2": {
      "capacity": 12,
      "currentBookings": 12,
      "description": "适合有一定基础的学员",
      "instructor": "李老师",
      "level": "中级",
      "name": "现代舞进阶班",
      "schedule": [
        {"day": 2, "time": "20:00-21:30"},
        {"day": 4, "time": "20:00-21:30"}
      ],
      "type": "modern"
    },
    "course3": {
      "capacity": 20,
      "currentBookings": 15,
      "description": "轻松愉快的爵士舞课程",
      "instructor": "王老师",
      "level": "所有级别",
      "name": "爵士舞周末班",
      "schedule": [
        {"day": 6, "time": "14:00-16:00"}
      ],
      "type": "jazz"
    },
    "course4": {
      "capacity": 10,
      "currentBookings": 6,
      "description": "探索当代舞蹈的多样性",
      "instructor": "陈老师",
      "level": "中级",
      "name": "当代舞探索班",
      "schedule": [
        {"day": 5, "time": "19:00-20:30"}
      ],
      "type": "contemporary"
    },
    "course5": {
      "capacity": 15,
      "currentBookings": 10,
      "description": "学习拉丁舞的基本步法和技巧",
      "instructor": "刘老师",
      "level": "初级",
      "name": "拉丁舞初级班",
      "schedule": [
        {"day": 6, "time": "10:00-11:30"}
      ],
      "type": "latin"
    },
    "course6": {
      "capacity": 18,
      "currentBookings": 12,
      "description": "学习街舞的基本动作和组合",
      "instructor": "赵老师",
      "level": "初级",
      "name": "街舞基础班",
      "schedule": [
        {"day": 0, "time": "15:00-16:30"}
      ],
      "type": "hiphop"
    }
  },
  "settings": {
    "siteName": "RT Atelier",
    "defaultLanguage": "zh-CN",
    "allowRegistrations": true
  }
}
```

6. 点击"文件" > "另存为"
7. 将文件命名为：`initial-data.json`
8. 确保"保存类型"选择为"所有文件"
9. 点击"保存"按钮
10. 回到Firebase控制台，点击"浏览"按钮，找到并选择您刚刚创建的`initial-data.json`文件
11. 点击"Import"（导入）按钮，等待导入完成

### 步骤 5: 获取Firebase配置信息

1. 在Firebase控制台中，点击左上角的齿轮图标，然后选择"Project settings"（项目设置）
2. 向下滚动到"Your apps"（您的应用）部分
3. 点击`</>`图标（这是Web应用的图标）
4. 在"App nickname"（应用昵称）字段中输入：`RT Atelier Web`
5. 不需要勾选"Also set up Firebase Hosting"（也设置Firebase Hosting）选项
6. 点击"Register app"（注册应用）按钮
7. 您将看到一段JavaScript代码，其中包含您的Firebase配置信息
8. 复制这段代码中的配置对象（从`const firebaseConfig = {`到`};`）
9. 将这段代码保存到一个安全的地方，我们稍后会用到它

## 部署到Vercel (10分钟)

### 步骤 1: 创建Vercel账号（如果您还没有）

1. 打开浏览器，访问 [Vercel官网](https://vercel.com)
2. 点击右上角的"Sign Up"按钮
3. 选择使用GitHub账号登录（这是最简单的方式，因为我们已经有了GitHub仓库）
4. 按照提示完成授权和注册过程

### 步骤 2: 导入GitHub仓库到Vercel

1. 登录Vercel后，您会看到Vercel的欢迎页面
2. 点击"Add New..."按钮，然后选择"Project"（项目）
3. 在"Import Git Repository"（导入Git仓库）部分，您应该能看到您的GitHub用户名和仓库列表
4. 找到并选择`rt-atelier-booking`仓库
5. 点击"Import"（导入）按钮

### 步骤 3: 配置环境变量

1. 导入仓库后，您会进入项目配置页面
2. 向下滚动到"Environment Variables"（环境变量）部分
3. 现在，我们需要添加Firebase配置作为环境变量
4. 从您之前保存的Firebase配置中，提取各个值，并添加以下环境变量：

| 名称 | 值（从Firebase配置中提取） |
|------|--------------------------|
| FIREBASE_API_KEY | apiKey字段的值 |
| FIREBASE_AUTH_DOMAIN | authDomain字段的值 |
| FIREBASE_DATABASE_URL | databaseURL字段的值 |
| FIREBASE_PROJECT_ID | projectId字段的值 |
| FIREBASE_STORAGE_BUCKET | storageBucket字段的值 |
| FIREBASE_MESSAGING_SENDER_ID | messagingSenderId字段的值 |
| FIREBASE_APP_ID | appId字段的值 |

5. 对于每个环境变量，点击"Add"按钮添加
6. 添加完所有环境变量后，点击页面底部的"Deploy"（部署）按钮

### 步骤 4: 等待部署完成

1. 点击部署按钮后，Vercel将开始构建和部署您的应用
2. 部署过程通常需要1-3分钟
3. 部署完成后，您会看到一个成功页面，显示"Your deployment is live"（您的部署已上线）
4. 复制显示的URL（例如：`rt-atelier-booking.vercel.app`），这是您应用的访问地址

## 完成设置 (5分钟)

### 步骤 1: 更新Firebase授权域名

1. 返回Firebase控制台
2. 在左侧菜单中点击"Authentication"（认证）
3. 点击"Settings"（设置）选项卡
4. 向下滚动到"Authorized domains"（授权域名）部分
5. 点击"Add domain"（添加域名）按钮
6. 输入您的Vercel域名（例如：`rt-atelier-booking.vercel.app`）
7. 点击"Add"（添加）按钮

## 验证部署 (5分钟)

### 步骤 1: 访问您的应用

1. 打开浏览器，输入您的Vercel域名
2. 您应该能看到RT Atelier应用的登录页面
3. 尝试浏览应用的不同页面，确保一切正常工作

### 步骤 2: 测试核心功能

1. 尝试切换语言（点击右上角的"语言"按钮）
2. 查看课程列表和日历视图
3. 尝试预订一门课程
4. 检查个人中心功能

如果所有功能都正常工作，恭喜您！您已成功部署了RT Atelier舞蹈课程预订应用。

## 常见问题解答

### Q: 我找不到下载项目文件的按钮怎么办？
A: 如果您在当前页面找不到下载按钮，可能是因为您需要先完成某些操作。请联系系统管理员或技术支持获取项目文件。

### Q: 上传项目文件到GitHub时遇到问题怎么办？
A: 如果上传失败，可能是因为文件太大或网络问题。您可以尝试分批上传文件，或者使用Git命令行工具上传。

### Q: 导入JSON数据到Firebase时出错怎么办？
A: 请确保您的JSON格式正确，没有语法错误。您可以使用[JSON验证工具](https://jsonlint.com/)检查JSON格式。

### Q: Vercel部署失败怎么办？
A: 检查部署日志以了解具体错误。常见问题包括环境变量设置不正确或构建脚本错误。

### Q: 应用能正常访问但无法连接数据库怎么办？
A: 检查Firebase配置是否正确，特别是databaseURL。同时确保Firebase数据库规则允许读取和写入。

---

恭喜您完成了RT Atelier舞蹈课程预订应用的部署！如果您在部署过程中遇到任何问题，请参考上述常见问题解答，或联系专业技术人员获取帮助。