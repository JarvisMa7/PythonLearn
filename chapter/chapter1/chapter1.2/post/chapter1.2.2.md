# 1.2.2 查询字典

简单方法是直接写键名，但如果键名不存在会抛出 `KeyError`：

```python
d = {'a': 61}
d['a'] # 值是 61
d['b'] # KeyError: 'b'
```

可以用 `if key in dict` 的判断来检查键是否存在，甚至可以先 `try` 再 `catch KeyError` ，但更加优雅简洁一些的写法是用 `get(k, default)` 方法来提供默认值：

```python
d = {'a': 61}
d.get('a', 62) # 得到 61
d.get('b', 62) # 得到 62
```

不过有时候，我们可能不仅仅要读出默认属性，更希望能把这个默认属性能写入到字典中，比如：

```python
d = {}
# 我们想对字典中某个 Value 做操作，如果 Key 不存在，就先写入一个空值
if 'list' not in d:
	d['list'] = []
d['list'].append(1)
```

这种情况下，`setdefault(key, default)` 函数或许更合适：

```python
d.setdefault('key', []).append(1)
```

这个函数虽然名为 `set`，但作用其实是查找，仅仅在查找不到时才会把默认值写入字典。