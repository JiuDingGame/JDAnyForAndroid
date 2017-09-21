# 玖鼎聚合JDAnySDK Android客户端文档
[![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://raw.githubusercontent.com/JiuDingGame/JDAnySDKForAndroid/master/LICENSE.md)&nbsp;

使用
==============
1.将jdanysdk.jar包复制到游戏项目对应的目录下。

2.导入库文件、修改manifest<br/>
<img src="https://github.com/JiuDingGame/JDAnySDKForAndroid/raw/master/Snapshots/Androidmanifest.png"><br/>

注意	
==============
为防止横竖屏切换出现重载acvitity而导致短暂黑屏的问题，需要在主acvitity节点设置属性
```
android:configChanges="orientation|keyboardHidden|screenSize" 
```

如果是unity开发的游戏，需要在AndroidManifest.xml 的主activity入口处添加如下meta-data
```
<meta-data android:name="unityplayer.ForwardNativeEventsToDalvik" android:value="true" />
```


API具体应用说明
==============
1.初始化SDK（必须在onCreate里调用）

```java
public void onCreate(Bundle savedInstanceState) 
{
	JDAnySDK.getInstance().init(MainActivity.this);           
    JDAnySDK.getInstance().onCreate();
    if(JDAnySDK.getInstance().getOrientation().equals("landscape"))
    {
    	setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);
    }
}
```

2.登录（必接）
```java
JDAnySDKUser.getInstance().login();
```

3.切换账号
```java
if (!JDAnySDKUser.getInstance().isSupport("switchLogin")) 
{
	return;
}
JDAnySDKUser.getInstance().switchLogin();
```

4.注销
```java
if (!JDAnySDKUser.getInstance().isSupport("logout"))
{
	return;
}
JDAnySDKUser.getInstance().logout();
```

5.支付（必接）

`如果游戏需要校验支付结果，可以由游戏服务器生成唯一订单号，复制给extension参数，在调用支付回调地址时，JDAnySDK服务器会把extension参数原封不动的传回给游戏服务器`

参数说明
<table>
    <thead>
        <tr>
            <th>字段名称</th>
            <th>类型</th>
            <th>说明</th>
            <th>备注</th> 
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>productId</td>
            <td>String</td>
            <td>商品id</td>
            <td></td>
        </tr>
        <tr>
            <td>productName</td>
            <td>String</td>
            <td>商品名称</td>
            <td></td>
        </tr>
        <tr>
            <td>productDesc</td>
            <td>String</td>
            <td>商品描述</td>
            <td></td>
        </tr>
        <tr>
            <td>price</td>
            <td>int</td>
            <td>单个商品价格</td>
            <td>单位：分</td>
        </tr>
        <tr>
            <td>ratio</td>
            <td>int</td>
            <td>兑换比例</td>
            <td></td>
        </tr>
        <tr>
            <td>buyNum</td>
            <td>int</td>
            <td>购买数量</td>
            <td>默认传1即可</td>
        </tr>
        <tr>
            <td>serverId</td>
            <td>String</td>
            <td>服务器id</td>
            <td></td>
        </tr>
        <tr>
            <td>serverName</td>
            <td>String</td>
            <td>服务器名称</td>
            <td></td>
        </tr>
        <tr>
            <td>roleId</td>
            <td>String</td>
            <td>角色id</td>
            <td></td>
        </tr>
        <tr>
            <td>roleName</td>
            <td>String</td>
            <td>角色名称</td>
            <td></td>
        </tr>
        <tr>
            <td>roleLevel</td>
            <td>int</td>
            <td>角色等级</td>
            <td></td>
        </tr>
        <tr>
            <td>payNotifyUrl</td>
            <td>String</td>
            <td>支付回调通知地址</td>
            <td>可不填写。需告知我们配置</td>
        </tr>
        <tr>
            <td>vip</td>
            <td>String</td>
            <td>vip等级</td>
            <td></td>
        </tr>
        <tr>
            <td>extension</td>
            <td>String</td>
            <td>扩展参数</td>
            <td></td>
        </tr>
        <tr>
            <td>union</td>
            <td>String</td>
            <td>公会名称</td>
            <td></td>
        </tr>
    </tbody>
</table>

代码示例
```java
PayParams params = new PayParams();
params.setBuyNum(1);
params.setCoinNum(100);
params.setExtension(System.currentTimeMillis()+"");
params.setPrice(100);
params.setProductId("1");
params.setProductName("元宝");
params.setProductDesc("购买100元宝");
params.setRoleId("1");
params.setRoleLevel(1);
params.setRoleName("角色名");
params.setServerId("10");
params.setServerName("测试");
params.setVip("vip1");
JDAnySDKPay.getInstance().orderAndPay(params);
```

5.提交扩展数据（必接）

用户扩展数据，已经登录的角色相关数据，有的渠道需要统计角色相关数据
（PS：由于选择服务器时只能获取到服务器ID和服务器名称，故其他参数填默认值就好）
参数说明

DataType 参数类型

<table>
    <thead>
        <tr>
            <th>类型</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
		<tr>
            <td>TYPE_SELECT_SERVER = 1</td>
            <td>选择服务器</td>
        </tr>
        <tr>
            <td>TYPE_CREATE_ROLE = 2</td>
            <td>创建角色</td>
        </tr>
        <tr>
            <td>TYPE_ENTER_GAME = 3</td>
            <td>进入游戏</td>
        </tr>
        <tr>
            <td>TYPE_LEVEL_UP = 4</td>
            <td>等级提升</td>
        </tr>
    </tbody>
</table>

扩展参数 extraData 说明

<table>
    <thead>
        <tr>
        	<th>参数</th>
            <th>类型</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>dataType</td>
            <td>DataType枚举</td>
            <td>数据类型，当前处于阶段</td>
        </tr>
        <tr>
            <td>roleID</td>
            <td>int</td>
            <td>角色id</td>
        </tr>
        <tr>
            <td>roleName</td>
            <td>String</td>
            <td>角色名称</td>
        </tr>
        <tr>
            <td>roleLevel</td>
            <td>String</td>
            <td>角色等级</td>
        </tr>
        <tr>
            <td>serverID</td>
            <td>int</td>
            <td>服务器id</td>
        </tr>
        <tr>
            <td>serverName</td>
            <td>String</td>
            <td>服务器名称</td>
        </tr>
        <tr>
            <td>moneyNum</td>
            <td>int</td>
            <td>余额</td>
        </tr>
        <tr>
            <td>vipLv</td>
            <td>int</td>
            <td>vip等级</td>
        </tr>
         <tr>
            <td>unionName</td>
            <td>String</td>
            <td>公会名称</td>
        </tr>
    </tbody>
</table>

代码示例
```java
if(JDAnySDKUser.getInstance().isSupport("submitExtraData")) 
{
	UserExtraData params = new UserExtraData();
	params.setRoleID("12345");
	params.setDataType(1);
	params.setMoneyNum(1);
	params.setRoleLevel("11");
	params.setRoleName("角色名");
	params.setServerID(1);
	params.setServerName("服务器名称");
	params.setUnionName("公会名称");
	params.setVipLv(10);
	params.setCreateTime(123456789); //角色创建时间 	
	JDAnySDKUser.getInstance().submitExtraData(params); 
}
```

6.退出SDK（必接）

代码示例
```java
if (!JDAnySDKUser.getInstance().isSupport("exit")) 
{
	//某些sdk没有指定退出逻辑，可以在这里调用自己的退出逻辑
	return;
}
JDAnySDKUser.getInstance().exit();
```

7.添加生命周期方法（必接）

代码示例

```java
public void onActivityResult(int requestCode, int resultCode, Intent data){
	JDAnySDK.getInstance().onActivityResult(requestCode, resultCode, data);
	super.onActivityResult(requestCode, resultCode, data);
}
	
public void onStart(){
	JDAnySDK.getInstance().onStart();
	super.onStart();
}

public void onPause(){
	JDAnySDK.getInstance().onPause();
	super.onPause();
}
public void onResume(){
	JDAnySDK.getInstance().onResume();
	super.onResume();
}
public void onNewIntent(Intent newIntent){
	JDAnySDK.getInstance().onNewIntent(newIntent);
	super.onNewIntent(newIntent);
}
public void onStop(){
	JDAnySDK.getInstance().onStop();
	super.onStop();
}
public void onDestroy(){
	JDAnySDK.getInstance().onDestroy();
	super.onDestroy();
}
public void onRestart(){
	JDAnySDK.getInstance().onRestart();
	super.onRestart();
}

public void onConfigurationChanged(Configuration newConfig){
	super.onConfigurationChanged(newConfig);
}

public boolean dispatchKeyEvent(KeyEvent event) {
	JDAnySDK.getInstance().onBackPressed();
	return super.dispatchKeyEvent(event);
}

//如果是unity开发的游戏，需要把onBackPressed和onKeyDown注释掉
public void onBackPressed()
{
	JDAnySDK.getInstance().onBackPressed();
	super.onBackPressed();
	if(JDAnySDKUser.getInstance().isSupport("exit"))
	{
		JDAnySDKUser.getInstance().exit();
	}
	else
	{
		this.finish();
		System.exit(0);
	}
}

public boolean onKeyDown(int keyCode, KeyEvent event)
{
	if(keyCode == KeyEvent.KEYCODE_BACK){
		if(JDAnySDKUser.getInstance().isSupport("exit"))
		{
			JDAnySDKUser.getInstance().exit();
		}
		else
		{
			this.finish();
			System.exit(0);
		}
	}
	return true;
}
```

API消息通知说明
==============

1.设置SDK接口监听器（在SDK初始化前设置）

回调方法说明 
<table>
    <thead>
        <tr>
            <th>操作回调</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
	    <tr>
            <td>onResult(int code, String msg)</td>
            <td>SDK操作返回的状态信息。比如SDK初始化成功，
            SDK初始化失败，SDK登陆成功，登陆失败等信息</td>
        </tr>
        <tr>
            <td>onInitResult(InitResult result)</td>
            <td>初始化成功回调</td>
        </tr>
        <tr>
            <td>onAuthResult(UToken authResult)</td>
            <td>登录验证成功，如果是切换账号验证成功，
            authResult.isSwitch()为true</td>
        </tr>
        <tr>
            <td>onLogout()</td>
            <td>注销成功</td>
        </tr>
        <tr>
            <td>onPayResult(PayResult result)</td>
            <td>支付成功</td>
        </tr>
    </tbody>
</table>

onResult参数code说明
<table>
    <thead>
        <tr>
            <th>错误码</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CODE_NO_NETWORK = 0</td>
            <td>没有网络连接</td>
        </tr>
        <tr>
            <td>CODE_INIT_SUCCESS = 1</td>
            <td>初始化成功</td>
        </tr>
        <tr>
            <td>CODE_INIT_FAIL = 2</td>
            <td>注销成功</td>
        </tr>
        <tr>
            <td>CODE_UNINIT = 3</td>
            <td>没有初始化</td>
        </tr>
        <tr>
            <td>CODE_LOGIN_SUCCESS = 4</td>
            <td>登录成功</td>
        </tr>
        <tr>
            <td>CODE_LOGIN_FAIL = 5</td>
            <td>登录失败</td>
        </tr>
        <tr>
            <td>CODE_LOGIN_TIMEOUT = 6</td>
            <td>登录超时</td>
        </tr>
        <tr>
            <td>CODE_UNLOGIN = 7</td>
            <td>没有登录</td>
        </tr>
        <tr>
            <td>CODE_LOGOUT_SUCCESS = 8</td>
            <td>登出成功</td>
        </tr>
        <tr>
            <td>CODE_LOGOUT_FAIL = 9</td>
            <td>登出失败</td>
        </tr>
        <tr>
            <td>CODE_PAY_SUCCESS = 10</td>
            <td>支付成功</td>
        </tr>
        <tr>
            <td>CODE_PAY_FAIL = 11</td>
            <td>支付失败</td>
        </tr>
        <tr>
            <td>CODE_EXIT_SUCCESS = 33</td>
            <td>退出成功</td>
        </tr>
    </tbody>
</table>


onAuthResult 回调参数UToken 说明

<table>
    <thead>
        <tr>
            <th>接口</th>
            <th>返回值</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>isSuc()</td>
            <td>Boolean</td>
            <td>验证结果</td>
        </tr>
        <tr>
            <td>getUserID()</td>
            <td>Int</td>
            <td>JDAnySDK返回的唯一账号</td>
        </tr>
        <tr>
            <td>getSdkUserID()</td>
            <td>String</td>
            <td>渠道sdk返回的账号</td>
        </tr>
        <tr>
            <td>getUsername()</td>
            <td>String</td>
            <td>JDAnySDK返回的用户名</td>
        </tr>
        <tr>
            <td>getSdkUsername()</td>
            <td>String</td>
            <td>渠道SDK返回的用户名，可能为空</td>
        </tr>
        <tr>
            <td>getToken()</td>
            <td>String</td>
            <td>用于游戏服向JDAnySDK服务器做二次验证的令牌（参见JDAnySDK服务端接入文档）</td>
        </tr>
        <tr>
            <td>getExtension()</td>
            <td>String</td>
            <td>登录渠道SDK成功后返回给客户端的数据</td>
        </tr>
        <tr>
            <td>isSwitch()</td>
            <td>Boolean</td>
            <td>是否切换账号</td>
        </tr>
    </tbody>
</table>

代码示例
```java
JDAnySDK.getInstance().setSDKListener(new IJDAnySDKListener()
{
	@Override
	public void onResult(final int code, final String message) 
	{
		JDAnySDK.getInstance().runOnMainThread(new Runnable() 
		{	
			@Override
			public void run() 
			{
				switch(code)
				{
				case JDAnySDKCode.CODE_INIT_SUCCESS:
					Log.d("FDSDK", "初始化成功");
					break;
				case JDAnySDKCode.CODE_INIT_FAIL:
					Log.d("FDSDK", "初始化失败");
					break;
				case JDAnySDKCode.CODE_LOGIN_FAIL:
					Log.d("FDSDK", "登录失败");
					break;
				case JDAnySDKCode.CODE_LOGIN_SUCCESS:
					Log.d("FDSDK", "登录成功");
					break;
				case JDAnySDKCode.CODE_LOGOUT_SUCCESS:
					Log.d("FDSDK", "注销成功");
					break;
				case JDAnySDKCode.CODE_EXIT_SUCCESS:
    				Log.d("FDSDK", "退出成功");
					break;
				default:
				Log.d("JDAnySDK", "onResult:"+message);
						
				}
			}
		});
		
	}
	
	@Override
	public void onLoginResult(String result) 
	{
		Log.d("JDAnySDK", "The sdk login result is "+result);
		MainActivity.this.runOnUiThread(new Runnable() 
		{
			@Override
			public void run() 
			{	
				Log.d("JDAnySDK", "登录成功");
			}
		});
	}
	
	@Override
	public void onAuthResult(final UToken authResult) 
	{
		MainActivity.this.runOnUiThread(new Runnable()
		{	
			@Override
			public void run() {
				if(authResult.isSuc())
				{
					Log.d("JDAnySDK", "获取Token成功:" + authResult.getToken());
					//Todo 在此处处理登录登录成功业务
				}
				else
				{
					Log.d("JDAnySDK", "获取Token失败");
				}
			}
		});
	}    			

	@Override
	public void onSwitchAccount() {
		MainActivity.this.runOnUiThread(new Runnable() {
			@Override
			public void run()
			{
				Log.d("JDAnySDK", "切换帐号成功");
			}
		});
	}

	@Override
	public void onSwitchAccount(String result) {
		MainActivity.this.runOnUiThread(new Runnable() {
			@Override
			public void run()
			{
				Log.d("JDAnySDK", "切换帐号并登录成功" + result);
			}
		});
	}

	@Override
	public void onLogout() {
		MainActivity.this.runOnUiThread(new Runnable() {
			@Override
			public void run()
			{
				Log.d("JDAnySDK", "个人中心退出帐号成功");
			}
		});
	}

	@Override
	public void onPayResult(final PayResult result) {
		MainActivity.this.runOnUiThread(new Runnable() {
			@Override
			public void run()
			{
				Log.d("JDAnySDK", "支付成功,商品:"+result.getProductName());
			}
		});
	}

	@Override
	public void onInitResult(InitResult result) {
		
	}
});
```


许可证
==============
JDSDK 使用 MIT 许可证，详情见 LICENSE 文件。
