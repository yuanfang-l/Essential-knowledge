# 锁类型：

- 可重入锁：在对象中所有同步方法不用再次获得锁

- 可中断锁：在等待获取锁的过程中可以中断当前线程

- 公平锁：等待获取锁的时间长的优先获取锁，队列效果

- 读写锁：读的时候可以多线程一起读，写的时候必须同步

  

# Synchronized

1. synchronized是java的关键字
2. 无法判断锁的状态
3. 可重入、不可中断、非公平锁
4. 不需要手动释放锁，在方法执行完自动释放锁

## 应用方式

- 修饰实例方法：作用于当前实例加锁，进入同步代码需要获得当前实例的锁

```
// 对当前实例加锁，访问当前实例同步代码必须要获得锁，访问当前实例非同步代码不受锁的影响	

    public void method(String str) {
		System.out.println("method===========" + str);
	}


	public synchronized void methodA(String str) throws Exception {
		System.out.println("methodA===========" + str);
		Thread.sleep(3000);
	}

	public synchronized void methodB(String str) throws Exception {
		System.out.println("methodB===========" + str);
		Thread.sleep(3000);
	}
```

 

- 修饰静态方法：作用于当前类对象加锁，进入同步代码需要获得当前类对象的锁

```
    // 锁的是当前类对象，跟当前类同为静态修饰的方法影响，跟非同步和其它同步修饰方法互不影响
	public synchronized static void methodC(String str) throws Exception {
		System.out.println("methodC===========" + str);
		Thread.sleep(3000);
	}
```

- 修饰代码块：指定加锁对象，对指定对象加锁，进入同步代码需要获得指定对象的锁

```
	//锁的是指定对象(str)

	public void methodD(String str) throws Exception {
		synchronized (str) {			
			System.out.println("methodD===========" + str);
			Thread.sleep(3000);
		}
	}
```

 

# Lock

1. 是一个接口
2. 可以判断锁的状态
3. 可重入、可中断、可公平也可非公平
4. 需要手动释放锁
5. 可能产生死锁问题

## 接口方法：

- lock()：获取锁，如果锁被使用则一直等待
- unlock()：释放锁
- tryLock()：返回boolean值，如果锁被占用就返回false，否则返回true
- tryLock(long time, TimeUnit unit)：返回boolean值，如果锁被占用，等待一定时间返回
- lockInterruptibly()：获取锁，线程在等待获取锁的时候，可以调用interrupt()方法中断线程

```
	// 非公平锁

	Lock lock = new ReentrantLock();

	// 公平锁
	
	Lock lock = new ReentrantLock(true);
```

## 示例：

```
public class TestReentrantLock {
	static Lock lock = new ReentrantLock(true);
	public static void main(String[] args) throws InterruptedException {
		ExecutorService exe = Executors.newFixedThreadPool(10);
		exe.execute(new Runnable() {

			@Override
			public void run() {
				testLock();
			}
		});
		exe.execute(new Runnable() {
			@Override
			public void run() {
				testTryLock();
			}
		});
		exe.execute(new Runnable() {
			@Override
			public void run() {
				try {
					testInterruptiblyLock();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
		});
		Thread thread = new Thread(new Runnable() {
			@Override
			public void run() {
				try {
					testInterruptiblyLock();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
		});

		thread.start();
		Thread.sleep(1000);
		thread.interrupt();
		exe.shutdown();
	}
	public static void testLock() {
		try {
			lock.lock();
			System.out.println("=======>testLock");
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		} finally {
			lock.unlock();
			System.out.println("===testLock====>释放锁");
		}
	}
	public static void testTryLock() {
		if (lock.tryLock()) {	
			try {
				System.out.println("=======>testTryLock");
				Thread.sleep(2000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			} finally {
				lock.unlock();
				System.out.println("=======>释放锁");
			}
		} else {
			System.out.println("===testTryLock====>没有获取锁");
		}
	}
	
	public static void testInterruptiblyLock() throws InterruptedException {
		lock.lockInterruptibly();
		try {
			System.out.println("=======>testInterruptiblyLock");
			Thread.sleep(3000);
		} finally {
			lock.unlock();
			System.out.println("==testInterruptiblyLock=====>释放锁");
		}
	}
	 
}
```

# 总结：

- Lock是一个接口，而synchronized是java中的关键字
- synchronized在发生异常时，会自动释放锁；而Lock在发生异常时需要调用unLock()方法释放锁，否则会造成死锁
- Lock可以让等待锁的线程响应中断，而synchronized会一直等待下去
- synchronized是非公平锁，多线程间竞争获取锁；而Lock可以做到公平锁，线程排除获取锁，也可以非公平
- 在性能上来说，如果竞争资源不激烈，两者性能差不多，当大量线程同时竞争时，Lock性能远远高于synchronized。