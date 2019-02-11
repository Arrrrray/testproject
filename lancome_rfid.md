
### 流程描述 ###

    1. 用户选择使用 RFID 购买
    2. 导购将含有 RFID 的商品放到读卡区域
    3. 读取 rfid 信息并切在屏幕上生成二维码（该二维码是按照汉光规则生成）
    3. 用户使用微信扫描二维码进入小程序进行下单,购买
    4. 购买成功后，汉光推送支付成功的消息


### 接口描述 ###
共需要一个接口和一个二维码的生成规则：如下

#### 1 二维码生成规则 ####

    参数：
      {
        "app_id": "app_343434"        # 汉光分配的app_id
        "order_id": "12345",          # rfid订单号
        "product_ids": "123,124,125", # 自动售货机商品ID(多个商品用逗号隔开)
        "source": "lancome_rfid"      # 固定参数
        "timestamp": "784583",        # 时间戳
        "sign": "dde847KJke",         # 签名
        "nonce": "Djei341DKE"         # 一次性字符串
      }

      sign方法：
        签名参数:
        {
          "app_id": "app_343434"        # 汉光分配的app_id
          "app_secret": "sldkjsdkfjdd", # 汉光分配的 secret
          "order_id": "12345",          # rfid订单号
          "product_ids": "123,124,125", # 自动售货机商品ID(多个商品用逗号隔开)
          "source": "lancome_rfid"      # 固定参数
          "timestamp": "784583423",     # 时间戳
          "nonce": "Djei341DKE"         # 一次性字符串
        }
        类似微信的签名,
        1. 将所有参数按照 key 从小到大排序（字典序），使用键值对的格式（即key1=value1&key2=value2…）拼接成字符串
        2. 对生成的字符串进行 MD5, 然后大写获得 sign


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
