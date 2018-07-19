# 咕泡学院

##策略模式

比较器、旅行路线、固定算法、买东西结算支付。

### 场景

根据用户的需求，处理数据时候需要对算法做出选择，固定的一些算法（不再发生变化的算法），拓展。

客户本身他不知道要采用什么样的算法计算，他可以选择既有的算法。（switch case ...）

枚举类可以看成在类之外无法创建新对象的普通类。

标准的策略模式由用户来选择，**实现内部不使用if/else**。

###demo

业务代码调用Order.pay()时，可以灵活选择PayType枚举类里提供的各种支付策略，对当前Order进行支付。

```java
/**
 * 姑且把这个枚举当做一个常量去维护
 * Created by Tom on 2018/3/11.
 */

public enum PayType {
    ALI_PAY(new AliPay()),
    WECHAT_PAY(new WechatPay()),
    UNION_PAY(new UnionPay()),
    JD_PAY(new JDPay());
    private Payment payment;  //枚举类也可以有私有成员
    PayType(Payment payment){   //枚举类也可以有构造方法
        this.payment = payment;
    }
    
    public Payment get(){ return  this.payment;} //枚举类也可以有普通成员方法，
                                                 // 代替之前的switch...case
}

public class Order {
    private String uid;
    private String orderId;
    private double amount;

    public Order(String uid,String orderId,double amount){
        this.uid = uid;
        this.orderId = orderId;
        this.amount = amount;
    }
    //这个参数，完全可以用Payment这个接口来代替
    //为什么没有用接口来代理？而用枚举类代替？
    //完美地解决了switch的过程，不需要在代码逻辑中写switch了
    //更不需要写if    else if，这就是策略模式的关键
    public PayState pay(PayType payType){
        return payType.get().pay(this.uid,this.amount);  // 这里调用枚举类的get()方法，
                                                         // 不是枚举类里的产量
    }
}

```

用户代码：

```java
//省略把商品添加到购物车，再从购物车下单
//直接从点单开始
Order order = new Order("1","20180311001000009",324.45);
//开始支付，选择微信支付、支付宝、银联卡、京东白条、财付通
//每个渠道它支付的具体算法是不一样的
//基本算法固定的
//这个值是在支付的时候才决定用哪个值（客户在这里做出的选择）
System.out.println(order.pay(PayType.WECHAT_PAY));  //用户代码很简洁
```

比较器也是一种策略模式，比如：

```java
new ArrayList<Object>().sort(new Comparator() {
    @Override
    public int compare(Object o1, Object o2) {
        return 0;
    }
});
```





