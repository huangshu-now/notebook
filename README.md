#1. 代码总览
![](https://github.com/huangshu-now/notebook/blob/main/3.png)
![](https://github.com/huangshu-now/notebook/blob/main/4.png)
#2.功能
##* 功能一 时间戳
预览：![](https://github.com/huangshu-now/notebook/blob/main/1.png)
基本功能代码：
从数据库获得每个笔记的时间
```
    private void getNoteList(){
        noteList.clear();
        Cursor allNotes = Note.getAllNotes(dbHelper);
        for (allNotes.moveToFirst(); !allNotes.isAfterLast(); allNotes.moveToNext()){
            NoteInfo noteInfo = new NoteInfo();
            noteInfo.setId(allNotes.getString(allNotes.getColumnIndex(Note._id)));
            noteInfo.setTitle(allNotes.getString(allNotes.getColumnIndex(Note.title)));
            noteInfo.setContent(allNotes.getString(allNotes.getColumnIndex(Note.content)));
            noteInfo.setDate(allNotes.getString(allNotes.getColumnIndex(Note.time)));
            noteList.add(noteInfo);
        }
  初始化视图
    }public ViewHolder(View itemView) {
        if (itemView == null){
            throw new IllegalArgumentException("item View can not be null!");
        }
        this.itemView = itemView;
        itemIcon = itemView.findViewById(R.id.rand_icon);
        itemNoteTitle = itemView.findViewById(R.id.item_note_title);
        itemNoteDate = itemView.findViewById(R.id.item_note_date);

    }
    添加布局
<TextView
            android:id="@+id/item_note_date"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:textSize="16sp"
            android:gravity="center_vertical"
            android:singleLine="true"/>
            ```
           
##* 功能二 搜索（根据标题）
预览：![](https://github.com/huangshu-now/notebook/blob/main/2.png)
添加布局
```<SearchView
        android:id="@+id/search2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        >
    </SearchView>
    ```
    修改notelist中布局文件
  setContentView(R.layout.searchview);
设置监听器
  private void SearchView(final SimpleCursorAdapter adapter) {
        SearchView searchView = findViewById(R.id.search2);
        searchView.setSubmitButtonEnabled(true);
        searchView.setOnQueryTextListener(new SearchView.OnQueryTextListener() {
            @Override
            public boolean onQueryTextSubmit(String query) {
                return false;
            }

            @Override
            public boolean onQueryTextChange(String s) {
                Cursor newCursor;

                if (!s.equals("")) {
                    String selection = NotePad.Notes.COLUMN_NAME_TITLE + " GLOB '*" + s + "*'";
                    newCursor = getContentResolver().query(
                            getIntent().getData(),
                            PROJECTION,
                            selection,
                            null,
                            NotePad.Notes.DEFAULT_SORT_ORDER
                    );
                } else {
                    newCursor = getContentResolver().query(
                            getIntent().getData(),
                            PROJECTION,
                            null,
                            null,
                            NotePad.Notes.DEFAULT_SORT_ORDER
                    );
                }
                adapter.swapCursor(newCursor); 
                return true;
            }
        });
    }



