<font size="4">
- length()是字符串的方法，返回字符串的长度

- length属性是针对数组说的,比如说你声明了一个数组,想知道这个数组的长度则用到了length这个属性

- size()方法是针对泛型集合说的,如果想看这个泛型有多少个元素,就调用此方法来查看



实例

    package org.phn.java.typeconversion;
    import java.util.ArrayList;
    import java.util.List;
    /**
     * @author phn
     * @TODO
     */
    public class DataTypeConversion {
    	public static void main(String[] args) {
    		String[] slist ={"asd","wrdffdsg"};
    		System.out.println(slist.length);
    		System.out.println(slist[0].length());
    		List<String> slists = new ArrayList<String>();
    		slists.add("asdfasfd");
    		System.out.println(slists.size());
    		String s = slists.get(0);
    		System.out.println(s.length());
    	}
    } 

结果

![](http://i.imgur.com/rUI40bf.png)