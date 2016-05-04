---
title: 基于SD卡的文件选择器
tags:
  - android
  - 文件选择器
id: 275
categories:
  - Android
  - Code
date: 2011-11-22 15:46:48
---

很久没有更新了，前一段时间很忙

项目需要用到一个文件选择器，网上很多资料，参考了其中一篇做了一个，具体地址忘记了，很不好意思

作者可以跟我联系
<pre name="code" class="java">public class ActivityFilePicker extends Activity {

	private ListView listViewFilePicker;
	private File mCurrentDirectory = new File(Environment.getExternalStorageDirectory().getAbsolutePath()); //根目录位置
	AdapterFilePicker mFileAdapter; //ListView适配器
	String fileEndings[] = { "mp3" }; 	//显示的文件类型

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.layout_activityfilepicker);
		listViewFilePicker = (ListView) findViewById(R.id.listview);
		listViewFilePicker.setCacheColorHint(0x00000000);
		mFileAdapter = new AdapterFilePicker(this);
		listViewFilePicker.setAdapter(mFileAdapter);
		//Item点击事件
		ListView.OnItemClickListener lv2click = new ListView.OnItemClickListener() {
			public void onItemClick(AdapterView parent, View view,
					int position, long id) {
				int fid = mFileAdapter.getItemType((int) id);
				String mPath = "";
				if (fid == 1) { //文件类型1为文件夹，点击进入该文件夹目录
					String s1 = mFileAdapter.getItem((int) id).name;
					if (s1.equals("..")) {
						mPath = mCurrentDirectory.getParent();
					} else {
						mPath = mCurrentDirectory.getPath() + "/" + s1 + "/";
					}
					mCurrentDirectory = new File(mPath);
					ListFile(mCurrentDirectory);
				} else { //返回选中的文件的Path
					Bundle bundle = new Bundle();
					bundle.putString("path", mCurrentDirectory.getPath()
							+ "/" + mFileAdapter.getItem((int) id).name);
					Intent mIntent = new Intent();
					mIntent.putExtras(bundle);
					setResult(RESULT_OK, mIntent);
					ActivityFilePicker.this.finish();
				}
			}
		};
		ListFile(mCurrentDirectory);
		listViewFilePicker.setOnItemClickListener(lv2click);
	}

	/**
	 * 列出该目录的文件
	 * @param aDirectory
	 */
	private void ListFile(File aDirectory) {
		mFileAdapter.clearItems();
		mFileAdapter.notifyDataSetChanged();
		listViewFilePicker.postInvalidate();
		if (!aDirectory.getPath().equals("/sdcard")) {
			FileData fd = new FileData();
			fd.name = "..";
			fd.type = 1;
			mFileAdapter.addItem(fd);
		}
		for (File f : aDirectory.listFiles()) {
			if (f.isDirectory()) {
				FileData fd = new FileData();
				fd.name = f.getName();
				fd.type = 1;
				mFileAdapter.addItem(fd);
			} else {
				if (checkEnds(f.getName().toLowerCase())) {
					FileData fd = new FileData();
					fd.name = f.getName();
					fd.type = 0;
					mFileAdapter.addItem(fd);
				}
			}
		}
		mFileAdapter.notifyDataSetChanged();
		listViewFilePicker.postInvalidate();
	}

	private boolean checkEnds(String checkItsEnd) {
		for (String aEnd : fileEndings) {
			if (checkItsEnd.endsWith(aEnd))
				return true;
		}
		return false;
	}
}</pre>
由于模块结构很简单，所以把遍历文件的方法放在Activity上了
<pre name="code" class="java">public class AdapterFilePicker extends BaseAdapter {
	private Context mContext;
	private Vector mItems = new Vector();
	private LinearLayout layout, layout_more;
	private LayoutInflater inflate;
	private TextView textViewItem;

	public AdapterFilePicker(Context context) {
		mContext = context;
		inflate = (LayoutInflater) mContext
				.getSystemService(android.content.Context.LAYOUT_INFLATER_SERVICE);
	}

	public void addItem(FileData item) {
		mItems.add(item);
	}

	public FileData getItem(int it) {
		return (FileData) mItems.elementAt(it);
	}

	public int getCount() {
		return mItems.size();
	}

	public long getItemId(int arg0) {
		return arg0;
	}

	public int getItemType(int arg0) {
		return getItem(arg0).type;
	}

	public void clearItems() {
		mItems.clear();
	}

	public View getView(int position, View view, ViewGroup arg2) {
		if(null == view){
			view = (LinearLayout) inflate.inflate(R.layout.item_filepicker_listview, null);
		}
		textViewItem = (TextView) view.findViewById(R.id.textViewFileName);
		textViewItem.setText(getItem(position).name);
		return view;
	}

}</pre>
由于只是一个Demo模块，UI并没有太过讲究，可以根据要求修改adapter来实现UI
<pre name="code" class="java">
/**
 * 文件结构
 * @author Administrator
 *
 */
public class FileData {
		public String name; //文件名
		public int type; //文件类型
}</pre>

效果：
<div align="center">![](http://tinone.net/wp-content/uploads/2011/11/1-180x300.png "1")                         ![](http://tinone.net/wp-content/uploads/2011/11/2-180x300.png "2")</div>