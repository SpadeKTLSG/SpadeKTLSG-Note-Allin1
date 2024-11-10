‍

‍

Guava Cache

[Link](https://github.com/google/guava/wiki/CachesExplained)

‍

* 全内存的本地缓存实现
* 高性能且功能丰富
* 线程安全，操作简单 (底层实现机制类似ConcurrentMap)

‍

‍

# 旧式

‍

‍

* 添加依赖

  ```
      <!--guava依赖包-->
      <dependency>
        <groupId>com.google.guava</groupId>
        <artifactId>guava</artifactId>
        <version>19.0</version>
      </dependency>
  ```
* 封装api

  ```
   private Cache<String,Object> tenMinuteCache = CacheBuilder.newBuilder()

              //设置缓存初始大小，应该合理设置，后续会扩容
              .initialCapacity(10)
              //最大值
              .maximumSize(100)
              //并发数设置
              .concurrencyLevel(5)

              //缓存过期时间，写入后10分钟过期
              .expireAfterWrite(600,TimeUnit.SECONDS)

              //统计缓存命中率
              .recordStats()

              .build();


      public Cache<String, Object> getTenMinuteCache() {
          return tenMinuteCache;
      }

      public void setTenMinuteCache(Cache<String, Object> tenMinuteCache) {
          this.tenMinuteCache = tenMinuteCache;
      }
  ```

  ‍

‍

## 布隆过滤器

‍

依赖

```xml
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>28.0-jre</version>
</dependency>
```

‍

指定误判率为（0.01）

```java
public static void main(String[] args) {
    // 创建布隆过滤器对象
    BloomFilter<Integer> filter = BloomFilter.create(
        Funnels.integerFunnel(),
        1500,
        0.01);
    // 判断指定元素是否存在
    System.out.println(filter.mightContain(1));
    System.out.println(filter.mightContain(2));
    // 将元素添加进布隆过滤器
    filter.put(1);
    filter.put(2);
    System.out.println(filter.mightContain(1));
    System.out.println(filter.mightContain(2));
}
```

‍

```java
class MyBloomFilter {
    //布隆过滤器容量
    private static final int DEFAULT_SIZE = 2 << 28;
    //bit数组，用来存放key
    private static BitSet bitSet = new BitSet(DEFAULT_SIZE);
    //后面hash函数会用到，用来生成不同的hash值，随意设置
    private static final int[] ints = {1, 6, 16, 38, 58, 68};

    //add方法，计算出key的hash值，并将对应下标置为true
    public void add(Object key) {
        Arrays.stream(ints).forEach(i -> bitSet.set(hash(key, i)));
    }

    //判断key是否存在，true不一定说明key存在，但是false一定说明不存在
    public boolean isContain(Object key) {
        boolean result = true;
        for (int i : ints) {
            //短路与，只要有一个bit位为false，则返回false
            result = result && bitSet.get(hash(key, i));
        }
        return result;
    }

    //hash函数，借鉴了hashmap的扰动算法
    private int hash(Object key, int i) {
        int h;
        return key == null ? 0 : (i * (DEFAULT_SIZE - 1) & ((h = key.hashCode()) ^ (h >>> 16)));
    }
}
```
