# wiki

> 维基维护辅助工具和检查清单

---

## 质量检查清单

定期运行以下检查，确保维基质量：

### 1. 孤立页面检查

查找没有被任何其他页面引用的页面：

```
grep -r "\[\[" . --include="*.md" | grep -oP '\[\[\K[^\]]+' | sort -u | xargs -I {} sh -c 'if ! grep -rl "\[\[{}\]" . --include="*.md" > /dev/null 2>&1; then echo "孤立页面: {}"; fi'
```

### 2. 断链检查

查找引用了但不存在的页面：

```
grep -rn "\[\[" . --include="*.md" | grep -oP '\[\[\K[^\]]+' | sort -u | xargs -I {} sh -c 'if ! find . -name "{}.md" > /dev/null 2>&1; then echo "断链: {}"; fi'
```

### 3. 重复标题检查

查找元数据头中 title 重复的页面：

```
grep -r "^title:" . --include="*.md" | sort | uniq -d
```

### 4. 缺失元数据头检查

查找缺少 YAML 元数据头的页面：

```
for f in $(find . -name "*.md" -not -path "./wiki.md" -not -path "./INDEX.md" -not -path "./模板/*"); do
  if ! head -1 "$f" | grep -q "^---$"; then
    echo "缺失元数据头: $f"
  fi
done
```

---

## 统计查询

### 按类型统计页面数量

```
grep -r "^type:" . --include="*.md" | sed 's/.*type: //' | sort | uniq -c | sort -rn
```

### 按标签统计页面数量

```
grep -r "^tags:" . --include="*.md" | sed 's/.*tags: \[\([^]]*\)\].*/\1/' | tr ',' '\n' | sed 's/^ *//' | sort | uniq -c | sort -rn
```

---

## 维护记录

| 日期 | 操作 | 备注 |
|------|------|------|
| 2026-04-26 | 补充病人011~030维基知识页面 | 创建症状51、病机17、治法15、方剂11、药物35、穴位24 |
