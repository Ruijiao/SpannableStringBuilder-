# SpannableStringBuilder-
### 使只能显示单一形式的TextView和EditText更炫酷的显示
## 一。用法详解之字符：
<pre>
<code>
  SpannableString ss = new SpannableString("红色打电话斜体删除线绿色下划线图片:."); 
  //用颜色标记文本
  ss.setSpan(new ForegroundColorSpan(Color.RED), 0, 2,  
  //setSpan时需要指定的 flag,Spanned.SPAN_EXCLUSIVE_EXCLUSIVE(前后都不包括).
  Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
  //用超链接标记文本
  ss.setSpan(new URLSpan("tel:4155551212"), 2, 5,  
  Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
  //用样式标记文本（斜体）
  ss.setSpan(new StyleSpan(Typeface.BOLD_ITALIC), 5, 7,  
  Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
  //用删除线标记文本
  ss.setSpan(new StrikethroughSpan(), 7, 10,  
         Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
  //用下划线标记文本
  ss.setSpan(new UnderlineSpan(), 10, 16,  
         Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
  //用颜色标记
  ss.setSpan(new ForegroundColorSpan(Color.GREEN), 10, 13,  
         Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
  //获取Drawable资源
  Drawable d = getResources().getDrawable(R.drawable.icon);  
  d.setBounds(0, 0, d.getIntrinsicWidth(), d.getIntrinsicHeight());
  //创建ImageSpan
  ImageSpan span = new ImageSpan(d, ImageSpan.ALIGN_BASELINE);
  //用ImageSpan替换文本
  ss.setSpan(span, 18, 19, Spannable.SPAN_INCLUSIVE_EXCLUSIVE);  
  txtInfo.setText(ss);
  txtInfo.setMovementMethod(LinkMovementMethod.getInstance()); //实现文本的滚动  
</code>
</pre>
#### //将需要的文字高亮显示： 
<pre>
<code>
  public void highlight(int start,int end){  
      SpannableStringBuilder spannable=new SpannableStringBuilder(getText().toString());//用于可变字符串  
      ForegroundColorSpan span=new ForegroundColorSpan(Color.RED);  
      spannable.setSpan(span, start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
      setText(spannable);  
  }  
</code>
</pre>
#### //加下划线： 
<pre>
<code>
  public void underline(int start,int end){  
      SpannableStringBuilder spannable=new SpannableStringBuilder(getText().toString());  
      CharacterStyle span=new UnderlineSpan();  
      spannable.setSpan(span, start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
      setText(spannable);  
  }  
</code>
</pre>
## 二。用法详解之图片
### 通常用于显示文字，但有时候也需要在文字中夹杂一些图片，比如QQ中就可以使用表情图片，又比如需要的文字高亮显示等等，如何在android中也做到这样呢？ 
记得android中有个android.text包，这里提供了对文本的强大的处理功能。 
添加图片主要用SpannableString和ImageSpan类：
<pre>
<code>
  Drawable drawable = getResources().getDrawable(id);  
  drawable.setBounds(0, 0, drawable.getIntrinsicWidth(), drawable.getIntrinsicHeight());  
  //需要处理的文本，[smile]是需要被替代的文本  
  SpannableString spannable = new SpannableString(getText().toString()+"[smile]");  
  //要让图片替代指定的文字就要用ImageSpan  
  ImageSpan span = new ImageSpan(drawable, ImageSpan.ALIGN_BASELINE);  
  //开始替换，注意第2和第3个参数表示从哪里开始替换到哪里替换结束（start和end）  
  //最后一个参数类似数学中的集合,[5,12)表示从5到12，包括5但不包括12  
  spannable.setSpan(span, getText().length(),getText().length()+"[smile]".length(), Spannable.SPAN_INCLUSIVE_EXCLUSIVE);    
  setText(spannable);  
</code>
</pre>
## 三。用法详解之举例  
### 组合运用：
<pre>
<code>
  SpannableStringBuilder spannable=new SpannableStringBuilder(getText().toString());  
  CharacterStyle span_1=new StyleSpan(android.graphics.Typeface.ITALIC);  
  CharacterStyle span_2=new ForegroundColorSpan(Color.RED);  
  spannable.setSpan(span_1, start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
  spannable.setSpan(span_2, start, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
  setText(spannable); 
</pre>
<code>
### 案例：带有\n换行符的字符串都可以用此方法显示2种颜色
<pre>
<code>
  /** 
   * 带有\n换行符的字符串都可以用此方法显示2种颜色 
   * @param text 
   * @param color1 
   * @param color2 
   * @return 
   */  
  public SpannableStringBuilder highlight(String text,int color1,int color2,int fontSize){  
      SpannableStringBuilder spannable=new SpannableStringBuilder(text);//用于可变字符串  
      CharacterStyle span_0=null,span_1=null,span_2;  
      int end=text.indexOf("\n");  
      if(end==-1){//如果没有换行符就使用第一种颜色显示  
          span_0=new ForegroundColorSpan(color1);  
          spannable.setSpan(span_0, 0, text.length(), Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
      }else{  
          span_0=new ForegroundColorSpan(color1);  
          span_1=new ForegroundColorSpan(color2);  
          spannable.setSpan(span_0, 0, end, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
          spannable.setSpan(span_1, end+1, text.length(), Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
            
          span_2=new AbsoluteSizeSpan(fontSize);//字体大小  
          spannable.setSpan(span_2, end+1, text.length(), Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
      }  
      return spannable;  
  }
</code>
</pre>
参考网址：
http://zhidao.baidu.com/link?url=hEnfF2UUSO9bvin1wmY185MT64rX_rufyvNGsifV0UiKqCNp2eV4_3D3sgWmmvYQGVEqY5GqrpHHQEHgkTCIScBtUtJRoUtk8nOp0WsfI0m
