[編集中]  
-----------------------------------
#Androidアプリのプログラムに挑戦  
テキストの保存と呼び出し  
-----------------------------------

今までは表示だけでしたが  
画面から文字入力して保存できるようにしてみましょう。  
ボタンを押すと別のページに遷移し保存した文字を  
出力してみます  

![20](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/20.png)

新規プロジェクトを作成します。  

![01](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/01.png)

最初の画面をAct_01という名前で作ります。  

![02](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/02.png)

１．XMLにDesignモードで[Text Fields]下の[Plain Text]と[Widgets]下の[Button]を  
ドラッグアンドドロップで追加します。  

![03](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/03.png)


TextモードでHelloWorldのTextView部分は削除します。  

![04](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/04.png)

EditTTextとButtonを下記のように編集します。  
android:hint=に書いた文字が、  
入力方法の説明として  
文字入力の部分に表示されます。  

    <EditText
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:lines="3"
        android:id="@+id/editText01"
        android:hint="@string/hint_01"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true" />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/button_01"
        android:id="@+id/button01"
        android:layout_below="@+id/editText01" />

![05](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/05.png)

「strings.xml」にhintに出力する文字とButtonに表示する文字を追加します  

    <string name="hint_01">好きな言葉を入力してください</string>  
    <string name="button_01">保存して次のページへ</string>  

![06](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/06.png)

プレビューで画面イメージを確認します。  

![07](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/07.png)

プログラムを書いていきます。  
Act_01.javaのメニューは使わないので削除します。  

![08](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/08.png)

ボタンクリックの処理を追加します。
ボタンクリック時に
"key_01"というIDで設定ファイルに保存します

    import android.content.Intent;
    import android.support.v7.app.ActionBarActivity;
    import android.os.Bundle;
    
    import android.content.SharedPreferences;
    import android.preference.PreferenceManager;
    import android.content.SharedPreferences.Editor;
    import android.view.View;
    import android.widget.Button;
    import android.widget.EditText;
    
    public class Act_01 extends ActionBarActivity {
    
        private SharedPreferences mPrefs;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_01);
    
            mPrefs = PreferenceManager.getDefaultSharedPreferences(this);
    
            Button btn01 = (Button) this.findViewById(R.id.button01);
            btn01.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    //テキストボックスに入力された値を取得
                    EditText txtContent = (EditText) findViewById(R.id.editText01);
                    String content = txtContent.getText().toString();
    
                    Editor editor = mPrefs.edit();
                    //key_01というIDで入力内容を設定ファイルに保存
                    editor.putString("key_01", content);
                    editor.apply();
                    //次のページへ
                    Intent intent = new Intent(getApplicationContext(),Act_02.class);
                    startActivity(intent);
    
                }
            });
    
        }
    
    }

![09](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/09.png)

遷移先の画面を追加します。  
[File]の[New]から[Actibity]-[Blank Actibity]を選択  


![10](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/10.png)

Activity_02.xmlに  
前画面で入力した文字を表示させる[TextView]を追加します。  

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text=""
        android:id="@+id/output_01"
        android:layout_alignParentLeft="true"
        android:layout_alignParentStart="true" />


![12](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/12.png)



    package com.example.androapp.appedit;
    
    import android.graphics.Color;
    import android.support.v7.app.ActionBarActivity;
    import android.os.Bundle;
    import android.content.SharedPreferences;
    import android.preference.PreferenceManager;
    import android.widget.TextView;
    
    public class Act_02 extends ActionBarActivity {
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_02);
    
            SharedPreferences mPrefs;
            mPrefs = PreferenceManager.getDefaultSharedPreferences(this);
            //key_01のIDで保存したテキストを取得
            String content = mPrefs.getString("key_01", "");
    
            TextView txtContent = (TextView) findViewById(R.id.output_01);
    
            txtContent.setText(content);
            txtContent.setTextColor(Color.rgb(255, 0, 0));
            txtContent.setTextSize(30.0f);
        }
    
    }

![13](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/13.png)

画面を追加すると  
\androidManifest.xmlに画面の定義が追加されます。  

    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.example.androapp.appedit" >
    
        <application
            android:allowBackup="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >
            <activity
                android:name=".Act_01"
                android:label="@string/app_name" >
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />
    
                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
            <activity
                android:name=".Act_02"
                android:label="@string/title_activity_act_02" >
            </activity>
        </application>
    
    </manifest>

![14](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/14.png)

![20](https://github.com/miyake-yasunaga/05_InOut/blob/master/images/20.png)



