### synchronized 和 Lock 有什么区别？用新的 Lock 有什么好处？
1. 原始构成
    
    synchronized 是关键字，属于 JVM 层面
        monitorenter (底层是通过 monitor 对象来完成的，其实 wait/notify 等方法也依赖与 monitor 对象，只有在同步块或者方法中才能调用 wait/notify 等方法)
        monitorexit
        
    Lock 是具体的类(java.util.concurrent.locks.Lock)是 api 层面的锁。
    
2. 使用方法
    
    synchronized 不需要用户手动释放锁，当 synchronized 代码执行结束后系统会自动让线程释放对锁的占用
    
    ReentrantLock 则需要用户手动释放锁，若没有主动释放锁，就有可能出现死锁现象，需要 lock() 和 unlock() 方法配合 try/finally 语句块来完成
    
3. 等待是否可中断
    synchronized 不可中断，除非抛出异常或者正常运行完成
    
    ReentrantLock 可中断
     - 设置超时方法 tryLock(long timeout, TimeUnit unit)
     -  lockInterruptibly() 放代码块中，调用 interrupted() 方法可中断
     
4. 加锁是否公平
    
    synchronized 非公平锁
    
    ReentrantLock 两者都可以，默认非公平锁，构造方法可以传 boolean 值，true 为公平锁、false 为非公平锁
    
5. 锁绑定多个条件 condition
    
    synchronized 没有
    
    ReentrantLock 用来实现分组唤醒需要唤醒的线程们，可以精确唤醒，而不像 synchronized 要么随机唤醒一个线程、要么唤醒全部线程