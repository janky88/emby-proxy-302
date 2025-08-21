# 🎬 Emby代理管理系统 (emby-proxy-tdr)

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python" alt="Python Version">
  <img src="https://img.shields.io/badge/Docker-Ready-blue?style=for-the-badge&logo=docker" alt="Docker Ready">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License">
  <img src="https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey?style=for-the-badge" alt="Platform">
</p>

<p align="center">
  <strong>一个现代化的Emby媒体代理管理平台，提供界面化配置、多实例管理、115网盘集成等功能</strong>
</p>

---

## 🌟 项目特色

### ✨ **核心功能**
- 🎨 **现代化UI设计** - 精美的Web界面，支持响应式布局
- 🚀 **多实例管理** - 支持创建和管理多个代理实例
- 🔐 **用户权限系统** - 基于角色的访问控制
- 📁 **115网盘集成** - 无缝集成115网盘直链播放
- ⚡ **实时监控** - 实例状态和系统资源实时监控
- 🔄 **路径映射** - 灵活的本地/远程路径映射配置

### 🏗️ **技术架构**
- **后端**: Flask + SQLAlchemy + Gevent
- **前端**: 现代CSS + Vanilla JavaScript
- **数据库**: SQLite (支持其他数据库)
- **部署**: Docker + Docker Compose
- **安全**: BCrypt密码加密 + CSRF防护

## 🐳 Docker 快速部署 (推荐)

### 📋 系统要求
- Docker 20.10+
- Docker Compose 2.0+
- 2GB+ 可用内存
- 1GB+ 可用磁盘空间

### 🚀 一键部署

1. **创建项目目录**
```bash
mkdir emby-proxy-tdr && cd emby-proxy-tdr
```

2. **下载Docker Compose配置**
```bash
wget https://raw.githubusercontent.com/your-username/emby-proxy-tdr/main/docker-compose.yml
```

3. **启动服务**
```bash
docker-compose up -d
```

4. **访问管理界面**
- 地址: http://localhost:11089
- 首次访问会引导创建管理员账户

### 📄 Docker Compose 配置

```yaml
services:
  emby-proxy-tdr:
    image: onemanjs/emby-proxy-tdr:latest
    container_name: emby-proxy-tdr
    restart: unless-stopped
    network_mode: "host"
    environment:
      - TZ=Asia/Shanghai
      - ADMIN_HOST=0.0.0.0
      - ADMIN_PORT=11089
      - DATABASE_URL=sqlite:////app/instance/emby_proxy.db
      - FLASK_ENV=production
      - PYTHONUNBUFFERED=1
      - DEBUG=false
      - ENABLE_REGISTRATION=auto
      - DEFAULT_LOG_LEVEL=INFO
      - CONTAINER_ENV=docker
    volumes:
      - ./instance:/app/instance
      - ./logs:/app/logs
```

### 🔧 环境变量说明

| 变量名 | 默认值                                   | 描述 |
|--------|---------------------------------------|------|
| `TZ` | Asia/Shanghai                         | 时区设置 |
| `ADMIN_HOST` | 0.0.0.0                               | 管理界面监听地址 |
| `ADMIN_PORT` | 11089                                 | 管理界面端口 |
| `DATABASE_URL` | sqlite:////app/instance/emby_proxy.db | 数据库连接串 |
| `DEBUG` | false                                 | 调试模式开关 |
| `ENABLE_REGISTRATION` | auto                                  | 注册功能控制 |
| `DEFAULT_LOG_LEVEL` | INFO                                  | 默认日志级别 |

### 📁 数据持久化

容器会将以下目录映射到主机：
- `./instance` - 数据库文件和实例配置
- `./logs` - 应用日志文件

### ⚡ Docker 管理命令

```bash
# 查看服务状态
docker-compose ps

# 查看实时日志
docker-compose logs -f

# 重启服务
docker-compose restart

# 停止服务
docker-compose down

# 更新镜像
docker-compose pull && docker-compose up -d
```

## 💻 本地开发部署

### 📋 环境要求
- Python 3.8+
- pip 包管理器

### 🛠️ 安装步骤

1. **克隆项目**
```bash
git clone https://github.com/OneManJS/emby-proxy-tdr.git
cd emby-proxy-tdr
```

2. **安装依赖**
```bash
pip install -r requirements.txt
```

3. **启动服务**
```bash
python start.py
```

4. **访问界面**
- 地址: http://localhost:11089

## 📖 使用指南

### 🎯 首次配置

1. **创建管理员账户**
   - 首次访问系统会自动跳转到注册页面
   - 设置管理员用户名和密码

2. **创建代理实例**
   - 登录后点击"实例管理" → "新建实例"
   - 填写实例基本信息（名称、端口等）
   - 配置目标Emby服务器地址

3. **配置路径映射** (可选)
   - 设置本地路径到远程资源的映射
   - 支持HTTP、HTTPS、SMB等协议

4. **集成115网盘** (可选)
   - 只劫持strm中?pickcode=xxxx类型的链接，cms生成的strm需要打开支持开关
   - 登录115网盘获取Cookie
   - 在实例配置中添加Cookie信息
   - 启用直链播放功能

### 🔧 实例管理

#### 创建实例
1. 点击"新建实例"按钮
2. 填写实例名称和监听端口
3. 配置目标Emby服务器信息
4. 设置路径映射规则
5. 保存并启动实例

#### 路径映射配置
```
本地路径: /mnt
远程路径: http://nas.local:8080
strm：/mnt/movies/外语电影/1.mp4
替换结果：http://nas.local:8080/movies/外语电影/1.mp4
```
支持懒人配置，输入一个完整的本地路径和对应的远程路径，程序可以自动匹配


### 👥 用户管理

- **用户角色**: 管理员、普通用户
- **权限控制**: 基于角色的功能访问控制
- **操作日志**: 记录用户操作历史

### ⚙️ 系统设置

- **界面主题**: 支持明暗主题切换
- **系统参数**: 调整系统运行参数
- **安全设置**: 配置安全策略
- **数据备份**: 支持数据导出导入

## 🐛 故障排除

### 🔧 常见问题

#### Docker容器无法启动
```bash
# 检查端口占用
netstat -tlnp | grep 11089

# 查看容器日志
docker-compose logs -f
```

#### 115网盘连接失败
1. 确认Cookie信息正确性
2. 检查网络连接状态
3. 尝试重新获取Cookie


### 📋 日志查看
```bash
# 查看应用日志
docker-compose logs -f emby-proxy-tdr

# 查看系统日志
tail -f ./logs/*.log
```

## 🚀 版本更新

### 更新Docker镜像
```bash
# 拉取最新镜像
docker-compose pull

# 重启服务应用更新
docker-compose up -d
```

### 数据备份
```bash
# 备份数据目录
tar -czf backup-$(date +%Y%m%d).tar.gz instance/ logs/

# 恢复数据
tar -xzf backup-20240101.tar.gz
```


### 🐞 问题反馈
- 使用GitHub Issues报告Bug
- 详细描述问题复现步骤
- 提供系统环境信息
- 附上相关错误日志


### 技术栈致谢
- [Flask](https://flask.palletsprojects.com/) - 轻量级Web框架
- [SQLAlchemy](https://www.sqlalchemy.org/) - Python ORM框架
- [Gevent](http://www.gevent.org/) - 协程网络库
- [115 API](https://github.com/ChenyangGao/p115client) - 115网盘Python客户端



<p align="center">
  <strong>🎉 享受现代化的Emby代理管理体验！</strong>
</p>

<p align="center">
  如果这个项目对您有帮助，请给个 ⭐ Star 支持一下！
</p>
