## Fragment already added问题的一些总结

1. 错误日志会显示出哪个fragment重复添加

		java.lang.IllegalStateException: Fragment already added: NativeAdFragment

找到这个fragment添加的地方进行即可定位问题所在点，但是打包的代码是被混淆的，所以需要找到mapping文件来定位是哪个fragment

> mapping.txt   列出了原始的类，方法和字段名与混淆后代码间的映射

> mapping目录在  \app\build\outputs\mapping\release


2. 这个错误会定位到executePendingTransactions，为什么呢？发生错误的地方并不是这里，但为什么会定位到这个地方？

看一下源码中对executePendingTransactions的解释：

>   /**
     * After a {@link FragmentTransaction} is committed with
     * {@link FragmentTransaction#commit FragmentTransaction.commit()}, it
     * is scheduled to be executed asynchronously on the process's main thread.
     * If you want to immediately executing any such pending operations, you
     * can call this function (only from the main thread) to do so.  Note that
     * all callbacks and other related behavior will be done from within this
     * call, so be careful about where this is called from.
     *
     * @return Returns true if there were any pending transactions to be
     * executed.
     */	
     
其中有一句是**all callbacks and other related behavior will be done from within this call** 意思是说当调用**executePendingTransactions**的时候其他所有的没有被执行的事务都会被立刻执行，我们之前添加fragment的那个地方由于是调用的commit，因为它并不会被立刻执行，所以也会在这个时候被执行，也难怪会在这个地方报重复添加的错误

3. 修改方式

1. 由于isadded经常判断无效，因为commit不是立刻执行的，让commit立刻执行即可，即调用commit之后调用executePendingTransactions
2. 找到重复添加的代码，去掉他


	

	