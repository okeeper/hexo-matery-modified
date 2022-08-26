---
title: Java bean的Getter Setter 自动编译生成工具Lombok
date: 2022-08-26 15:54:13
author: okeeper
top: false
toc: true
categories: 软件笔记
tags:
  - 软件笔记
---

## 背景

  我们在开发过程中，通常都会定义大量的JavaBean，然后通过IDE去生成其属性的构造器、getter、setter、equals、hashcode、toString方法，当要对某个属性进行改变时，比如命名、类型等，都需要重新去生成上面提到的这些方法，那Java中有没有一种方式能够避免这种重复的劳动呢？答案是有，lombok插件
  
## Idea插件安装lombok
1. File > Settings > Plugins > Browse repositories... > Search for "lombok" > Install Plugin
2. 在使用项目中引入lombok 的类库
```
<dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.16.10</version>
        <scope>provided</scope>
    </dependency>
```

## 使用

**@Getter / @Setter**

  可以作用在类上和属性上，放在类上，会对所有的非静态(non-static)属性生成Getter/Setter方法，放在属性上，会对该属性生成Getter/Setter方法。并可以指定Getter/Setter方法的访问级别。

**@EqualsAndHashCode**

  默认情况下，会使用所有非瞬态(non-transient)和非静态(non-static)字段来生成equals和hascode方法，也可以指定具体使用哪些属性。

**@ToString**

  生成toString方法，默认情况下，会输出类名、所有属性，属性会按照顺序输出，以逗号分割。

**@NoArgsConstructor, @RequiredArgsConstructor and @AllArgsConstructor**

  无参构造器、部分参数构造器、全参构造器，当我们需要重载多个构造器的时候，Lombok就无能为力了。

**@Data**

  @ToString, @EqualsAndHashCode, 所有属性的@Getter, 所有non-final属性的@Setter和@RequiredArgsConstructor的组合，通常情况下，我们使用这个注解就足够了。
  
  ## 原理
  该插件的原理是在编码过程中屏蔽bean的getter/setter 方法，在编译成class的时候插件自动根据注解生成对应的gettter/setter 方法，我们通过发编译源码发现
  
```
@Data
public class Test {
    private String id;
    private String name;

    public void test(){
        this.getId();
    }
}
```
编译后等价于

```
public class Test {
    private String id;
    private String name;

    public void test() {
        this.getId();
    }

    public Test() {
    }

    public String getId() {
        return this.id;
    }

    public String getName() {
        return this.name;
    }

    public void setId(String id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public boolean equals(Object o) {
        if(o == this) {
            return true;
        } else if(!(o instanceof Test)) {
            return false;
        } else {
            Test other = (Test)o;
            if(!other.canEqual(this)) {
                return false;
            } else {
                String this$id = this.getId();
                String other$id = other.getId();
                if(this$id == null) {
                    if(other$id != null) {
                        return false;
                    }
                } else if(!this$id.equals(other$id)) {
                    return false;
                }

                String this$name = this.getName();
                String other$name = other.getName();
                if(this$name == null) {
                    if(other$name == null) {
                        return true;
                    }
                } else if(this$name.equals(other$name)) {
                    return true;
                }

                return false;
            }
        }
    }

    protected boolean canEqual(Object other) {
        return other instanceof Test;
    }

    public int hashCode() {
        boolean PRIME = true;
        byte result = 1;
        String $id = this.getId();
        int result1 = result * 59 + ($id == null?43:$id.hashCode());
        String $name = this.getName();
        result1 = result1 * 59 + ($name == null?43:$name.hashCode());
        return result1;
    }

    public String toString() {
        return "Test(id=" + this.getId() + ", name=" + this.getName() + ")";
    }
}

```