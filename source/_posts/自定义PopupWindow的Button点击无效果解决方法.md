---
title: 自定义PopupWindow的Button点击无效果解决方法
id: 355
categories:
  - 未分类
date: 2012-03-07 14:58:42
tags:
---

<pre class="lang:java decode:true ">public class ExitWindow extends PopupWindow implements OnClickListener{
	private Button btn_grade;
	private Button btn_more;
	private Button btn_exit;
	private Button btn_cancel;
	private Handler mHandler;

	public ExitWindow(Context context,Handler handler){
		super((LinearLayout) ((LayoutInflater) context
				.getSystemService(Activity.LAYOUT_INFLATER_SERVICE)).inflate(
				R.layout.exitwindow, null), LayoutParams.WRAP_CONTENT,
				LayoutParams.WRAP_CONTENT);
		mHandler = handler;
		LayoutInflater inflater  = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
		LinearLayout linearLayout = (LinearLayout)inflater.inflate(R.layout.exitwindow, null);
		btn_grade = (Button)linearLayout.findViewById(R.id.btn_grade);
		btn_grade.setOnClickListener(this);
		btn_more = (Button)linearLayout.findViewById(R.id.btn_more);
		btn_more.setOnClickListener(this);
		btn_exit = (Button)linearLayout.findViewById(R.id.btn_exit);
		btn_exit.setOnClickListener(this);
		btn_cancel = (Button)linearLayout.findViewById(R.id.btn_cancel);
		btn_cancel.setOnClickListener(this);
		this.setFocusable(true);
		this.setContentView(linearLayout); //要加上这一行
	}

	@Override
	public void onClick(View v) {
		int id = v.getId();
		switch(id){
		case R.id.btn_grade:
			break;
		case R.id.btn_more:
			break;
		case R.id.btn_exit:
			mHandler.sendEmptyMessage(BookListActivity.EXIT);
			this.dismiss();
			break;
		case R.id.btn_cancel:
			this.dismiss();
			break;
		}

	}
}</pre>
&nbsp;