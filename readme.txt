1.redis缓存详情页
第一节
（1）页面缓存：商品列表
String html = redisService.get(GoodsKey.getGoodsList, "", String.class);
if(html == null || html.length() <= 0) {
	List<GoodsVo> goodsList = goodsService.listGoodsVo();
	model.addAttribute("goodsList", goodsList);
	//手动渲染模板
	html = renderTemplate("goods_list", model, thymeleafViewResolver,request,response,applicationContext);
	if(html!=null && html.length() > 0) {
		redisService.set(GoodsKey.getGoodsList, "", html);
	}
}
（2）url缓存：商品详情
第二节
（3）对象缓存：用户token，getById改造
（4）压测商品列表，对比QPS

2. 页面静态化，把页面缓存到客户端

第一节：
（1）详情页静态化改造
static/goods_detail.htm
goods_list.html的跳转换成'/goods_detail.htm?goodsId='+${goods.id}
修改goods_detail.htm里面的js：
第二节：
（2）改造秒杀接口，演示浏览器缓存
修改配置：
spring.resources.add-mappings=true
spring.resources.cache-period= 3600
spring.resources.chain.cache=true 
spring.resources.chain.enabled=true
spring.resources.chain.gzipped=true
spring.resources.chain.html-application-cache=true
spring.resources.static-locations=classpath:/static/
第三节：
修改减少库存的SQL
压测秒杀接口， 同一个token  同一个goodsId,20个并发  演示卖超


synchronized()? redis分布式锁？

下一章对接口进行优化！

3.静态资源优化
（1）压缩
（2）合并
（3）CDN就近访问

