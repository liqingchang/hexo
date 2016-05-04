---
title: Andoroid呼叫转移
tags:
  - android
  - 呼叫转移
id: 317
categories:
  - Android
date: 2012-02-02 11:09:41
---

呼叫转移是指机主手机无法接听电话的时候，把来电转移到目标号码的服务。

利用这个服务，还可以实现Android上的电话防火墙，具体是目标号码打来的话就转移到预设的防打扰提示音。

呼叫转移并不是手机提供的服务，所以只要服务商提供这个服务，就可以开通，无论你的是什么手机

以中国移动为例，假如手机上没有呼叫转移的设置，可以直接拨打相应号码开通
<div align="center">
<table border="1" cellspacing="0">
<tbody>
<tr>
<td align="left" valign="center" width="86">功 能</td>
<td align="left" valign="center" width="200">设 置</td>
<td align="left" valign="center" width="66">取 消</td>
<td align="left" valign="center" width="66">查 询</td>
</tr>
<tr>
<td align="left" valign="center" width="66">无条件转移</td>
<td align="left" valign="center" width="66">**21*电话号码#</td>
<td align="left" valign="center" width="66">##21#</td>
<td align="left" valign="center" width="66">*#21#</td>
</tr>
<tr>
<td align="left" valign="center" width="66">无信号转移</td>
<td align="left" valign="center" width="66">**62*电话号码#</td>
<td align="left" valign="center" width="66">##62#</td>
<td align="left" valign="center" width="66">*#62#</td>
</tr>
<tr>
<td align="left" valign="center" width="66">无应答转移</td>
<td align="left" valign="center" width="66">**61*电话号码*10*响铃时间#</td>
<td align="left" valign="center" width="66">##61#</td>
<td align="left" valign="center" width="66">*#61#</td>
</tr>
<tr>
<td align="left" valign="center" width="66">遇忙转移</td>
<td align="left" valign="center" width="66">**67*电话号码#</td>
<td align="left" valign="center" width="66">##67#</td>
<td align="left" valign="center" width="66">*#67#</td>
</tr>
</tbody>
</table>
</div>
比如我想在有人来电但手机没有信号的时候把来电转到13711111111这个号码，那么可以直接用手机拨打**62*1371111111#

利用这个原理，就可以方便的对手机呼叫转移进行设置

**1\. Android上的呼叫转移设置Demo：**

根据前面描述，可以直接利用intent拨打对应的号码来设置呼叫转移（源程序见篇末附件）：
<pre lang="java">    public class CallForwardingDemoActivity extends Activity implements OnClickListener{

        private final String WELL = "%23" ; //#号要用%23代替

	private String fordwardCondition = "21"; //呼叫转移条件：无条件-21 无信号-62 无应答-61 忙-67
	private String number;

	private RadioGroup rgType;
	private Button btnSet;
	private Button btnCancel;
	private Button btnQuery;
	private EditText forwardNum;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);
        init();
    }

    public void init(){
    	forwardNum = (EditText)findViewById(R.id.forward_number);
    	rgType = (RadioGroup)findViewById(R.id.rgtype);
    	rgType.setOnCheckedChangeListener(new OnCheckedChangeListener() {
			@Override
			public void onCheckedChanged(RadioGroup group, int checkedId) {
				switch(checkedId){
				case R.id.rbn_allforward:
					fordwardCondition = "21";
					break;
				case R.id.rbn_nosignalforward:
					fordwardCondition = "62";
					break;
				case R.id.rbn_noresponseforward:
					fordwardCondition = "61";
					break;
				case R.id.rbn_busyforward:
					fordwardCondition = "67";
					break;
				}
			}
		});
    	btnSet = (Button)findViewById(R.id.btnset);
    	btnSet.setOnClickListener(this);
    	btnCancel = (Button)findViewById(R.id.btncancel);
    	btnCancel.setOnClickListener(this);
    	btnQuery = (Button)findViewById(R.id.btnquery);
    	btnQuery.setOnClickListener(this);
    }

	@Override
	public void onClick(View view) {
		if(forwardNum.getText().toString().equals("")){
			Toast.makeText(this, R.string.empty, Toast.LENGTH_SHORT).show();
			return;
		}else{
			int id = view.getId();
			switch(id){
			case R.id.btnset:
				number = "**" + fordwardCondition + "*" + forwardNum.getText().toString() + WELL;
				break;
			case R.id.btncancel:
				number = WELL+WELL + fordwardCondition + WELL + forwardNum.getText().toString() + WELL;
				break;
			case R.id.btnquery:
				number = "*"+WELL + fordwardCondition + WELL + forwardNum.getText().toString() + WELL;
				break;
			}
			call();
		}
	}

	public void call(){
		Intent intent = new Intent();
		intent.setAction("android.intent.action.CALL");  
		Uri uri = Uri.parse("tel:" + number);  
		intent.setData(uri);  
		startActivity(intent);
	}
}</pre>
[![](http://tinone.net/wp-content/uploads/2012/02/1-180x300.png "1")](http://tinone.net/wp-content/uploads/2012/02/1.png)

效果如图所示。

**2\. 直接调用系统的呼叫转移设置页面：**
直接用代码实现呼叫转移虽然灵活，但是不是所有sim卡都支持这样的使用，测试发现会有部分手机出现如下情况：
[![](http://tinone.net/wp-content/uploads/2012/02/3-180x300.png "3")](http://tinone.net/wp-content/uploads/2012/02/3.png)
出现如下情况的手机有部分在系统设置中可以成功（还有部分系统设置也不行）
所以部分应用可能会需要用到跳转到系统的呼叫转移设置的功能
Api虽然没有呼叫转移对应的Uri
但查看代码后发现呼叫转移为com.android.phone包的GsmUmtsCallForwardOptions
所以可以直接跳转，代码如下：
<pre lang="java">   Intent intent = new Intent();
   intent.setClassName("com.android.phone", "com.android.phone.GsmUmtsCallForwardOptions");  
   startActivity(intent);</pre>
&nbsp;

附件：[CallForwardingDemo](http://tinone.net/wp-content/uploads/2012/02/CallForwardingDemo.rar)