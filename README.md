# 员工工作时长管理系统

基于PHP的员工工作时长管理系统，无需数据库，使用JSON文件存储数据。

## 系统要求

- PHP 7.0 或更高版本
- Web服务器（Apache/Nginx）或 PHP内置服务器
- 支持cURL或file\_get\_contents访问外部URL（用于授权验证）

## 安装步骤

1. 将所有文件复制到Web服务器的根目录
2. 确保 `includes/data` 目录有写入权限
3. 通过浏览器访问 `index.php`
4. 首次访问需要输入授权码

## 默认账户

### 超级管理员账户

- 账号：`hcmd`
- 密码：`asdfghjkl`
- 权限：完整管理权限

### 测试员工账户

| 工号  | 姓名 | 部门  | 初始密码   | 首次登录   |
| --- | -- | --- | ------ | ------ |
| 001 | 张三 | 技术部 | 123456 | 需要修改密码 |
| 002 | 李四 | 技术部 | 123456 | 需要修改密码 |
| 003 | 王五 | 市场部 | 123456 | 已修改密码  |

## 功能说明

### 员工前台功能

1. **登录模块**
   - 支持员工账号密码登录
   - 首次登录强制修改密码
   - 密码复杂度要求：至少8位，包含大写字母、小写字母和数字
2. **日历视图**
   - 显示完整日历组件
   - 颜色标记：
     - 🟢 绿色：已完成工作日期
     - 🔵 蓝色：未来排班日期
     - 🟡 黄色：请假日期
     - 🔴 红色：未参与（工作时长为0）
   - 点击日期查看详细排班信息
3. **排班信息展示**
   - 员工姓名、项目名称、主管
   - 工作地点、公司地址
   - 具体工作时间和状态
4. **请假申请**
   - 选择已有排班进行请假申请
   - 请假类型、理由
   - 提交后等待管理员审批
   - 可查看申请状态
5. **工作时长统计**
   - 本月工作时长统计
   - 已确认时长、待确认时长
   - 状态展示
6. **刷新功能**
   - 页面顶部提供刷新按钮

### 管理员后台功能

采用四栏式布局：

1. **排班与时长管理**
   - 工号查询指定员工
   - 设置工作时长和工作时间
   - 支持批量操作与单个操作
   - 排序：待确认 → 已排班 → 已确认（按日期从新到旧）
2. **请假申请管理**
   - 查看所有请假申请
   - 批准/拒绝功能，可添加审批意见
   - 按员工、日期范围筛选
   - 操作后自动刷新页面
3. **员工添加**
   - 工号、姓名、身份证号、手机号、初始密码、部门
   - 表单验证（身份证号、手机号格式）
   - 初始密码需符合安全要求
4. **二级管理员添加**
   - 工号、姓名、身份证号、手机号、初始密码、部门
   - 多选下属员工（仅限同部门员工）
   - 首次登录强制修改密码
   - 二级管理员仅能管理本部门员工

### 安全功能

1. **开发者工具防护**
   - 禁止F12键
   - 禁止Ctrl+Shift+I/J/C
   - 禁止Ctrl+U查看源代码
   - 禁止右键菜单
   - 三次违规自动封禁IP
2. **IP封禁管理**
   - 超级管理员可查看封禁列表
   - 支持手动解封IP
3. **授权验证**
   - 系统启动需要授权码验证
   - 授权服务器：<https://www.xkym.cn/key/{授权码}.php>
   - 返回格式：【正常/封禁】--【有效期】
   - 封禁或过期状态无法使用系统
4. **源代码保护**
   - 尝试查看源代码返回乱码

## 文件结构

```
员工工作时长网站/
├── index.php              # 登录页面
├── dashboard.php          # 员工工作台
├── change_password.php    # 密码修改页面
├── admin.php              # 管理员后台
├── license.php            # 授权码验证页面
├── banned_ips.php         # IP封禁管理页面
├── logout.php             # 退出登录
├── security_check.php     # 安全检查模块
├── .htaccess              # Apache配置
├── includes/
│   ├── config.php         # 配置文件（数据文件初始化）
│   └── functions.php      # 公共函数库
└── includes/data/         # 数据存储目录
    ├── employees.json     # 员工信息
    ├── admins.json        # 二级管理员信息
    ├── schedules.json     # 排班信息
    ├── leaves.json        # 请假申请
    └── license.json       # 授权信息
```

## 快速启动

### 方法1：使用PHP内置服务器

```bash
cd 员工工作时长网站
php -S localhost:8080
```

然后在浏览器中访问：`http://localhost:8080`

### 方法2：使用XAMPP/WampServer

1. 将项目复制到 `htdocs` 目录
2. 启动Apache服务
3. 访问：`http://localhost/员工工作时长网站`

## 数据存储

系统使用JSON文件存储数据，无需配置数据库：

| 文件               | 说明                 |
| ---------------- | ------------------ |
| `employees.json` | 员工信息（工号、姓名、密码、部门等） |
| `admins.json`    | 二级管理员信息（部门、下属员工）   |
| `schedules.json` | 排班信息（日期、项目、时长、状态）  |
| `leaves.json`    | 请假申请（状态、审批意见）      |
| `license.json`   | 授权信息（授权码、有效期）      |

## UI特点

- **毛玻璃效果**：所有界面采用半透明毛玻璃设计
- **响应式布局**：支持手机、平板、电脑访问
- **现代配色**：渐变背景，清晰的视觉层次

## 注意事项

1. 首次使用会自动初始化测试数据
2. 请确保 `includes/data` 目录可写
3. 生产环境请修改管理员默认密码
4. 建议定期备份 `includes/data` 目录
5. 授权码需从官方渠道获取
6. 二级管理员仅能管理本部门员工
7. 员工请假后对应排班状态变为"请假中"
8. 管理员设置工作时长为0小时标记为"未参与"

******.htaccess文件请填写如下
# 禁止访问敏感文件
<FilesMatch "\.(json|log|md)$">
    Order Allow,Deny
    Deny from all
</FilesMatch>

# 禁止访问includes目录
<FilesMatch "includes/">
    Order Allow,Deny
    Deny from all
</FilesMatch>

# 禁止访问security_check.php直接
<Files "security_check.php">
    Order Allow,Deny
    Deny from all
</Files>

# 禁止访问客户端安全文件
<FilesMatch "_sec_.*\.php$">
    Order Allow,Deny
    Deny from all
</FilesMatch>

# 禁止目录列表
Options -Indexes

# 防止PHP文件被下载
<FilesMatch "\.php$">
    <IfModule mod_php5.c>
        php_flag engine on
    </IfModule>
    <IfModule mod_php7.c>
        php_flag engine on
    </IfModule>
</FilesMatch>

# 设置默认字符集
AddDefaultCharset UTF-8

# 防止文件执行
<FilesMatch "\.(bak|config|sql|fla|psd|ini|log|sh|inc|swp|dist)$">
    Order Allow,Deny
    Deny from all
</FilesMatch>