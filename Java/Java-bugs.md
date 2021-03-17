# Java Bugs

# java.util.NoSuchElementException: No line found

有多个Scanner对象的情况下，关闭一个后导致其他Scanner对象无法读取。即使是调用了两个不同的方法，它们各自都创建了Scanner对象，先执行的方法关闭了Scanner资源后，后调用的方法中的Scanner也无法读取。