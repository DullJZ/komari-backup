<div align="center">

# 📦 Komari Backup

通过 GitHub Actions 全自动备份 [Komari](https://github.com/komari-monitor/komari) 数据的轻量级解决方案。

[![Komari Monitor](https://img.shields.io/badge/Komari%20Monitor-000000?style=for-the-badge&logo=github)](https://github.com/komari-monitor/komari)
[![Backup Workflow](https://img.shields.io/badge/GitHub%20Actions-Backup-2088FF?style=for-the-badge&logo=github-actions)](.github/workflows/backup.yml)

</div>

---

无需额外的服务器或定时任务，利用 GitHub Actions 的免费算力即可实现数据安全备份。自动将数据导出并安全地存储在本仓库的 `backup` 分支中。

## 🚀 部署步骤

> [!CAUTION]
> **数据泄露警告**：请务必确保您的**备份仓库是私有（Private）的**！否则所有的备份文件都将被公开访问！

1. 📥 **导入本仓库**：打开 [GitHub Import](https://github.com/new/import)，在 `Your old repository's clone URL` 中填入本仓库地址：
   ```
   https://github.com/DullJZ/komari-backup
   ```
   在 `Privacy` 中选择 **Private**，然后点击 `Begin import` 完成导入。
2. 🔑 **配置环境变量**：在导入后的仓库中，进入 `Settings` -> `Secrets and variables` -> `Actions`，添加所需的 [Action Secrets](#%EF%B8%8F-secrets-配置)。
3. ▶️ **启用并测试**：进入 `Actions` 页面，启用并同意使用 Workflows，运行一次 `Komari Data Backup` 工作流进行初步测试。

---

## ⚙️ Secrets 配置

在仓库设置中添加以下 Action Secrets 以允许工作流访问您的 Komari 实例 API：

| Secret 名称 | 描述说明 | 示例取值 |
| :--- | :--- | :--- |
| `KOMARI_API_KEY` | Komari 管理面板中设置的 API Key。 | `sk-your-super-secret-api-key` |
| `KOMARI_BASE_URL` | Komari 服务的外部访问地址（**末尾请勿带 `/`**）。 | `https://monitor.example.com` |

---

## 💡 工作原理

本项目采用极简的工作流设计，核心备份流程如下：

1. ⏱️ 每天 UTC 时间 `00:00` 定时触发执行（也完全支持通过页面手动触发）。
2. 📥 调用 `GET /api/admin/download/backup` 接口自动下载备份文件（压缩包内包含完整的数据库和 `data` 目录）。
3. ☁️ 将下载好的 zip 文件安全推送到本仓库的 [`backup`](../../tree/backup) 分支中。
4. 🧹 自动清理旧数据，**仅保留最近 30 天**的备份文件。

> [!NOTE]  
> 所有代码提交均使用 `github-actions[bot]` 的机器号身份，**不会**计入您的个人贡献图（绿墙）。

---

## 🛠️ 管理与还原

### 手动触发备份

如需立即备份，可在 GitHub 仓库页面进入 **Actions**，选择左侧的 **Komari Data Backup**，点击右侧的 **Run workflow** 执行即时备份。

### 查看备份文件

在主页左侧的分支选择器中，切换到 [`backup`](../../tree/backup) 分支即可查看所有已完成的备份。

提取数据时，文件名格式遵循：
📄 `backup-YYYYMMDD-HHMMSS.zip`

您可以随时下载所需日期的版本进行数据恢复。
