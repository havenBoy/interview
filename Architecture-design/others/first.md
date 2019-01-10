# 如何防止订单的重复提交
> 订单的重复提交导致用户的体验不佳，会有有人有意或者无意的提交订单，导致订单的重复支付问题则是最为严重的

* 实现的方法
  - 一般来说如果已经做了防止订单重复提交的系统会提示订单超时或者订单错误请刷新网页后重新提交；
  - 使用一个带有有效时间的值作为计时器，redis作为介质，如果值存在表示不可再次生成订单，如果没有则会创建订单；
  - 使用spring security在页面生成token机制，当提交订单时，由订单的有效时间来判断订单是否允许提交，在高并发的情况下，只能有一个
线程来访问用户的token，使用过后就会清掉，之后token就不能提交；