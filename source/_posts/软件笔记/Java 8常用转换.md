# 1. List转Map

```
/**
 * List -> Map
 * 需要注意的是：
 * toMap 如果集合对象有重复的key，会报错Duplicate key ....
 *  apple1,apple12的id都为1。
 *  可以用 (k1,k2)->k1 来设置，如果有重复的key,则保留key1,舍弃key2
 */
Map<Integer, Apple> appleMap = appleList.stream().collect(Collectors.toMap(Apple::getId, a -> a,(k1,k2)->k1));
```

# List分组成Map

```
//List 以ID分组 Map<Integer,List<Apple>>
Map<Integer, List<Apple>> groupBy = appleList.stream().collect(Collectors.groupingBy(Apple::getId));
 
System.err.println("groupBy:"+groupBy);
{1=[Apple{id=1, name='苹果1', money=3.25, num=10}, Apple{id=1, name='苹果2', money=1.35, num=20}], 2=[Apple{id=2, name='香蕉', money=2.89, num=30}], 3=[Apple{id=3, name='荔枝', money=9.99, num=40}]}

```

# 3. Map转List

```
List<Long> skuIdList = order.getItemList().stream().map(OrderItemDTO::getSkuId).collect(Collectors.toList());
```

# 4. 统计求和

```

//计算 总金额
BigDecimal totalMoney = appleList.stream().map(Apple::getMoney).reduce(BigDecimal.ZERO, BigDecimal::add);
System.err.println("totalMoney:"+totalMoney);  //totalMoney:17.48
```

# 5. 最大值、最小值

```

Optional<Dish> maxDish = Dish.menu.stream().
      collect(Collectors.maxBy(Comparator.comparing(Dish::getCalories)));
maxDish.ifPresent(System.out::println);
 
Optional<Dish> minDish = Dish.menu.stream().
      collect(Collectors.minBy(Comparator.comparing(Dish::getCalories)));

```


# 6. 过滤Map

```
//Map -> Stream -> Filter -> MAP
	Map<Integer, String> collect = map.entrySet().stream()
		.filter(x -> x.getKey() == 2)
		.collect(Collectors.toMap(x -> x.getKey(), x -> x.getValue()));
```