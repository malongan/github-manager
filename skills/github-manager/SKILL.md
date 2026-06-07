# GitHub Manager Skill

GitHub 仓库管理技能，提供完整的仓库管理功能。

## 配置

- **GitHub Token:** (请在 AGENTS.md 中配置)
- **用户名:** malongan
- **API 基础地址:** `https://api.github.com`

## 工具

本技能使用 curl 命令与 GitHub API 交互。

### 基础请求头

```bash
-H "Authorization: token <YOUR_TOKEN>"
-H "Content-Type: application/json"
```

---

## 功能列表

### 1. 列出所有仓库

```bash
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/user/repos?per_page=100&sort=updated"
```

### 2. 查看单个仓库

```bash
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/repos/malongan/<repo-name>"
```

### 3. 创建仓库

```bash
curl -s -X POST \
  -H "Authorization: token <TOKEN>" \
  -H "Content-Type: application/json" \
  "https://api.github.com/user/repos" \
  -d '{
    "name": "仓库名",
    "description": "描述",
    "private": false,
    "auto_init": true
  }'
```

### 4. 删除仓库

```bash
curl -s -X DELETE \
  -H "Authorization: token <TOKEN>" \
  "https://api.github.com/repos/malongan/<repo-name>"
```

⚠️ **危险操作，需要二次确认**

### 5. 查看仓库文件

```bash
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/repos/malongan/<repo>/contents/"
```

### 6. 获取文件内容

```bash
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/repos/malongan/<repo>/contents/<path>"
```

### 7. 创建/更新文件

```bash
# 1. 获取原文件的 SHA（如果是更新）
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/repos/malongan/<repo>/contents/<path>"

# 2. 上传文件（content 需要 base64 编码）
curl -s -X PUT \
  -H "Authorization: token <TOKEN>" \
  -H "Content-Type: application/json" \
  "https://api.github.com/repos/malongan/<repo>/contents/<path>" \
  -d '{
    "message": "commit message",
    "content": "<base64_content>",
    "sha": "<file_sha>"  // 更新时需要
  }'
```

### 8. 批量上传文件

遍历文件列表，逐个上传。

### 9. 查看仓库统计

```bash
# 语言统计
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/repos/malongan/<repo>/languages"

# 提交统计
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/repos/malongan/<repo>/stats/commit_activity"

# 贡献者
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/repos/malongan/<repo>/contributors"
```

### 10. 搜索仓库

```bash
curl -s -H "Authorization: token <TOKEN>" \
  "https://api.github.com/search/repositories?q=user:malongan+关键词"
```

---

## 使用示例

### 创建新仓库

```
用户: 帮我创建一个叫 test-repo 的仓库
助手: 
1. 执行创建命令
2. 返回结果
```

### 上传文件

```
用户: 把 ./test.py 上传到 test-repo 仓库
助手:
1. base64 编码文件
2. 执行上传命令
3. 返回结果
```

### 查看仓库列表

```
用户: 我有哪些仓库？
助手: 列出所有仓库及其基本信息
```

---

## 错误处理

| 错误码 | 说明 | 处理 |
|--------|------|------|
| 401 | Token 无效 | 检查 Token 配置 |
| 403 | 权限不足 | 确认 Token 有 repo 权限 |
| 404 | 仓库/文件不存在 | 检查名称 |
| 422 | 验证失败 | 检查参数格式 |

---

## 注意事项

1. **Token 安全**: 不要在日志中暴露完整 Token
2. **二次确认**: 删除仓库等危险操作需要确认
3. **频率限制**: GitHub API 有 rate limit，批量操作需要间隔
4. **编码问题**: 文件内容需要 base64 编码
