# 维基维护工具

> 维基维护辅助工具和检查清单

---

## 质量检查清单

### 1. 断链检查

查找引用了但不存在的页面：

```bash
grep -rn "\[\[" . --include="*.md" | grep -oP '\[\[\K[^\]]+' | sort -u | while read link; do
  target=$(echo "$link" | cut -d'|' -f1)
  if ! find . -name "${target}.md" > /dev/null 2>&1; then
    echo "断链: $link"
  fi
done
```

### 2. 缺失元数据头检查

```bash
for f in $(find . -name "*.md" -not -path "*/模板/*" -not -name "INDEX.md" -not -name "LOG.md" -not -name "WIKI.md"); do
  if ! head -1 "$f" | grep -q "^---$"; then
    echo "缺失元数据头: $f"
  fi
done
```

### 3. 重复标题检查

```bash
grep -r "^title:" . --include="*.md" | sort | uniq -d
```

---

## 统计查询

### 按类型统计

```bash
grep -r "^type:" . --include="*.md" | sed 's/.*type: //' | sort | uniq -c | sort -rn
```

---

## 维护记录

| 日期 | 操作 | 备注 |
|------|------|------|
| 2026-05-12 | 知识库重构 | 建立维基双层架构 |
