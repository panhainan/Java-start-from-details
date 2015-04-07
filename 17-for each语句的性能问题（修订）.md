<font size="4">
关于Java5中的for each语句的性能问题

今天在**LeetCode**上做全排列的算法题，当时在我的eclipse里面已经得出正确结果了；但是，我在提交到Leetcode上去的时候，报了**“Time Limit Exceeded”**的结果。

<a href="https://leetcode.com/problems/permutations-ii/" target="_blank">Leetcode上的permutation问题</a>

我开始很纳闷，在Leetcode里面搜索了很多遇到类似的问题，但是没有找到与自己遇到的问题差不多的。很多都是说最后都是自己在自己代码里面检查出了错误。于是乎我就去仔细检查我的代码问题，后面发现把我的一个循环打印List的代码去掉后再次提交就成功了，当时我觉得很奇怪，但是有不确定是什么原因，就暂时记下了。

晚上看到很多人说 **Evernote** 很好用，本来我已经放弃了 Evernote 的了，改用**为知笔记**的了；但是好奇心驱使去再尝试一下 Evernote，于是乎我去了我的印象笔记，忽然间看到了以前收藏的一篇博客，讲的是 <a href="http://pda158.iteye.com/blog/2158898" target="_blank">**Java list三种遍历方法性能比较**</a> ，我想起今天遇到了一个类似的问题。于是乎就去看了，然后自己测试了一下，发现唉，真的如博主所说呢，<strike>Java5语言支持的新语法，代码最简洁，但是性能确实是最差的；</strike>然后再想想白天遇到的问题，就很明白了。


下面是测试List遍历性能的代码：

	package org.phn;
	
	import java.util.ArrayList;
	import java.util.Iterator;
	import java.util.List;
	
	public class TestListPrintTime {
	
		public static void main(String[] args) {
			List<String> list = new ArrayList<String>();
			long t1, t2;
			for (int j = 0; j < 10000000; j++) {
				list.add("aaaaaa" + j);
			}
			System.out.println("List first visit method:");
			t1 = System.currentTimeMillis();
			for (String tmp : list) {
				// System.out.println(tmp);
			}
			t2 = System.currentTimeMillis();
			System.out.println("Run Time:" + (t2 - t1) + "(ms)");
			System.out.println("List second visit method:");
			t1 = System.currentTimeMillis();
			for (int i = 0; i < list.size(); i++) {
				list.get(i);
				// System.out.println(list.get(i));
			}
			t2 = System.currentTimeMillis();
			System.out.println("Run Time:" + (t2 - t1) + "(ms)");
			System.out.println("List Third visit method:");
			Iterator<String> iter = list.iterator();
			t1 = System.currentTimeMillis();
			while (iter.hasNext()) {
				iter.next();
				// System.out.println(iter.next());
			}
			t2 = System.currentTimeMillis();
			System.out.println("Run Time:" + (t2 - t1) + "(ms)");
			System.out.println("Finished!!!!!!!!");
		}
	}
	
结果：

![](http://i.imgur.com/r6sRD4R.png)


<strike>可以看出Java5中的for each语句用起来确实是很方便、清楚，但是性能却差了好几倍啊，各位谨慎使用哦。留个心，可能以后遇到的一些问题就是它引起的哦。</strike>




----------
补充，这里会出现上述情况是因为**“for each做了强制转换和变量赋值”**我们可以这样做，在其他两个循环里相应增加赋值转换

    String s = list.get(i);

	String s = iter.next();

这样就会得出结果大致相同。大家可以看看

附上修改后的代码：

    package org.phn;
    
    import java.util.ArrayList;
    import java.util.Iterator;
    import java.util.List;
    
    public class TestListPrintTime {
    
    	public static void main(String[] args) {
    		List<String> list = new ArrayList<String>();
    		long t1, t2;
    		for (int j = 0; j < 1000000; j++) {
    			list.add("a" + j);
    		}
    		System.out.println("List first visit method:");
    		t1 = System.currentTimeMillis();
    		for (String s : list) {
    //			System.out.print(tmp);
    		}
    		t2 = System.currentTimeMillis();
    		System.out.println("\nRun Time:" + (t2 - t1) + "(ms)");
    		System.out.println("List second visit method:");
    		t1 = System.currentTimeMillis();
    		for (int i = 0; i < list.size(); i++) {
    			String s =  list.get(i);
    //			System.out.print(s);
    		}
    		t2 = System.currentTimeMillis();
    		System.out.println("\nRun Time:" + (t2 - t1) + "(ms)");
    		System.out.println("List Third visit method:");
    		Iterator<String> iter = list.iterator();
    		t1 = System.currentTimeMillis();
    		while (iter.hasNext()) {
    			String s = iter.next();
    //			System.out.print(s);
    		}
    		t2 = System.currentTimeMillis();
    		System.out.println("\nRun Time:" + (t2 - t1) + "(ms)");
    		System.out.println("Finished!!!!!!!!");
    	}
    }
    
结果

![](http://i.imgur.com/4yG3XIs.png)

**可以看出其实几种循环语句的性能其实都是相差不大的，怪楼主多想了。抱歉了各位。**

</font>