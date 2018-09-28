# 5.3.2 JSON 读写


```python
import json

d = {'name': 'JarvisMa', 'age': 22}
json.dumps(d)  # 返回字符串：{"name": "JarvisMa", "age": 22}
```

最简单的对象转 JSON 通过 `json` 模块的 `dumps` 函数来完成，最后一个字母 s 表示生成字符串，也可以用 `dump(data, fp)` 来把内容写入文件：

```python
import json

d = {'name': 'JarvisMa', 'age': 22}
with opne('path_to_file', 'w') as f:
	json.dump(d, f)
```
调用 `dumps` 参数时，有几个命令可能会派上作用：

```python
import json

d = {'name': 'JarvisMa', 'age': 22}
json.dumps(d, skipkeys=True, sort_keys=True, indent=4)

#{
#    "age": 22,
#    "name": "JarvisMa"
#}
```

其中，`sort_keys` 表示对字典的键排序，这样输出结果一定是固定的，`indent` 用于控制多少个空格缩进，可以增加可读性。`skipkeys 表示如果字典的键不是字符串，就忽略这一条记录。

JSON 支持的类型很有限，只有 `None` ， `bool` ， `int` ， `float` 和 `str 这五种基本类型和包含这些类型的字典或者数组。自定义的对象如果转成 JSON 默认会报错，我们可以实现一个通用函数，读取任意对象的 `__dict__`，这样就可以用于 JSON 化了：

```python
import json

def serialize_instance(obj):
	d = { 'class' : type(obj).__name__ }
	d.update(vars(obj))
	return d

class Person:
	def __init__(self, name, age):
		self.name = name
		self.age = age

bs = Person('JarvisMa', 22)

# 直接 dumps(bs) 会得到这个报错：
# TypeError: Object of type 'Person' is not JSON serializable

json.dumps(serialize_instance(bs), skipkeys=True, sort_keys=True, indent=4)

#{
#    "age": 22,
#    "class": "Person",
#    "name": "JarvisMa"
#}
```

或者更优雅的做法是使用 3.3.3 节中介绍的 Mixin：

```python
import json

# 注意 Mixin 的原则，功能要单一，实现上不能依赖子类
class Serializable:
	def serialize(self):
		d = { 'class' : type(self).__name__ }
		d.update(vars(self))
		return d

# 混入 Serializable 立刻就有了转字典的能力，或者也可以重写
class Person(Serializable):
	def __init__(self, name, age):
		self.name = name
		self.age = age

bs = Person('JarvisMa', 22)

json.dumps(bs.serialize())
```

解析 JSON 字符串的方法也是类似的，调用 `json.loads()` 函数即可：

```python
import json

s = '{"name": "JarvisMa", "age": 22}'
d = json.loads(s)
print(d)
这里的 loads 表示从字符串中读取 JSON，也可以用 load 函数从文件中读取。不过有时候我们更希望把读取出来的字典直接转成对象。这需要调用者提供一个函数，把字典转换成对象，一般需要用到元编程。下面是一个简单的示例：

import json

class Deserializable:
	@classmethod
	def deserialize(cls, d):
		clsname = d.pop('classname', None)
		if clsname:
			obj = cls.__new__(cls)
			for key, value in d.items():
				setattr(obj, key, value)
			return obj
		else:
			return d

class Person(Deserializable):
	def __init__(self, name, age):
		self.name = name
		self.age = age

s = '{"age": 22, "name": "JarvisMa"}'
bs = json.loads(s, object_hook = Person.deserialize)
print(bs)
```

`Person` 类通过混入 `Deserializable` 具备了反序列化的能力，只要把这个函数传入 `loads` 方法中即可，`deserialize` 函数的第一个参数是调用类，第二个参数是解析出来的字典。这段代码其实等价于：

```python
d = json.loads(s)
bs = Person.deserialize(d)
```
