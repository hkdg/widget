#JavaScript大整数的位比较
>Javascript好多坑

业务上有个需求是做标签配置，一个条目可以配多个标签。因为标签id递增整数，且不会多。为了方便查询，内容表的标签字段用三个bigint字段表示，一个字段可以表示六十个标签。标签id1-60的存第一个字段，61-120存第二个字段，121-180存第三个字段。以第一个字段为例，包含id为1、3的标签对应二进制101，转换成十进制存进字段。这样就避免了表连接查询。（后台同学的逻辑思维前端同学表示很烧脑）
具体存取转换这里不做讨论，在条目展示的时候，取出来的标签字段值跟标签id需要做一次&的操作，判断条目有多少个标签。发现数值过大的时候，&操作的结果总是0。查了下资料发现对于位运算，JavaScript仅支持32位整数型，这就坑爹了。如下是在谷歌浏览器控制台的测试结果：
Math.pow(2,31)&Math.pow(2,31)
-2147483648
Math.pow(2,30)&Math.pow(2,30)
1073741824

对于过大的整数按位比较原生JavaScript不支持。网上没搜到合适解决方案，只能自己做实现。

思路是将十进制转换成二进制，然后按30长度分段，对每段再转换成十进制进行按位比较