# COS相关订单服务和支付服务开发内容梳理

* 接口总计：29个，其中订单服务14 个；支付服务15 个。

### 订单服务

* 商品详情页，添加商品到购物车接口（分登录和未登录状态），包含：创建购物车和购物车条目(商品)两个接口，0.5天；
* 购物车列表页，更新购物车商品信息接口（分登录和未登录状态），0.5天；
* 购物车列表页，查看购物车列表接口（分登录和未登录状态），包含：查询商品及其sku信息接口，0.5天；
* 我的页面，查购物车商品数量的接口（分登录和未登录状态）；
* 结算页，获取待提交订单详情接口，必须登录，包含：获取当前会员信息接口，获取会员地址信息接口，查看购物车列表接口，查看商品sku信息接口；还需考虑特殊商品的相关处理：比如，门票类商品，需要查看或新增出行人信息接口等等；初探有些复杂，还需细看，2天；
* 结算页，提交订单接口（生成会员订单接口，不拆单），1天：
* 选择支付页，查询主订单信息接口，0.5天；
* 我的页面，查看当前会员各种状态（待付款、待发货、待收货、待评价、待自提等）订单数量的接口，1天；
* 我的订单列表页，查看会员各状态订单列表接口（特别注意：待付款状态是未拆分订单，其他状态都是已拆分订单）,包含：获取当前会员信息接口，查看商品sku信息接口；已有，但需要微调，1天；
* 订单详情页，查看订单详情接口（特别注意，待付款状态是未拆分订单，其他状态都是已拆分订单），包含：查看订单行列表接口，查看商品及其sku信息接口，还需考虑特殊商品订单相关信息的查询，如：门票类商品订单详情还需要查看出行人信息，如果是预约类的门票，还需查看预约时间等等；已有，但需考虑特殊情形，待细估，1天；
* 我的售后，会员退货订单列表接口，1天；
* 我的售后，退货订单详情接口，0.5天；
* 支付成功页，查看自提订单列表接口，0.5天；

### 支付服务

* 支付页，查询第三方支付表单信息接口（含微信支付、支付宝、银联支付、招行支付、块钱支付）；不同支付方式区别较大，还需细估，2.5天；
* 整理微信支付开发规范文档，现有流程：1，先调微信统一Oauth认证地址；2，认证后返回重定向淘璞支付准备页，调微信统一支付接口，支付成功，微信支付后台发送同步通知，异步调淘璞支付结果处理接口，并返回签名数据；3，进行微信支付签名认证，返回微信支付成功页；1天；
* 微信支付需要接口：微信支付调用封装接口；微信支付结果通知处理接口；支付成功订单统一处理接口（拆分订单）；另外还有查看订单信息和自提订单列表信息的接口；3天；
* 整理支付宝开发规范文档，现有流程：封装统一支付页面，请求支付网关进行支付，交易结束发送后台异步通知处理订单支付信息，支付成功返回支付成功页；1天；
* 支付宝支付需要接口：统一支付页面封装接口；异步通知订单处理接口；查看订单及其订单列表接口；2天；
* 整理银联支付开发规范文档，现有流程：后端组装支付表单提交页；支付，支付成功后银联异步通知淘璞处理订单，返回银联支付结果；返回商户查看订单支付结果；1天；
* 银联支付相关接口：支付表单信息组装接口；异步通知处理订单接口；返回商户查看订单支付详情接口；2天；
* 整理招行支付开发规范文档，现有流程：组装支付表单参数；提交招行Wap支付，支付成功后发送后台异步通知处理订单信息；返回商户查看订单支付信息；
* 招行支付接口：支付表单信息组装接口；异步通知处理订单接口；返回商户查看订单支付详情接口；2天；
* 整理快钱开发规范文档，现有流程：组装支付表单参数；进入银行选择页面；填写银行卡信息；提交支付，支付成功后异步通知商户处理订单信息；返回商户查看订单支付信息；
* 快钱支付相关接口：支付表单信息组装接口；异步通知处理订单接口；返回商户查看订单支付详情接口；2天；
* 另外，原系统支付公共模块jar包如果无法直接使用，则需要整块迁移，3-5天；

### 待确认问题

* 添加商品到购物车是否需要登录认证？目前未登录的用户也可以添加商品到购物车。如果无需登录认证，则需要制定购物车及其商品存储方案，目前使用的是Cookie存储方案：前端存Cookieid，后端根据Cookieid存购物车及其商品信息。
* 原系统设计，如果会员的购物车里已经有商品，未登录状态购物车里的商品不会进入登录会员的购物车里去；
* 原系统，有支付基础公共模块：如处理各支付表单基础参数，订单统一处理函数等，新系统是否需要？如果无法集成jar包，则需要整块迁移。
* 支付页中，表单提交url和表单内容都是在后端组合后传给前端的，是否需要重写？如：银联支付，来源ChinaunionPayProcessor.java第205行。等等。思考：鉴于支付页返回有明细bug，是否考虑废弃统一的嵌套页，各自写自己的。
* 购物车是否需要单独开发服务；

* 补充：购物车清除接口；

