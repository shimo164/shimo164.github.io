labelタグをcheckboxにつけることで、文字をクリックしてもチェックすることができるようになる。


&lt;label&gt;&lt;input type="checkbox" name="fav" value="1"&gt;ラーメン&lt;/label&gt;





■JSでデフォルトでは消しておく



これは"p1"がのidを持った要素が先に記述されていないとエラーになる



document.getElementById("p1").style.display ="none";





■選択に応じて表示非表示を切り替えるJavaScript



https://www.nishishi.com/javascript-tips/radio-text-display.html



■Uncaught ReferenceError: none is not defined



"none" ではなく'none'を使ってみると良い？？



■ セレクトメニューの特定項目が選択された場合にテキストフィールドを有効にする

http://www.openspc2.org/reibun/javascript/form_selectmenu/023/



■datalist は選択したあとに変更できない。ブラウザによって対応が違うなどであまり使えなかった



https://www.tagindex.com/html5/form/datalist.html



■フォームで必須項目であるときはrequired を使う。ブラウザによってメッセージが違う。



&lt;input type="text" required&gt;

もしくは

&lt;input type="text" required="required"&gt;



■buttonのtypeはデフォルトでsubmit

submitしたくないときは、typeをbuttonとかにする

&lt;button  type="button" onclick="add_day();"&gt;

こんな感じにすると、onclickにしたときに遷移したくないときに関数だけ呼べる



http://www.htmq.com/html/button.shtml



■JSのインデントはスペース2個が多いらしい

https://ics.media/entry/10234



■NaN!==NaN



Javascriptでnanの判定をするときisNanを使うとややこしいようです。



a! == a がTrueなら a はNanという特性を利用します。

