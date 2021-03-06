# 3.1.3 弱引用

Python 内存管理使用垃圾回收的方式，当没有指向对象的引用时，对象就会被回收。然而对象一直被持有也并非什么好事，比如我们要实现一个缓存，预期目标是缓存中的内容随着真正对象的存在而存在，随着真正对象的消失而消失。如果因为缓存的存在，导致被缓存的对象无法释放，就会导致内存泄漏。

Python 提供了语言级别的支持，我们可以使用 `weakref` 模块，它提供了 `weakref.WeakValueDictionary` 这个弱引用字典来确保字典中的值不会被引用。如果想要获取某个对象的弱引用，可以使用 `weakref.ref(obj)` 函数。