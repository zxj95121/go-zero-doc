#SDK接入文档

版本 | 日期 | 说明
--------- | ------------- | -------------
1.0 | 2017.8.1 | 初始化
1.1 | 2017.8.30 | 新增方法</br>&nbsp;●&nbsp;hasPlatformUserCenter</br>&nbsp;●&nbsp;isLogined
1.2 | 2017.9.20 | 新增分享
1.3 | 2018.8.20 | 增加使用⽅法名调用方法支持(可选)
1.4 | 2019.6.19 | <font color=#FF7F24 size=3>增加海外发行配置说明</font>

[TOC]

# 快速接入流程
## 配置 AndroidManifest.xml (必选)
targetSdkVersion设为28（AndroidStudio在app/build.gradle中配置）

添加框架需要的Permission和Application

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    
配置 android:name="com.s.plugin.platform.SPlatformApplication"
> 如果应用已有Application，请将已有Application继承至com.s.plugin.platform.SPlatformApplication

在 Application 增加 SWebActivity 配置

        <activity
            android:name="com.s.core.activity.SWebActivity" >
        </activity>

<font color=#FF7F24 size=3>配置 appkey, 参数由我方提供（国内发行不用配置）</font>

        <meta-data
            android:name="appkey"
            android:value="30d104c5bdf07ab117eee7a0c4bc262d" />


        
## 初始化SDK (必选)
> import com.m7758bt.framework.M7758btSDK;

描述
> <font color=#ff0000 size=3>* 在主 Activity 的 onCreate 方法中调用SDK初始化</font>

接口
>
```
M7758btSDK.getInstance().init(this, appInfo);
```

参数
>
参数1：Activity
参数2：AppInfo

AppInfo（海外发行marketingArea和language为必传参数）
>
key | 说明
--------- | -------------
name | 应用名
shortName | 应用名简写
direction | 应用屏幕方向（0:横屏 1:竖屏）
<font color=#FF7F24 size=3>marketingArea</font> | 发行区域（0:国内 1:台湾 2:韩国）
<font color=#FF7F24 size=3>language</font> | 语言（0:简体中文 1:繁体中文 4:韩文）

> <font color=#ff0000 size=3>* 以上参数由SDK方提供</font>

回调说明
> 参考 平台系统->初始化

样例
>
		Map<String, String> appInfo = new HashMap<String, String>();
		appInfo.put("name", "Demo");
		appInfo.put("shortName", "demo");
		appInfo.put("direction", "1");
		//appInfo.put("marketingArea", "1");
		//appInfo.put("language", "1");
>	
		M7758btSDK.getInstance().setDebug(false);
		M7758btSDK.getInstance().init(this, appInfo);

正式包请设置
> <font color=#ff0000 size=3>* debug为false</font>

## 重写主 Activity 生命周期相关方法 (必选)

> import com.m7758bt.framework.SPluginWrapper;

	@Override
	protected void onStart()
	{
		super.onStart();
		SPluginWrapper.onStart();
	}
	
	@Override
	protected void onPause()
	{
		super.onPause();
		SPluginWrapper.onPause();
	}
	
	@Override
	protected void onResume()
	{
		super.onResume();
		SPluginWrapper.onResume();
	}
	
	@Override
	protected void onStop()
	{
		super.onStop();
		SPluginWrapper.onStop();
	}
	
	@Override
	protected void onRestart()
	{
		super.onRestart();
		SPluginWrapper.onRestart();
	}
	
	@Override
	protected void onDestroy()
	{
		super.onDestroy();
		SPluginWrapper.onDestroy();
	}
	
	@Override
	protected void onNewIntent(Intent intent)
	{
		super.onNewIntent(intent);
		SPluginWrapper.onNewIntent(intent);
	}
	
	@Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data)
	{
		super.onActivityResult(requestCode, resultCode, data);
		SPluginWrapper.onActivityResult(requestCode, resultCode, data);
	}
	
	@Override
	public void onConfigurationChanged(Configuration newConfig)
	{
		super.onConfigurationChanged(newConfig);
		SPluginWrapper.onConfigurationChanged(newConfig);
	}
	
	@Override
	public void onWindowFocusChanged(boolean hasFocus)
	{
		super.onWindowFocusChanged(hasFocus);
		SPluginWrapper.onWindowFocusChanged(hasFocus);
	}
	
	@Override
	protected void onSaveInstanceState(Bundle outState)
	{
		super.onSaveInstanceState(outState);
		SPluginWrapper.onSaveInstanceState(outState);
	}
	
	@Override
	public void onBackPressed()
	{
		super.onBackPressed();
		SPluginWrapper.onBackPressed();
	}

## 释放资源 (必选)
描述
> 
调用 SDK 的 release 方法进行资源释放，在游戏结束进程之前调用


接口
> 
```
M7758btSDK.getInstance().release();
```

参数
> 无

回调说明
> 无

## 使⽤⽅法名调用方法 (可选)
描述
> 
调用isFunctionSupported判断系统是否支持该方法调用，调用callFunction调用对应的方法


例子
> 
```
if (M7758btSDKPlatform.getInstance().isFunctionSupported("test")) 
{
	M7758btSDKPlatform.getInstance().callFunction("test");
}
```

参数
> 无

回调说明
> 无

# 平台系统 (必选)

## 设置平台监听 (必选)
>
import com.m7758bt.framework.M7758btSDKListener;
import com.m7758bt.framework.platform.M7758btSDKPlatform;

    M7758btSDKPlatform.getInstance().setListener(new M7758btSDKListener()
    {
    	@Override
    	public void onCallBack(int action, Map<String, String> result)
    	{
    		// TODO Auto-generated method stub
    		String msg = null;
    		switch (action)
    		{
    			case M7758btSDKPlatform.ACTION_INIT_SUCCESS:
    				msg = "初始化成功";
    				break;
    			case M7758btSDKPlatform.ACTION_INIT_FAILURE:
    				msg = "初始化失败";
    				break;
    			case M7758btSDKPlatform.ACTION_LOGIN_SUCCESS:
    				msg = "登录成功";
    				break;
    			case M7758btSDKPlatform.ACTION_LOGIN_FAILURE:
    				msg = "登录失败";
    				break;
    			case M7758btSDKPlatform.ACTION_ACCOUNTSWITCH_LOGOUT_SUCCESS:
    				msg = "帐号切换-注销成功";
    				break;
    			case M7758btSDKPlatform.ACTION_ACCOUNTSWITCH_FAILURE:
    				msg = "帐号切换失败";
    				break;
    			case M7758btSDKPlatform.ACTION_PAY_SUCCESS:
    				msg = "支付成功";
    				break;
    			case M7758btSDKPlatform.ACTION_PAY_FAILURE:
    				msg = "支付失败";
    				break;
    			case M7758btSDKPlatform.ACTION_EXIT_FROM_PLATFORM:
    				msg = "第三方平台退出";
    				break;
    			case M7758btSDKPlatform.ACTION_EXIT_FROM_GAME:
    				msg = "游戏退出";
    				break;
    			case M7758btSDKPlatform.ACTION_ANTI_ADDICTION_QUERY_SUCCESS:
    				msg = "实名制查询成功";
    				break;
    			case M7758btSDKPlatform.ACTION_ANTI_ADDICTION_QUERY_FAILURE:
    				msg = "实名制查询失败";
    				break;
    			default:
    				break;
    		}
    		
    		Log.d(TAG, "msg:" + msg + "\t result:" + (null != result ? result.toString() : null));
    	}
    });

## 相关接口说明

失败回调说明
> 
key | 说明
--------- | -------------
code | 错误码
message | 错误消息

### 初始化 (必选)
描述
> 在初始化SDK的时候会对平台SDK进行初始化

接口
> 参考 接入流程->第二点

参数
> 无

回调说明
> 
说明 | action | result
--------- | ------------- | -------------
初始化成功 | ACTION_INIT_SUCCESS | null
初始化失败 | ACTION_INIT_FAILURE | 参考《失败回调说明》

### 登录 (必选)
描述
> 调用平台SDK进行登录

接口
> 
```
M7758btSDKPlatform.getInstance().doLogin();
```

参数
> 无

回调说明
> 
说明 | action | result
--------- | ------------- | -------------
登录成功 | ACTION_LOGIN_SUCCESS | ```Map<String, String>```
登录失败 | ACTION_LOGIN_FAILURE | 参考《失败回调说明》

ACTION_LOGIN_SUCCESS 说明
> 
key | 说明
--------- | -------------
userId | 用户Id
token | Token
platformId | 平台Id

### 设置角色信息 (必选)
描述
> 调用平台SDK相关统计，登录成功后调用，切换账号登录成功后需重新调用，请勿多次调用

接口
> 
```
M7758btSDKPlatform.getInstance().setRoleInfo(roleInfo);
```

参数
> 
key | 说明
--------- | -------------
roleId | 角色Id
roleName | 角色名
zoneId | 区服Id
zoneName | 区服名称
partyName | 公会、帮派等
roleLevel | 角色等级
roleVipLevel | 角色VIP等级
balance | 玩家游戏内虚拟币余额
isNewRole | 是否新角色（0:已有角色 1:新角色）

回调说明
> 无

样例
>	  Map<String, String> roleInfo = new HashMap<String, String>();
	roleInfo.put("roleId", "10001");
	roleInfo.put("roleName", "我是角色名");
	roleInfo.put("zoneId", "100");
	roleInfo.put("zoneName", "测试服");
	roleInfo.put("partyName", "公会名称");
	roleInfo.put("roleLevel", "10");
	roleInfo.put("roleVipLevel", "0");
	roleInfo.put("balance", "0");
	roleInfo.put("isNewRole", "0");
	M7758btSDKPlatform.getInstance().setRoleInfo(roleInfo);

### 切换帐号 (必选)
描述
> 调用平台SDK进行帐号切换

接口
> 
```
M7758btSDKPlatform.getInstance().doAccountSwitch();
```

参数
> 无

回调说明
> 
说明 | action | result
--------- | ------------- | -------------
切换帐号-注销成功 | ACTION_ACCOUNTSWITCH_LOGOUT_SUCCESS | null
切换帐号失败 | ACTION_ACCOUNTSWITCH_FAILURE | 参考《失败回调说明》

ACTION_ACCOUNTSWITCH_LOGOUT_SUCCESS 说明
> 平台SDK注销成功，游戏需返回登录前的界面，平台用户重新登录成功后回调ACTION_LOGIN_SUCCESS

### 支付 (必选)
描述
> 调用平台SDK进行支付

接口
> 
```
M7758btSDKPlatform.getInstance().doPay(payInfo);
```

参数
> 
key | 说明
--------- | -------------
productId | 产品Id（每个产品Id需唯一）
productName | 产品名称
productDesc | 产品描述
productPrice | 产品单价（元）
productCount | 产品数量（正常传1即可）
productType | 产品类型（0:普通 1:月卡 2:周卡 3:季卡 4:年卡 5:终身卡）
coinName | 虚拟币币名称（如金币、钻石等）
coinRate | 虚拟币兑换比例（例如10，表示1元人民币购买10虚拟币）
extendInfo | 扩展信息
roleId | 角色Id
roleName | 角色名
zoneId | 区服Id
zoneName | 区服名称
partyName | 公会、帮派等
roleLevel | 角色等级
roleVipLevel | 角色VIP等级
balance | 玩家游戏内虚拟币余额

回调说明
>
说明 | action | result
--------- | ------------- | -------------
支付成功 | ACTION_PAY_SUCCESS | null
支付失败 | ACTION_PAY_FAILURE | 参考《失败回调说明》

样例
>
    Map<String, String> payInfo = new HashMap<String, String>();
	payInfo.put("productId", "10001");
	payInfo.put("productName", "60元宝");
	payInfo.put("productDesc", "购买60元宝");
	payInfo.put("productPrice", "6");
	payInfo.put("productCount", "1");
	payInfo.put("productType", "0");
	payInfo.put("coinName", "元宝");
	payInfo.put("coinRate", "10");
	payInfo.put("extendInfo", "扩展透传信息");
    payInfo.put("roleId", "10001");
    payInfo.put("roleName", "我是角色名");
    payInfo.put("zoneId", "100");
    payInfo.put("zoneName", "测试服");
    payInfo.put("partyName", "公会名称");
    payInfo.put("roleLevel", "10");
    payInfo.put("roleVipLevel", "0");
    payInfo.put("balance", "0");
    M7758btSDKPlatform.getInstance().doPay(payInfo);

### 退出 (必选)
描述
> 调用平台SDK执行退出相关逻辑

接口
> 
```
M7758btSDKPlatform.getInstance().doExit();
```

参数
> 无

回调说明
> 
说明 | action | result
--------- | ------------- | -------------
平台退出 | ACTION_EXIT_FROM_PLATFORM | null
游戏退出 | ACTION_EXIT_FROM_GAME | null

ACTION_EXIT_FROM_PLATFORM
> 执行退出游戏操作，不展示游戏确认退出对话框

ACTION_EXIT_FROM_GAME
> 展示游戏确认退出对话框及相关逻辑

### 角色等级升级 (必选)
描述
> 调用SDK相关数据统计分析

接口
> 
```
M7758btSDKPlatform.getInstance().onRoleLevelUpgrade(roleLevel);
```

参数
> 角色新等级（int）

回调说明
> 无

### 角色名更新 (可选)
描述
> 调用SDK相关数据统计分析

接口
> 
```
M7758btSDKPlatform.getInstance().onRoleNameUpdate(newRoleName);
```

参数
> 更新后新角色名（String）

回调说明
> 无

### 防沉迷查询 (必选)
描述
> 调用平台SDK进行防沉迷查询，需要在登录成功后查询

接口
> 
```
M7758btSDKPlatform.getInstance().doAntiAddictionQuery();
```

参数
> 无

回调说明
> 
说明 | action | result
--------- | ------------- | -------------
查询成功 | ACTION_ANTI_ADDICTION_QUERY_SUCCESS | ```Map<String, String>```
查询失败 | ACTION_ANTI_ADDICTION_QUERY_FAILURE | null

ACTION_ANTI_ADDICTION_QUERY_SUCCESS
> 
key | 说明
--------- | -------------
result | 查询结果（0:无此用户数据，1:未成年，2:已成年）

### 进入平台中心 (可选)
描述
> 
调用平台SDK相关方法展示平台中心首页。需要在游戏内加一个按钮，点击按钮后响应该方法，根据调用hasPlatformUserCenter()的返回值判断该按钮显示(true)或隐藏(false)。该方法是针对极少数平台没有悬浮按钮且需要展示平台中心的情况。

接口
> 
```
判断显示(true)或隐藏(false)调用
M7758btSDKPlatform.getInstance().hasPlatformUserCenter();
```
```
点击按钮后调用
M7758btSDKPlatform.getInstance().doEnterPlatform();
```

参数
> 无

回调说明
> 无


### 获取登录状态 (可选)
描述
> 调用SDK方法获取登录状态
true  : 已登录
false : 未登录

接口
> 
```
M7758btSDKPlatform.getInstance().isLogined();
```

参数
> 无

回调说明
> 无

### 获取登录用户信息 (可选)
描述
> 调用SDK方法获取登录用户信息(用户未登录的情况下返回null)，返回值参考登录成功

接口
> 
```
M7758btSDKPlatform.getInstance().getUser();
```

参数
> 无

回调说明
> 无

# 分享系统 (可选)
### 设置监听 (必选)

> import com.m7758bt.framework.share.M7758btSDKShare;
	
	M7758btSDKShare.getInstance().setListener(new M7758btSDKListener()
	{
		@Override
		public void onCallBack(int code, Map<String, String> result)
		{
			// TODO Auto-generated method stub
			switch (code)
			{
				case M7758btSDKShare.SHARE_SUCCESS:
					//	分享成功
					int socialId = Integer.parseInt(result.get("socialId"));
					if (M7758btSDKShare.SOCIAL_WECHAT == socialId)
					{
						Log.d(TAG, result.toString());
					}
					break;
				case M7758btSDKShare.SHARE_FAILURE:
					//	分享失败
					break;
				default:
					break;
			}
		}
	});

### 分享 (必选)
描述
> 调用社交平台分享

接口
> 
```
M7758btSDKShare.getInstance().doShare(contents, to);
```

参数
> 
contents

key | 说明
--------- | -------------
SHARE_TITLE | 标题(String)
SHARE_TEXT | 文本(String)
SHARE_DESCRIPTION | 描述(String)
SHARE_IMAGE_DATA | 图片(Bitmap)
SHARE_IMAGE_LOCAL_PATH | 图片路径(String)
SHARE_THUMB_IMAGE_DATA | 缩略图(Bitmap)
SHARE_THUMB_IMAGE_LOCAL_PATH | 缩略图路径(String)
SHARE_LINK_URL | 链接(String)
SHARE_EXTEND | 透传(String)

>
to

to | 说明
--------- | -------------
SHARE_TO_WECHAT_SESSION | 微信聊天界面
SHARE_TO_WECHAT_TIMELINE | 微信朋友圈
SHARE_TO_QQ | 手机QQ
SHARE_TO_QZONE | QQ空间
SHARE_TO_SINA | 新浪微博
SHARE_TO_FACEBOOK | FaceBook

回调说明
> 
说明 | action | result
--------- | ------------- | -------------
分享成功 | SHARE_SUCCESS | ```Map<String, String>```
分享失败 | SHARE_FAILURE | 参考《失败回调说明》

socialId
> 
id | 说明
--------- | -------------
SOCIAL_WECHAT | 微信
SOCIAL_QQ | QQ
SOCIAL_SINA | 新浪微博
SOCIAL_FACEBOOK | FaceBook


