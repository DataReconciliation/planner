# 时间块规划器 - 安装和使用指南

## 项目简介

这是一个基于Ruby的时间块规划器，可以生成PDF格式的规划页面，灵感来源于Cal Newport的时间块规划方法。

## 系统要求

- Linux/macOS/Windows (WSL)
- Ruby 3.0+
- Bundler

## 安装步骤

### 1. 安装Ruby和Bundler

**Ubuntu/Debian系统：**
```bash
sudo apt update
sudo apt install ruby ruby-bundler
```

**CentOS/RHEL系统：**
```bash
sudo yum install ruby rubygems ruby-devel
gem install bundler
```

**使用Snap（通用）：**
```bash
sudo snap install ruby
```

### 2. 验证安装
```bash
ruby --version
bundle --version
```

### 3. 克隆项目（如果还没有）
```bash
git clone git@github.com:drewish/planner.git
cd planner
```

### 4. 安装项目依赖

**设置本地安装路径（避免权限问题）：**
```bash
bundle config set --local path vendor/bundle
```

**安装依赖包：**
```bash
bundle install
```

**如果是Linux系统，需要添加matrix依赖：**
```bash
bundle add matrix
```

### 5. 修复字体路径（Linux系统）

编辑 `config.rb` 文件，将Mac字体路径替换为Linux字体：

```ruby
# 原始Mac配置
OSX_FONT_PATH = "/System/Library/Fonts/Supplemental/Futura.ttc"

# 修改为Linux配置
UBUNTU_FONT_BASE = "/usr/share/fonts/truetype/ubuntu"
FONTS = {
  'Ubuntu' => {
    normal: "#{UBUNTU_FONT_BASE}/Ubuntu-R.ttf",
    italic: "#{UBUNTU_FONT_BASE}/Ubuntu-RI.ttf", 
    bold: "#{UBUNTU_FONT_BASE}/Ubuntu-B.ttf",
    condensed: "#{UBUNTU_FONT_BASE}/Ubuntu-C.ttf",
  }
}
```

## 使用方法

### 基本命令

**生成当前周的规划页面：**
```bash
bundle exec ruby planner.rb
```

**生成指定日期开始的规划页面：**
```bash
bundle exec ruby planner.rb 2025-09-01
```

**生成多周页面：**
```bash
bundle exec ruby planner.rb --weeks 4
```

**生成特定时间段的页面（例如从2025年9月1日到2026年1月1日）：**
```bash
bundle exec ruby planner.rb --weeks 18 2025-09-01
```

### 其他功能

**生成一对一会议页面：**
```bash
bundle exec ruby one-on-one.rb --weeks 4 2025-09-01
```

**生成笔记页面：**
```bash
bundle exec ruby notes.rb
```

**查看帮助：**
```bash
bundle exec ruby planner.rb --help
```

### 命令参数说明

- `--weeks NUM` 或 `-w NUM`：生成指定周数的页面
- `--locale LOCALE` 或 `-l LOCALE`：设置语言环境
- `--help` 或 `-h`：显示帮助信息
- `STARTDATE`：指定开始日期，格式如 `2025-09-01`

## 输出文件

- **规划页面**：`time_block_pages.pdf`
- **一对一会议页面**：由one-on-one.rb脚本生成
- **笔记页面**：由notes.rb脚本生成

## 自定义配置

### 编辑工作时间
修改 `config.rb` 中的 `HOUR_LABELS` 数组：
```ruby
HOUR_LABELS = [nil, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, nil, nil]
```

### 设置一对一会议
在 `config.rb` 的 `one_on_ones_for` 函数中添加你的会议安排：
```ruby
def one_on_ones_for sunday
  tue = %w(张三)
  wed = %w(李四 王五)
  thr = %w(赵六)
  # ...
end
```

### 调整页面设置
- 页面大小：修改 `PAGE_SIZE`（'LETTER' 或 'A4'）
- 颜色方案：修改 `LIGHT_COLOR`, `MEDIUM_COLOR`, `DARK_COLOR`
- 页边距：修改 `LEFT_PAGE_MARGINS`, `RIGHT_PAGE_MARGINS`

## 打印建议

**Linux系统打印：**
```bash
lpr time_block_pages.pdf
```

**查看PDF：**
```bash
evince time_block_pages.pdf  # 或其他PDF查看器
```

## 故障排除

### 常见问题

1. **字体错误**：确保已修复Linux字体路径配置
2. **权限错误**：使用 `bundle config set --local path vendor/bundle`
3. **Ruby版本问题**：确保Ruby版本 >= 3.0

### 检查安装
```bash
# 检查Ruby版本
ruby --version

# 检查依赖安装
bundle check

# 测试生成
bundle exec ruby planner.rb
```

## 示例使用场景

**学期规划（4个月）：**
```bash
bundle exec ruby planner.rb --weeks 16 2025-09-01
```

**季度规划（3个月）：**
```bash
bundle exec ruby planner.rb --weeks 12 2025-10-01
```

**月度规划：**
```bash
bundle exec ruby planner.rb --weeks 4 2025-09-01
```

---

现在你可以根据这个文档轻松地安装、配置和使用时间块规划器了！