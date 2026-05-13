# 素材预览模块 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在 index.html 的素材配置模块下方新增素材预览功能，点击按钮展开 mock 数据表格。

**Architecture:** 单文件改动（index.html），分三块：CSS 样式、HTML 结构、JS 逻辑。预览按钮插入底部按钮区，预览区域插入素材配置模块与底部按钮之间。

**Tech Stack:** Vanilla HTML/CSS/JS，无框架，无外部依赖，数据全部 mock。

---

### Task 1: 新增 CSS 样式

**Files:**
- Modify: `index.html` — `<style>` 块内追加

- [ ] **Step 1: 追加预览区域和表格样式**

在 `</style>` 前（第 642 行附近）插入：

```css
/* 素材预览区域 */
.material-preview-section {
    margin-top: 24px;
    border: 1px solid #dcdfe6;
    border-radius: 4px;
    display: none;
}
.material-preview-section.show {
    display: block;
}
.material-preview-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 16px;
    border-bottom: 1px solid #ebeef5;
    font-size: 14px;
    color: #606266;
    font-weight: 500;
}
.material-preview-table-wrap {
    overflow-x: auto;
}
.material-preview-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
    min-width: 960px;
}
.material-preview-table th {
    padding: 8px 12px;
    border-bottom: 1px solid #ebeef5;
    color: #909399;
    font-weight: 500;
    text-align: left;
    background: #f5f7fa;
    white-space: nowrap;
}
.material-preview-table td {
    padding: 8px 12px;
    border-bottom: 1px solid #ebeef5;
    color: #606266;
    white-space: nowrap;
}
.material-preview-table tbody tr:hover {
    background-color: #f5f7fa;
}
.material-preview-empty {
    text-align: center;
    padding: 32px;
    color: #909399;
    font-size: 14px;
}
.material-preview-footer {
    padding: 8px 16px;
    border-top: 1px solid #ebeef5;
    font-size: 12px;
    color: #909399;
    text-align: center;
}
```

- [ ] **Step 2: 验证 CSS** — 检查 `uvx ruff check` 对 HTML 无 CSS 语法警告（跳过此步，项目无构建工具，人工 review 即可）

---

### Task 2: 新增 HTML 结构

**Files:**
- Modify: `index.html` — 素材配置模块结束注释后、底部按钮区前

- [ ] **Step 1: 插入预览区域 HTML**

在第 942 行（`<!-- ==================== 素材配置模块 结束 ==================== -->`）之后、第 944 行（`<!-- 底部操作按钮 -->`）之前插入：

```html
                <!-- 素材预览区域 -->
                <div class="material-preview-section" id="materialPreviewSection">
                    <div class="material-preview-header">
                        <span id="materialPreviewTitle">素材预览</span>
                        <span class="tag-close" onclick="closeMaterialPreview()" style="cursor:pointer;">&times;</span>
                    </div>
                    <div id="materialPreviewContent">
                        <div class="material-preview-empty">点击「预览素材」查看匹配结果</div>
                    </div>
                </div>
```

- [ ] **Step 2: 在底部按钮区插入预览按钮**

在第 946 行，`btn-cancel` 按钮和 `btn-submit` 按钮之间插入：

```html
                    <button type="button" class="btn-cancel" id="btnPreview" onclick="toggleMaterialPreview()" style="border-color:#4080ff;color:#4080ff;">预览素材</button>
```

---

### Task 3: 新增 JS 逻辑

**Files:**
- Modify: `index.html` — `<script>` 块内追加

- [ ] **Step 1: 追加 mock 数据生成函数**

在 `</script>` 前（第 1401 行附近），追加 mock 数据与预览逻辑：

```javascript
        // ==================== 素材预览功能 ====================
        const mockMaterialData = [
            { name: '素材_001.mp4', bookId: 'L0001', bookName: '繁花', drama: '繁花', lang: '中文', cost: 1234, doroi: 2.3, compliance: 85, created: '2026-05-12' },
            { name: '素材_002.mp4', bookId: 'L0001', bookName: '繁花', drama: 'Blooming', lang: '英文', cost: 856, doroi: 1.8, compliance: 72, created: '2026-05-11' },
            { name: '素材_003.mp4', bookId: 'L0001', bookName: '繁花', drama: 'บุษบา', lang: '泰文', cost: 2100, doroi: 3.1, compliance: 91, created: '2026-05-10' },
            { name: '素材_004.mp4', bookId: 'L0001', bookName: '繁花', drama: 'Mekar', lang: '马来文', cost: 450, doroi: 0.9, compliance: 58, created: '2026-05-13' },
            { name: '素材_005.mp4', bookId: 'L0002', bookName: '狂飙', drama: '狂飙', lang: '中文', cost: 3200, doroi: 4.5, compliance: 95, created: '2026-05-09' },
            { name: '素材_006.mp4', bookId: 'L0002', bookName: '狂飙', drama: 'The Knockout', lang: '英文', cost: 1890, doroi: 2.7, compliance: 80, created: '2026-05-08' },
            { name: '素材_007.mp4', bookId: 'L0003', bookName: '漫长的季节', drama: '漫长的季节', lang: '中文', cost: 670, doroi: 1.5, compliance: 66, created: '2026-05-11' },
            { name: '素材_008.mp4', bookId: 'L0003', bookName: '漫长的季节', drama: 'The Long Season', lang: '英文', cost: 980, doroi: 2.0, compliance: 74, created: '2026-05-07' },
            { name: '素材_009.mp4', bookId: 'L0004', bookName: '三体', drama: '三体', lang: '中文', cost: 5600, doroi: 5.2, compliance: 98, created: '2026-05-13' },
            { name: '素材_010.mp4', bookId: 'L0004', bookName: '三体', drama: 'Three-Body', lang: '英文', cost: 4200, doroi: 4.8, compliance: 93, created: '2026-05-06' },
            { name: '素材_011.mp4', bookId: 'L0005', bookName: '庆余年', drama: '庆余年', lang: '中文', cost: 1500, doroi: 2.9, compliance: 82, created: '2026-05-12' },
            { name: '素材_012.mp4', bookId: 'L0005', bookName: '庆余年', drama: 'Joy of Life', lang: '英文', cost: 1100, doroi: 2.1, compliance: 76, created: '2026-05-10' },
        ];

        function generatePreviewData() {
            const count = Math.floor(Math.random() * 5) + 6;
            const shuffled = [...mockMaterialData].sort(() => Math.random() - 0.5);
            return shuffled.slice(0, count).sort((a, b) => b.cost - a.cost);
        }

        let previewVisible = false;

        function toggleMaterialPreview() {
            const section = document.getElementById('materialPreviewSection');
            const content = document.getElementById('materialPreviewContent');
            const btn = document.getElementById('btnPreview');

            if (previewVisible) {
                previewVisible = false;
                section.classList.remove('show');
                btn.textContent = '预览素材';
                return;
            }

            previewVisible = true;
            section.classList.add('show');
            btn.textContent = '收起预览';
            content.innerHTML = '<div class="material-preview-empty">正在查询匹配素材...</div>';

            setTimeout(() => {
                const data = generatePreviewData();
                if (data.length === 0) {
                    content.innerHTML = '<div class="material-preview-empty">未找到匹配的素材，请调整筛选条件</div>';
                    return;
                }
                const rows = data.map(d => `
                    <tr>
                        <td>${d.name}</td>
                        <td>${d.bookId}</td>
                        <td>${d.bookName}</td>
                        <td>${d.drama}</td>
                        <td>${d.lang}</td>
                        <td>¥${d.cost.toLocaleString()}</td>
                        <td>${d.doroi}</td>
                        <td>${d.compliance}%</td>
                        <td>${d.created}</td>
                    </tr>
                `).join('');
                const limitNote = data.length > 10 ? `<div class="material-preview-footer">共 ${data.length} 条，仅展示前 10 条</div>` : `<div class="material-preview-footer">共 ${data.length} 条</div>`;
                content.innerHTML = `
                    <div class="material-preview-table-wrap">
                        <table class="material-preview-table">
                            <thead>
                                <tr>
                                    <th>素材名称</th><th>剧本ID</th><th>剧本名称</th><th>短剧</th><th>语言</th><th>消耗</th><th>DOROI</th><th>达标率</th><th>创建时间</th>
                                </tr>
                            </thead>
                            <tbody>${rows}</tbody>
                        </table>
                    </div>
                    ${limitNote}
                `;
            }, 600);
        }

        function closeMaterialPreview() {
            previewVisible = false;
            const section = document.getElementById('materialPreviewSection');
            section.classList.remove('show');
            document.getElementById('btnPreview').textContent = '预览素材';
        }

        function resetMaterialPreview() {
            if (previewVisible) {
                closeMaterialPreview();
            }
            document.getElementById('materialPreviewContent').innerHTML = '<div class="material-preview-empty">点击「预览素材」查看匹配结果</div>';
        }
```

- [ ] **Step 2: 素材条件变更时自动清空预览**

在 `checkMaterialFilterVisible()` 函数末尾（第 1235 行后），追加 `resetMaterialPreview()` 调用。在该函数内 `checkMaterialConditionRequired()` 调用后插入：

```javascript
            resetMaterialPreview();
```

同样在以下事件回调中追加 `resetMaterialPreview()`：
- `materialFilterTypeBtns` 的 click 回调（第 1224 行的 `checkMaterialFilterVisible()` 之后已有）
- `addMaterialConditionBtn` 的 click 回调末尾
- 素材条件行的 metric select change 回调末尾
- 素材条件行的 operator select change 回调末尾
- `materialCountBtns` 的 click 回调末尾
- `repeatCountBtns` 的 click 回调末尾
- `sortMetricBtns` 的 click 回调末尾
- `statRangeBtns` 的 click 回调末尾

具体修改：

在第 1224 行 `checkMaterialConditionRequired();` 后追加 `resetMaterialPreview();`

在第 1313 行（`addMaterialConditionBtn` 回调末尾 `checkMaterialConditionDuplicate();` 后）追加 `resetMaterialPreview();`

在第 1210 行（`materialCountBtns` 回调末尾 `});` 前）追加 `resetMaterialPreview();`

在第 1343 行（`repeatCountBtns` 回调末尾 `});` 前）追加 `resetMaterialPreview();`

在第 1222 行（`sortMetricBtns` 回调末尾 `});` 前）追加 `resetMaterialPreview();`

在第 1243 行（`statRangeBtns` 回调末尾 `});` 前）追加 `resetMaterialPreview();`

在第 1195 行（`createTimeBtns` 回调末尾 `});` 前）追加 `resetMaterialPreview();`

- [ ] **Step 3: 短剧变更时自动清空预览**

在 `addDrama()` 函数末尾（第 1140 行 `});` 前）追加 `resetMaterialPreview();`
在 `removeDrama()` 函数末尾（第 1149 行 `}` 前）追加 `resetMaterialPreview();`

- [ ] **Step 4: 预览按钮禁用逻辑**

在 `checkMaterialConditionRequired()` 末尾（第 1321 行 `return !needCheck || hasRows;` 前）追加：

```javascript
            const previewBtn = document.getElementById('btnPreview');
            if (previewBtn) {
                previewBtn.disabled = !(!needCheck || hasRows);
                previewBtn.style.opacity = (!needCheck || hasRows) ? '1' : '0.5';
                previewBtn.style.cursor = (!needCheck || hasRows) ? 'pointer' : 'not-allowed';
            }
```

---

### Task 4: 验证

- [ ] **Step 1: 检查 HTML 结构完整性**

确认预览区域 HTML 在素材配置结束注释和底部按钮之间，预览按钮在取消和确定之间。

- [ ] **Step 2: 检查 JS 逻辑**

确认所有回调中 `resetMaterialPreview()` 调用位置正确，不会导致引用错误（函数定义在使用前）。

- [ ] **Step 3: 检查 CSS 无冲突**

确认新增 CSS 类名不与现有类名冲突。

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: 新增素材预览模块"
```
