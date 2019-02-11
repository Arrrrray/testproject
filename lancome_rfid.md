
### 流程描述 ###
    购买:
    1. 用户选择使用 RFID 购买
    2. 导购将含有 RFID 的商品放到读卡区域
    3. 读取商品信息并在屏幕上生成二维码（该二维码是按照汉光规则生成）
    3. 用户使用微信扫描二维码进入小程序进行下单,购买
    4. 购买成功后，汉光推送支付成功的消息

    退款: 汉光客服处理

    对账: 使用手机对账


### RFID 购买接口 ###
共需要一个接口和一个二维码的生成规则：如下

#### 1 二维码生成规则 ####
    签名生成方式:
    1 参数
      {
        "app_id": "app_343434"               # 汉光分配的app_id
        "app_secret": "sldkjsdkfjdd",        # 汉光分配的 secret
        "order_id": "12345",                 # rfid订单号
        "products": "(1232,1)-(3442,2)",     # 汉光商品ID
        "source": "lancome_rfid"             # 固定参数
        "timestamp": "784583",               # 时间戳
        "sign": "dde847KJke",                # 签名
        "nonce": "Djei341DKE"                # 一次性字符串
      }
      Note:
        products字段说明, 字符串：
          单个商品: (132438,1)             # 第一个数字是商品ID，第二个数字是商品数量
          多个商品: (132438,1)-(128899,2)  # 多个商品之间是用“-”隔开

    2 将上述参数按照key值进行排序后，按照key=value的格式拼接成一个字符串"app_id=app_1534851431&app_secret=cdke8945lk92dc5&nonce=23455&products=(1232,1)-(3442,2)&timestamp=1534853718&order_id=12345"
    3 对这个字符串取MD5，然后大写,可获得类似: A1D05398BE89B4906006364DE2725579 的签名

    二维码规则:
      参数(参数同上面生成签名的参数):
        {
          "app_id": "app_343434"            # 汉光分配的app_id
          "order_id": "12345",              # rfid订单号
          "products": "(1232,1)-(3442,2)",  # 汉光商品ID
          "source": "lancome_rfid"          # 固定参数
          "timestamp": "784583423",         # 时间戳
          "nonce": "Djei341DKE"             # 一次性字符串
          "sign": "dde847KJke",             # 签名
          "jump_type": "lancome_rfid"       # 固定参数(不参与签名, 该参数为前端小程序路由跳转使用)
        }
        二维码内容拼接(测试系统)
        https://sparrow.dongyouliang.com/wx-app/jumpBridge?
        jump_type=lancome_rfid&order_id=12345&products=(1232,1)-(3442,2)&app_id=app_1534851431&timestamp=1534853718&nonce=23455&sign=A1D05398BE89B4906006364DE2725579

        二维码内容拼接(正式系统)
        https://sparrow.hanguangbaihuo.com/wx-app/jumpBridge?
        jump_type=lancome_rfid&order_id=12345&products=(1232,1)-(3442,2)&app_id=app_1534851431&timestamp=1534853718&nonce=23455&sign=A1D05398BE89B4906006364DE2725579

        将这个url生成二维码供用户扫码使用
        Note:
            测试和正式的区别是: 前缀不同
            测试系统: https://sparrow.dongyouliang.com/wx-app/jumpBridge
            正式系统: https://sparrow.hanguangbaihuo.com/wx-app/jumpBridge

#### 2 支付成功后，汉光百货推送支付成功的消息 ####

    请求方式：POST
    参数：
      {
        "order_id": "12345",       # rfid订单号
        "price": "341.97",         # 支付金额
        "timestamp": "784583423",  # 时间戳
        "nonce": "Djei341DKE"      # 一次性字符串
        "sign": "LKelkoo34o9ukm"   # 签名
      }

      sign：签名算法同上
      参数:
      {
        "app_id": "app_343434",           # 汉光分配的app_id
        "app_secret": "sldkjsdkfjdd",     # 汉光分配的 secret
        "order_id": "12345",              # rfid订单号
        "hg_order_id": "190208999000001", # 汉光的订单号
        "price": "341.97",                # 支付金额, 汉光的实际支付金额
        "timestamp": "784583423",         # 时间戳
        "nonce": "Djei341DKE"             # 一次性字符串
      }

### 小程序前端使用接口(需要重写) ###

#### 1 获取购物车 ####

    待定

#### 2 获取优惠券 ####

    待定

#### 3 使用优惠券 ####

    待定

#### 4 下单 ####

    待定

#### 5 支付 ####

    请求方式:  POST 验证订单有效性, 支付
