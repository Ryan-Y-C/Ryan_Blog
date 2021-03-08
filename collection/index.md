# Collection


<!--more-->

## 1  &nbsp; Collection &nbsp; 体系原理及实战
Collection体系简介

- Collection  &nbsp; 包含一组元素

List/Set约定

- List  &nbsp;  一组有序的集合
- Set  &nbsp; 一组无序的集合

File Structure（Ctrl + 12）

- 查看当前类的所有方法 

Collection 常用用方法

- new:new ArrayList(Collection),new ArrayList()
- R:size()/isEmpty()/contains()/for()/stream()
    - isEmpty():是否为空
    - contains():是否包含一个指定元素
- C/U：add()/addAll()/retainAll()
    - retainAll :保留Collection中有的元素
- D：clear/remove()/removeAll()   
Collections工具方法集合
- emptySet（）：等返回一个方便的空集合。
- synchronizedCollection:将一个集合变成线程安全的
- unmodifiableCollection:将一个集合变成不可变的（也可以使用Guava的Immutable）

Collection其他实现：
- Queue（队列）
    - 带有优先级的一组集合
    - 具有插入、提取、检查
    - LILO (先进先出)：Last In Last Out。
- Deque（线性集合）
    - 在两端插入和删除元素
- LinkedList （链表）、
- PriorityQueue(优先级队列)
## 2 List

ArrayList

- 本质就是一个数组

ArrayList动态扩容的实现

- 创建一个更大的空间，然后把原先的所有元素拷贝过去
- 扩容调用的是grow()方法，通过grow()方法中调用的Arrays.copyof()方法进行对原数组的复制，
在通过调用System.arraycopy()方法进行复制，达到扩容的目的
- 新的数组的大小为原先的大小+原先大小的一半，也就是1.5倍的原先大小
- 属于线性复杂度
- [ArrayList扩容详解]:https://blog.csdn.net/tiantiandjava/article/details/80677823
[ArrayList扩容详解]

## 3 Set
Set &nbsp; 不允许有重复元素的集合（通过hashcode值在hash表中查找该元素是否有相同元素）

- 判断重复： equals方法

通过比较，实现Set功能

Java世界里第二个重要的约定：hashCode
- 同一个对象必须始终返回相同的hashCode
- 两个对象对的equals返回true，必须返回相同的hashCode
- 两个对象不等，也可能返回相同的hashCode

哈希算法
- 哈希就是一个单向的映射
    - 所有的hashCode都是一个int
    - 对象是无限的，所以无法将具体int值应映射到具体的对象
- 从任意对象到一个整数的hashCode

HashSet

- 最常用，最高效的Set实现
- 哈希算法使得HashSet更高效
- HashSet是无序的

LinkedHashSet 

- 与插入顺序相同

TreeSet
- 有序的集合
- 使用Comparable约定，认为排序相等的元素相等
- 红黑树
## 4   Map
Map的简介与实战
- C/U：put()/putAll()
- R:
    - get()/size()
    - containsKey()/containsValue()
        - containsKey():当前Map中是否包含某一个Key
        - containsValue（）：是否包含某个Value
    - KeySet()/values()/entrySet()
        - keySet():key的集合。
        - keySet组成的Set的集合中的元素与Map中组成的元素具有关联，删除或修改Map中的元素，keySet中的元素也会随之影响。
        - values：value集合。
        - entrySet（）：返回一个键值对。
- D:remove()/clear()
HashMap
- 基于哈希表的集合，具有快速的查找优势。
- HashSet就是HashMap
- HashMap扩容的过程 ：类似ArrayList扩容，创建新的Map。
- [HashMap扩容详解]:https://blog.csdn.net/login_sonata/article/details/76598675
[HashMap扩容详解]
- HashMap线程不安全性，HashSet的实现没有同步。可能出现死循环现象
    - 建议使用ConcurrentHashMap:并发的HashMap
    -Jdk7之后 将链表改为使用红黑树算法提高性能。为了处理同一个hash桶里的元素碰撞问题 。

## 5 Guava
- Lists/Sets/Maps
- ImmutableMap/ImmutableSet(不可变的集合)
- Multiset/Multimap（显示相同元素次数/一个Kay对应多个value）
- BiMap（双向键值对，可以通过value映射到Key）

## 6  List排序常用方法

三种按照属性中的某一个字段排序的方法

### 6.1 stream写法
```
//按提交时间降序--stream写法
//根据参数查询符合的实体列表
List<Company> companyList =this.mapper.selectCompany(param);
//根据创建时间倒排
companyList = companyList.stream().sorted(Comparator.comparing(Company::getCreateTime).reversed()).collect(Collectors.toList());

```

### 6.2 Collections写法
```
//按提交时间降序 --Lamdba表达式
Collections.sort(companyList, (a, b) -> b.getCreateTime().compareTo(a.getCreateTime()));
```

### 6.3 借助工具类—常规写法
```
/按提交时间降序--工具类写法
SortListUtil.sort(companyList,"createTime","desc");
```
### 6.4 示例

```java
package com.github.hcsp.collection;

import java.util.*;
import java.util.stream.Collectors;

public class Main {
    // 请编写一个方法，对传入的List<User>进行如下处理：
    // 返回一个从部门名到这个部门的所有用户的映射。同一个部门的用户按照年龄进行从小到大排序。
    // 例如，传入的users是[{name=张三, department=技术部, age=40 }, {name=李四, department=技术部, age=30 },
    // {name=王五, department=市场部, age=40 }]
    // 返回如下映射：
    //    技术部 -> [{name=李四, department=技术部, age=30 }, {name=张三, department=技术部, age=40 }]
    //    市场部 -> [{name=王五, department=市场部, age=40 }]
    public static Map<String, List<User>> collect(List<User> users) {
        Map<String, List<User>> map = new HashMap<>();
        //将所有用户按年龄从小到大排序
        List<User> list = users.stream().sorted(Comparator.comparing(User::getAge)).collect(Collectors.toList());
        for (User user : list) {
            //当前的用户所在的部门是否存在
            //如果存在就将当前用户添加到对应的list中
            if (map.containsKey(user.getDepartment())) {
                map.get(user.getDepartment()).add(user);
            } else {
                //如果不存在就向Map中添加一个新的键值
                map.put(user.getDepartment(), new ArrayList<>(Collections.singleton(user)));
            }
        }
        return map;
    }

    public static void main(String[] args) {
        System.out.println(
                collect(
                        Arrays.asList(
                                new User(1, "张三", 40, "技术部"),
                                new User(2, "李四", 30, "技术部"),
                                new User(3, "王五", 40, "市场部"))));
    }
}
```
User
```java
package com.github.hcsp.collection;

import java.util.Objects;

public class User {
    // 用户的id
    private final Integer id;
    // 用户的姓名
    private final String name;
    // 用户的年龄
    private final int age;
    // 用户的部门，例如"技术部"/"市场部"
    private final String department;

    public User(Integer id, String name, int age, String department) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.department = department;
    }

    public Integer getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getDepartment() {
        return department;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        User person = (User) o;
        return Objects.equals(id, person.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }

    @Override
    public String toString() {
        return department+"->"+"";
    }
}
```
[comment]: <> (## 7 待研究_Collection常见问题)

[comment]: <>  (### 7.1  Collection体系的常用类及其背后的数据结构)

[comment]: <>  (### 7.2  Collection体系的常用类及其背后的数据结构对比)

[comment]: <>  (### 7.3  ArrayList源码阅读)

[comment]: <>  (### 7.4  ArrayList是如何扩容的？)

[comment]: <>  (### 7.5  HashMap源码阅读)

[comment]: <>  (### 7.6  HashMap是如何扩容的？)

[comment]: <>  ( ### 7.7  HashMap从Java7到Java8发生了哪些变化？)

[comment]: <>  (### 7.8  为什么HashMap不是线程安全的？)


                         
               
               
               
               
               
               
               
               
              
               
               





