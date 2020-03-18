#### uuid

>UUID基本概念:
>UUID是指在一台机器上生成的数字，它保证对在同一时空中的所有机器都是唯一的。
>UUID组成部分:当前日期和时间+时钟序列+随机数+全局唯一的IEEE机器识别号
>全局唯一的IEEE机器识别号:如果有网卡，从网卡MAC地址获得，没有网卡以其他方式获得。
>UUID优缺点:
>优点:
>  简单，代码方便
>  生成ID性能非常好，基本不会有性能问题
>  全球唯一，在遇见数据迁移，系统数据合并，或者数据库变更等情况下，可以从容应对
>缺点:
>  没有排序，无法保证趋势递增
>  UUID往往是使用字符串存储，查询的效率比较低
>  存储空间比较大，如果是海量数据库，就需要考虑存储量的问题。
>  传输数据量大

```
  String s = UUID.randomUUID().toString().replaceAll("-", "");
  System.out.println(s);
  //
  String a=String.format("%1$05d", 1);
  System.out.println(a);
  
```

#### mysql

>实现思路：利用数据库自增或者序列号方式实现订单号
>注意：在数据库集群环境下，默认自增方式存在问题，因为都是从1开始自增，可能会存在重复，应该设置每台不同数据库自增的间隔方式不同。
>优点:
>  简单，代码方便，性能可以接受。
>  数字ID天然排序，对分页或者需要排序的结果很有帮助。
>缺点:
> 不同数据库语法和实现不同，数据库迁移的时候或多数据库版本支持的时候需要处理。
> 在性能达不到要求的情况下，比较难于扩展。
> 在单个数据库或读写分离或一主多从的情况下，只有一个主库可以生成。有单点故障的风险。
> 分表分库的时候会有麻烦。

**如何设置数据库集群**

```
在数据库集群环境下，默认自增方式存在问题，因为都是从1开始自增，可能会存在重复，应该设置每台节点自增步长不同。
查询自增的步长
SHOW VARIABLES LIKE 'auto_inc%'
修改自增的步长
SET @@auto_increment_increment=10;
修改起始值
SET @@auto_increment_offset=5;
假设有两台mysql数据库服务器
节点①自增  1 3 5 7 9 11 ….
节点②自增  2 4 6 8 10 12 ….
注意:在最开始设置好了每台节点自增方式步长后，确定好了mysql集群数量后，无法扩展新的mysql，不然生成步长的规则可能会发生变化。
```

#### redis

>比较适合使用Redis来生成每天从0开始的流水号。比如订单号=日期+当日自增长号。可以每天在Redis中生成一个Key，使用INCR进行累加。
>
>如果生成的订单号超过自增增长的话，可以采用前缀+自增+并且设置有效期
>
>

```
public String order(String key) {
		RedisAtomicLong redisAtomicLong = new RedisAtomicLong(key, redisTemplate.getConnectionFactory());
		for (int i = 0; i < 100; i++) {
			long incrementAndGet = redisAtomicLong.incrementAndGet();
			// 5位
			String orderId = prefix() + "-" + String.format("%1$05d", incrementAndGet);
			String orderSQL = "insert into orderNumber value('" + orderId + "');";
			System.out.println(orderSQL);
		}

		return "success";
	}
public static String prefix() {
		String temp_str = "";
		Date dt = new Date();
		// 最后的aa表示“上午”或“下午” HH表示24小时制 如果换成hh表示12小时制
		SimpleDateFormat sdf = new SimpleDateFormat("yyyyMMddHHmmss");
		temp_str = sdf.format(dt);
		return temp_str;
	}
```

#### Twitter的Snowflake算法生成全局id

>Twitter的snowflake(雪花)算法
>snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：
>高位随机+毫秒数+机器码（数据中心+机器id）+10位的流水号码
>Github地址: https://github.com/twitter-archive/snowflake
>Snowflake 原理:
>snowflake生产的ID是一个18位的long型数字，二进制结构表示如下(每部分用-分开):
>0 - 00000000 00000000 00000000 00000000 00000000 0 - 00000 - 00000 - 00000000 0000
>第一位未使用，接下来的41位为毫秒级时间(41位的长度可以使用69年，从1970-01-01 08:00:00)，然后是5位datacenterId（最大支持2^5＝32个，二进制表示从00000-11111，也即是十进制0-31），和5位workerId（最大支持2^5＝32个，原理同datacenterId），所以datacenterId*workerId最多支持部署1024个节点，最后12位是毫秒内的计数（12位的计数顺序号支持每个节点每毫秒产生2^12＝4096个ID序号）.所有位数加起来共64位，恰好是一个Long型（转换为字符串长度为18）.单台机器实例，通过时间戳保证前41位是唯一的，分布式系统多台机器实例下，通过对每个机器实例分配不同的datacenterId和workerId避免中间的10位碰撞。最后12位每毫秒从0递增生产ID，再提一次：每毫秒最多生成4096个ID，每秒可达4096000个

```
package com.mayikt.controller;

/*
 * Twitter_Snowflake<br>
 * SnowFlake的结构如下(每部分用-分开):<br>
 * 0 - 0000000000 0000000000 0000000000 0000000000 0 - 00000 - 00000 -
 * 000000000000 <br>
 * 1位标识，由于long基本类型在Java中是带符号的，最高位是符号位，正数是0，负数是1，所以id一般是正数，最高位是0<br>
 * 41位时间截(毫秒级)，注意，41位时间截不是存储当前时间的时间截，而是存储时间截的差值（当前时间截 - 开始时间截)
 * 得到的值），这里的的开始时间截，一般是我们的id生成器开始使用的时间，由我们程序来指定的（如下下面程序IdWorker类的startTime属性）。
 * 41位的时间截，可以使用69年，年T = (1L << 41) / (1000L * 60 * 60 * 24 * 365) = 69<br>
 * 10位的数据机器位，可以部署在1024个节点，包括5位datacenterId和5位workerId<br>
 * 12位序列，毫秒内的计数，12位的计数顺序号支持每个节点每毫秒(同一机器，同一时间截)产生4096个ID序号<br>
 * 加起来刚好64位，为一个Long型。<br>
 * SnowFlake的优点是，整体上按照时间自增排序，并且整个分布式系统内不会产生ID碰撞(由数据中心ID和机器ID作区分)，并且效率较高，经测试，
 * SnowFlake每秒能够产生26万ID左右。
 */
public class SnowflakeIdWorker {

	// ==============================Fields===========================================
	/** 开始时间截 (2015-01-01) */
	private final long twepoch = 1420041600000L;

	/** 机器id所占的位数 */
	private final long workerIdBits = 5L;

	/** 数据标识id所占的位数 */
	private final long datacenterIdBits = 5L;

	/** 支持的最大机器id，结果是31 (这个移位算法可以很快的计算出几位二进制数所能表示的最大十进制数) */
	private final long maxWorkerId = -1L ^ (-1L << workerIdBits);

	/** 支持的最大数据标识id，结果是31 */
	private final long maxDatacenterId = -1L ^ (-1L << datacenterIdBits);

	/** 序列在id中占的位数 */
	private final long sequenceBits = 12L;

	/** 机器ID向左移12位 */
	private final long workerIdShift = sequenceBits;

	/** 数据标识id向左移17位(12+5) */
	private final long datacenterIdShift = sequenceBits + workerIdBits;

	/** 时间截向左移22位(5+5+12) */
	private final long timestampLeftShift = sequenceBits + workerIdBits + datacenterIdBits;

	/** 生成序列的掩码，这里为4095 (0b111111111111=0xfff=4095) */
	private final long sequenceMask = -1L ^ (-1L << sequenceBits);

	/** 工作机器ID(0~31) */
	private long workerId;

	/** 数据中心ID(0~31) */
	private long datacenterId;

	/** 毫秒内序列(0~4095) */
	private long sequence = 0L;

	/** 上次生成ID的时间截 */
	private long lastTimestamp = -1L;

	// ==============================Constructors=====================================
	/**
	 * 构造函数
	 * 
	 * @param workerId
	 *            工作ID (0~31)
	 * @param datacenterId
	 *            数据中心ID (0~31)
	 */
	public SnowflakeIdWorker(long workerId, long datacenterId) {
		if (workerId > maxWorkerId || workerId < 0) {
			throw new IllegalArgumentException(
					String.format("worker Id can't be greater than %d or less than 0", maxWorkerId));
		}
		if (datacenterId > maxDatacenterId || datacenterId < 0) {
			throw new IllegalArgumentException(
					String.format("datacenter Id can't be greater than %d or less than 0", maxDatacenterId));
		}
		this.workerId = workerId;
		this.datacenterId = datacenterId;
	}

	// ==============================Methods==========================================
	/**
	 * 获得下一个ID (该方法是线程安全的)
	 * 
	 * @return SnowflakeId
	 */
	public synchronized long nextId() {
		long timestamp = timeGen();

		// 如果当前时间小于上一次ID生成的时间戳，说明系统时钟回退过这个时候应当抛出异常
		if (timestamp < lastTimestamp) {
			throw new RuntimeException(String.format(
					"Clock moved backwards.  Refusing to generate id for %d milliseconds", lastTimestamp - timestamp));
		}

		// 如果是同一时间生成的，则进行毫秒内序列
		if (lastTimestamp == timestamp) {
			sequence = (sequence + 1) & sequenceMask;
			// 毫秒内序列溢出
			if (sequence == 0) {
				// 阻塞到下一个毫秒,获得新的时间戳
				timestamp = tilNextMillis(lastTimestamp);
			}
		}
		// 时间戳改变，毫秒内序列重置
		else {
			sequence = 0L;
		}

		// 上次生成ID的时间截
		lastTimestamp = timestamp;

		// 移位并通过或运算拼到一起组成64位的ID
		return ((timestamp - twepoch) << timestampLeftShift) //
				| (datacenterId << datacenterIdShift) //
				| (workerId << workerIdShift) //
				| sequence;
	}

	/**
	 * 阻塞到下一个毫秒，直到获得新的时间戳
	 * 
	 * @param lastTimestamp
	 *            上次生成ID的时间截
	 * @return 当前时间戳
	 */
	protected long tilNextMillis(long lastTimestamp) {
		long timestamp = timeGen();
		while (timestamp <= lastTimestamp) {
			timestamp = timeGen();
		}
		return timestamp;
	}

	/**
	 * 返回以毫秒为单位的当前时间
	 * 
	 * @return 当前时间(毫秒)
	 */
	protected long timeGen() {
		return System.currentTimeMillis();
	}

	// ==============================Test=============================================
	/** 测试 */
	public static void main(String[] args) {
		SnowflakeIdWorker idWorker = new SnowflakeIdWorker(0, 0);
		for (int i = 0; i < 100; i++) {
			long id = idWorker.nextId();
			String insertSQL = "insert into orderNumber value('" + id + "');";
			System.out.println(insertSQL);
		}
	}
}
```

#### zookeeper

[zookeeper生成唯一id](https://www.cnblogs.com/xiufengchen/p/10339072.html)



