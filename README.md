# 灵犀笔记公告系统使用指南

## 📋 概述

本应用使用 GitHub 作为公告系统，无需后端服务器即可实现安全公告和应用通知功能。

## 🎯 功能特性

- ✅ **完全免费**：使用 GitHub 托管公告内容
- ✅ **实时更新**：应用启动时自动检查新公告
- ✅ **安全公告**：支持紧急安全事件通知
- ✅ **版本检查**：检查应用版本兼容性
- ✅ **离线可用**：公告内容缓存到本地
- ✅ **用户友好**：支持关闭已读公告

## 📁 文件结构

```
lingxi-privacy/
├── announcements.json          # 公告配置文件
├── privacy.md                  # 隐私政策
├── terms.md                    # 用户协议
└── README.md                   # 本文档
```

## 🔧 如何发布新公告

### 1. 编辑 `announcements.json` 文件

在 `announcements` 数组中添加新公告：

```json
{
  "id": "unique-id",
  "type": "info",
  "title": "公告标题",
  "message": "公告内容",
  "version": "1.0.0",
  "date": "2026-05-10",
  "url": "https://github.com/lingxitec/lingxi-privacy/issues/1",
  "dismissible": true
}
```

### 2. 公告类型说明

| 类型 | 图标 | 颜色 | 用途 |
|------|------|------|------|
| `info` | ℹ️ | 蓝色 | 一般信息通知 |
| `warning` | ⚠️ | 橙色 | 警告信息 |
| `error` | ❌ | 红色 | 错误通知 |
| `security` | 🔒 | 紫色 | 安全公告 |

### 3. 发布安全公告

在 `security_alerts` 数组中添加安全公告：

```json
{
  "id": "security-2026-001",
  "type": "security",
  "title": "安全更新通知",
  "message": "发现安全漏洞，请立即更新到最新版本",
  "version": "1.0.1",
  "date": "2026-05-10",
  "url": "https://github.com/lingxitec/lingxi-privacy/security/advisories",
  "dismissible": false
}
```

### 4. 提交更改

```bash
git add announcements.json
git commit -m "添加新公告：[公告标题]"
git push
```

## 📱 应用集成

### 1. 在应用启动时检查公告

在 `ContentView.swift` 中添加：

```swift
import SwiftUI

struct ContentView: View {
    @StateObject private var announcementService = AnnouncementService.shared
    
    var body: some View {
        // 您的视图内容
        .onAppear {
            Task {
                await announcementService.fetchAnnouncements()
            }
        }
        .sheet(item: $announcementService.selectedSecurityAlert) { alert in
            SecurityAlertSheet(alert: alert) {
                announcementService.dismissSecurityAlert(alert)
            }
        }
    }
}
```

### 2. 在设置页面显示公告

```swift
struct SettingsView: View {
    @StateObject private var announcementService = AnnouncementService.shared
    
    var body: some View {
        List {
            if !announcementService.announcements.isEmpty {
                Section("公告") {
                    ForEach(announcementService.announcements) { announcement in
                        AnnouncementBanner(announcement: announcement) {
                            announcementService.dismissAnnouncement(announcement)
                        }
                    }
                }
            }
            
            // 其他设置项
        }
    }
}
```

## 🚨 安全事件处理流程

### 1. 发现安全漏洞

1. 在 GitHub 创建 Security Advisory
2. 更新 `announcements.json` 中的 `security_alerts`
3. 发布新版本应用
4. 在 App Store 更新说明中提及安全更新

### 2. 通知用户

用户下次打开应用时：
- 自动检查安全公告
- 显示安全警告弹窗
- 引导用户更新应用

## 📊 公告统计

您可以通过以下方式了解公告效果：

1. **GitHub Insights**：查看仓库访问统计
2. **App Store Connect**：查看应用更新率
3. **应用内统计**：使用友盟+跟踪公告展示次数

## 🔐 最佳实践

### 1. 公告内容

- ✅ 简洁明了，突出重点
- ✅ 提供详细链接（GitHub Issue）
- ✅ 包含版本信息
- ✅ 标注日期

### 2. 安全公告

- ⚠️ 不要泄露漏洞细节
- ⚠️ 提供明确的修复建议
- ⚠️ 设置 `dismissible: false`
- ⚠️ 优先级高于普通公告

### 3. 版本兼容性

```json
{
  "current_version": "1.0.1",
  "min_supported_version": "1.0.0"
}
```

- `current_version`：最新版本
- `min_supported_version`：最低支持版本

## 📝 示例公告

### 功能更新公告

```json
{
  "id": "feature-2026-001",
  "type": "info",
  "title": "新功能上线",
  "message": "AI每日回顾功能已上线，快来体验吧！",
  "version": "1.1.0",
  "date": "2026-05-15",
  "url": "https://github.com/lingxitec/lingxi-privacy/issues/10",
  "dismissible": true
}
```

### 维护通知

```json
{
  "id": "maintenance-2026-001",
  "type": "warning",
  "title": "计划维护通知",
  "message": "AI服务将于5月20日凌晨2-4点进行维护",
  "version": "1.0.0",
  "date": "2026-05-18",
  "url": "https://github.com/lingxitec/lingxi-privacy/issues/11",
  "dismissible": true
}
```

### 安全更新

```json
{
  "id": "security-2026-001",
  "type": "security",
  "title": "重要安全更新",
  "message": "发现潜在安全问题，请立即更新到 v1.0.2",
  "version": "1.0.2",
  "date": "2026-05-20",
  "url": "https://github.com/lingxitec/lingxi-privacy/security/advisories/1",
  "dismissible": false
}
```

## 🆘 常见问题

### Q: 公告没有显示？
A: 
1. 检查 `announcements.json` 格式是否正确
2. 确认文件已推送到 GitHub
3. 检查网络连接
4. 查看应用日志

### Q: 如何删除公告？
A: 
1. 从 `announcements.json` 中删除对应条目
2. 提交并推送到 GitHub
3. 用户下次打开应用时会同步更新

### Q: 如何修改已发布的公告？
A: 
1. 修改 `announcements.json` 中的内容
2. 提交并推送
3. 注意：已关闭的公告不会再次显示

## 📞 联系方式

如有问题，请通过以下方式联系：

- **GitHub Issues**: https://github.com/lingxitec/lingxi-privacy/issues
- **开发者邮箱**: 892086417@qq.com

---

**最后更新**: 2026-05-10
