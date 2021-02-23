# 1、Lock锁（重点）

>传统Synchrongazied



>Lock接口

![image-20200823224418967](.\typora-user-images\image-20200823224418967.png)

![image-20200823224515147](.\typora-user-images\image-20200823224515147.png)

公平锁：十分公平，可以先来后到；
**非公平锁：十分不公平，可以插队（默认）；**

>##### Synchrongazied与Lock的区别

1、Synchrongazied是java内置的关键字，Lock是一个Java类。

2、Synchrongazied无法判断是否获取锁的状态，Lock可以判断是否获取到锁

3、Synchrongazied会自动释放锁，Lock必须手动释放，否则会造成   **死锁**

4、Synchrongazied  线程1（获得锁，等待）线程2（等待，一直等待）；Lock锁不一定会等待下去；

5、Synchrongazied可重入锁，不可以中断的,非公平; Lock , 可重入锁,可以判断锁,非公平(可以自己设置) ;

6、Synchrongazied适合锁少量的代码同步问题，Lock适合锁大量的同步代码； 

>锁是什么，如何判断锁的是谁

# 2、生产者消费者问题

> 生产者和消费者Synchrongazied版

 ```java
//判断等待，业务，通知
class Data{
    private int number = 0;

        //+1
    public synchronized void increment() throws InterruptedException {

        if (number!=0){
            //等待
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);

        this.notifyAll();    }
        //-1
    public  synchronized void decrement() throws InterruptedException {
        if(number==0){
            //等待
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);

        this.notifyAll();
    }


}
 ```





> 多个线程会出现虚假唤醒



![image-20200824212947807](.\typora-user-images\image-20200824212947807.png)

**if改为while**

```java
class Data{
    private int number = 0;

        //+1
    public synchronized void increment() throws InterruptedException {

        while (number!=0){
            //等待
            this.wait();
        }
        number++;
        System.out.println(Thread.currentThread().getName()+"=>"+number);

        this.notifyAll();    }
        //-1
    public  synchronized void decrement() throws InterruptedException {
        while (number==0){
            //等待
            this.wait();
        }
        number--;
        System.out.println(Thread.currentThread().getName()+"=>"+number);

        this.notifyAll();
    }


}
```

> JUC版的生产者和消费者的问题

通过Lock找到Condition

![image-20200824213744127](.\typora-user-images\image-20200824213744127.png)

![image-20200824213958697](.\typora-user-images\image-20200824213958697.png)

代码实现 

```javs
class Data2{
    private int number = 0;
    Lock lock = new ReentrantLock();
    Condition condition = lock.newCondition();

    //+1
    public void increment() throws InterruptedException {
        try {
            lock.lock();
            while (number!=0){
                //等待
                condition.await();
            }
            number++;
            System.out.println(Thread.currentThread().getName()+"=>"+number);
            condition.signalAll();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }

    }
    //-1
    public  void decrement() throws InterruptedException {
        try {
            lock.lock();
            while (number==0){
                //等待
                condition.await();
            }
            number--;
            System.out.println(Thread.currentThread().getName()+"=>"+number);
            condition.signalAll();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }


    }
}
```



> Condition 精准的通知和唤醒线程

```java
class Data3{
    private Lock lock = new ReentrantLock();
    Condition condition1 = lock.newCondition();
    Condition condition2 = lock.newCondition();
    Condition condition3= lock.newCondition();
    private int number = 1;


    public void printA(){
        lock.lock();
        try {
            while (number != 1){
                //等待
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>AAAAAAAAAAAAAA");
            number = 2;
            condition2.signal();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }

    }
    public void printB(){
        lock.lock();
        try {
            while (number != 2){
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>BBBBBBBBBBBB");
            number = 3;
            condition3.signal();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
    public void printC(){
        lock.lock();
        try {
            while (number != 3){
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName()+"=>CCCCCCCCC");
            number = 1;
            condition1.signal();

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }


}
```

# 3、8锁现象

如何判断锁的是谁!永远的知道什么锁,锁到底锁的是谁!
对象、Class



## 

> 1.synchronized 锁的对象是方法的调用者！

> 2.两个方法使用同一个对象谁先拿到谁执行!

> 3.不加锁的普通方法不受锁的影响！

> 4.两个对象，两个同步方法调用不同对象，获得两把锁，不会相互影响，只看等待时间先后！

> 5.static 类一加载就形成了，锁的对象是反射的Class类模板！





> **锁的到底是什么**

**new 出来 锁的是具体资源对象！！**

**static 出来 锁的是Class模板！！**

















