# ARTS 第一周打卡

## Algorithm

​	x的平方跟。leetcode地址：<https://leetcode-cn.com/problems/sqrtx/>

​	 解体思路：牛顿迭代法

```java
    public int mySqrt(int x) {
        long t = 1<<8;
        while(!(t*t <=x && (t+1)*(t+1) >x)){
            t = (t + x/t) /2;
        }
        return (int)t;
    }
```

执行用时 : 6 ms, 在Sqrt(x)的Java提交中击败了97.63% 的用户

内存消耗 : 33.3 MB, 在Sqrt(x)的Java提交中击败了78.22% 的用户

## Review

### 遗留代码中去重技巧

```
本文翻译至 <https://blog.frankel.ch/deduplication-trick/>
```



```
这可能发生在你需要去重一系列来自于一六代码的重复数据。这些我们称之为遗留代码的对象的类已经实现了equals()方法和hashCode()方法。因为害怕破坏正在运行的代码，我们不能改变实现。并且不幸的是，Java API没有提供比较他们特征不同的方法（与普通类）。
```

```
在下面的例子中，一个小技巧就是，使用包装类来包装遗留类，并在包装类中使用期望的实现。（equals()或者hashCode(()）。
```

```java
public class LegacyObject {

  private final UUID id;
  private final String foo;
  private final int bar;

  public LegacyObject(UUID id, String foo, int bar) {
    this.id = id;
    this.foo = foo;
    this.bar = bar;
  }

  @Override
  public int hashCode() {
    return Objects.hash(id);
  }

  // Implementation of equals() using only the id field
  // Getters
}
```

```java
public class DeduplicateWrapper {

  private final LegacyObject object;

  public DeduplicateWrapper(LegacyObject object) {
    this.object = object;
  }

  public LegacyObject getObject() {
    return object;
  }

  @Override
  public int hashCode() {
    return Objects.hash(object.getFoo());
  }

  // Implementation of equals() using only the foo field of the   //wrapped object
}
```

```
我们可以无脑的使用列表的流式运算来进行去重。
```

```java
List<LegacyObject> duplicates = ...;

duplicates.stream()
    .map(DeduplicateWrapper::new)
    .distinct()
    .map(DeduplicateWrapper::getObject);
```

```
如果在java8之前，我们需要更麻烦一点。
```

```java
List<LegacyObject> deduplicated = new ArrayList<>();
Set<DeduplicateWrapper> wrappers = new HashSet<>();
for (LegacyObject duplicate: duplicates) {
  wrappers.add(new DeduplicateWrapper(duplicate));
}
for (DeduplicateWrapper wrapper: wrappers) {
  deduplicated.add(wrapper.getObject());
}
```



又或者，存在第三方工具库帮助我们解决这个问题。



------



```
个人总结：这个方法很像计算机领域的一句名言：没有什么问题不是抽象出一层解决不了的。如果有，那就再抽象一层。这个去重比较的方法就是通过包装了一层，在不改变原来的类的情况下实现的。
```

## Tips

### Git别名的使用。

​	有一个小技巧可以使你的 Git 体验更简单、容易、熟悉：别名。Git 并不会在你输入部分命令时自动推断出你想要的命令。 如果不想每次都输入完整的 Git 命令，可以通过 `git config` 文件来轻松地为每一个命令设置一个别名。 这里有一些例子你可以试试：

```
$ git config --global alias.co checkout
$ git config --global alias.st status
$ git config --global alias.br branch
$ git config --global alias.ci commit
```

## Share

 二分查找，详情见博客<http://www.cxzd.space/2019/04/07/%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE/>