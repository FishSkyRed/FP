[NoteList中显示条目增加时间戳显示.md](https://github.com/FishSkyRed/FP/files/6530446/NoteList.md)
# NotePad
This is an AndroidStudio rebuild of google SDK sample NotePad
# NoteList中显示条目增加时间戳显示

要增加时间戳，首先要获取当前时间，北京时间要用GMT时间加八小时得到（这里是创建Note时获取的时间）

	Date date = new Date(now);
	SimpleDateFormat format = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
	format.setTimeZone(TimeZone.getTimeZone("GMT+08:00"));
	String formatDate = format.format(date);

时间戳是显示在每条Note的下面，所以在noteslist_item.xml添加一个用于显示时间戳的TextView

	<TextView
	    android:id="@+id/text1_date"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:textSize="17dp"
	    android:paddingLeft="5dip"
	/>
	接着在它的java文件NoteList.java中关于显示Note的函数里加上时间的显示
	private static final String[] PROJECTION = new String[] {
	        NotePad.Notes._ID, // 0
	        NotePad.Notes.COLUMN_NAME_TITLE, // 1
	        NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE //日期
	};
	private static final int COLUMN_INDEX_TITLE = 1;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    setDefaultKeyMode(DEFAULT_KEYS_SHORTCUT);
	    Intent intent = getIntent();
	    if (intent.getData() == null) {
	        intent.setData(NotePad.Notes.CONTENT_URI);
	    }
	    getListView().setOnCreateContextMenuListener(this);
	    Cursor cursor = managedQuery(
	        getIntent().getData(),            // Use the default content URI for the provider.
	        PROJECTION,                       // Return the note ID and title for each note.
	        null,                             // No where clause, return all records.
	        null,                             // No where clause, therefore no where column values.
	        NotePad.Notes.DEFAULT_SORT_ORDER  // Use the default sort order.
	    );
	    String[] dataColumns = {
	            NotePad.Notes.COLUMN_NAME_TITLE,
	            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE
	    } ;
	    int[] viewIDs = {
	            android.R.id.text1,
	            R.id.text1_date
	    };
	    SimpleCursorAdapter adapter
	        = new SimpleCursorAdapter(
	                  this,                             // The Context for the ListView
	                  R.layout.noteslist_item,          // Points to the XML for a list item
	                  cursor,                           // The cursor to get items from
	                  dataColumns,
	                  viewIDs
	          );
	    setListAdapter(adapter);
	}
	注意，还要在NoteEditor.java里也添加获取时间的功能（这里是编辑Note时获取的时间），不然无法正确显示编辑Note后的时间
运行效果截图：
![snipaste20210524_201240](https://user-images.githubusercontent.com/84312532/119346480-d7837900-bccc-11eb-83d5-ff4c36af6ce9.png)
![snipaste20210524_201154](https://user-images.githubusercontent.com/84312532/119346509-e10ce100-bccc-11eb-8a69-ca04f38a9fe3.png)
