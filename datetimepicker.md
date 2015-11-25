## 日期和时间类 datePicker
###1.datetimepicker
功能：可实现日期选择、日期时间选择、日期时间范围选择。
依赖: jquery
使用方法：
①引入插件
```html
<!-- this should go after your </body> -->
<link rel="stylesheet" type="text/css" href="/jquery.datetimepicker.css"/ >
<script src="/jquery.js"></script>
<script src="/build/jquery.datetimepicker.full.min.js"></script>
```
②添加html元素
```html
<input id="datetimepicker" type="text" >
```
③生成datetimepicker实例
```javascript
jQuery('#datetimepicker').datetimepicker();
```
设置全局中文
```javascript
jQuery.datetimepicker.setLocale('zh')
```
自定义月份、星期 文本
```javascript
jQuery.datetimepicker.setLocale('zh');
jQuery('#datetimepicker1').datetimepicker({
 i18n:{
  zh:{
   months:[
    '一月','二月','三月','四月',
    '五月','六月','七月','八月',
    '九月','十月','十一月','十二月',
   ],
   dayOfWeek:[
    "周一.", "周二", "周三", "周四", 
    "周五", "周六", "周日",
   ]
  }
 },
 timepicker:false,
 format:'d.m.Y'
});
```
只显示时间/只能选择指定时间
```javascript
jQuery('#datetimepicker2').datetimepicker({
  format:'H:i',
  datepicker:false,
  //timepicker: true, //这项设为true 则显示时间
  allowTimes:[
  '12:00', '13:00', '15:00', 
  '17:00', '17:05', '17:20', '19:00', '20:00'
 ]
});
```
设置起始日期
```javascript
jQuery('#datetimepicker_start_time').datetimepicker({
  startDate:'1971/05/01'//or 1986/12/08
});
```
格式为unix时间戳
```javascript
jQuery('#datetimepicker_unixtime').datetimepicker({
  format:'unixtime'
});
```
日历插件设置为行内直接显示
```javascript
jQuery('#datetimepicker3').datetimepicker({
  inline:true
});
```
日历show/hide/distory
```javascript
jQuery('#datetimepicker').datetimepicker(); //需要先生成实例
jQuery('#image_button').click(function(){
  jQuery('#datetimepicker').datetimepicker('show'); //support hide,show and destroy command
});
```
日期时间变动事件
```javascript
jQuery('#datetimepicker6').datetimepicker({
  timepicker:false,
  onChangeDateTime:function(dp,$input){
    console.log($input.val());
  }
});
```
设定可选日期范围
```javascript
jQuery('#datetimepicker7').datetimepicker({
 timepicker:false,
 formatDate:'Y/m/d',
 minDate:'2015/11/12',
 maxDate:'2020/06/08'
});
```
```javascript
jQuery('#datetimepicker7').datetimepicker({
 timepicker:false,
 formatDate:'Y/m/d',
 minDate:'-1970/01/02',//yesterday is minimum date(for today use 0 or -1970/01/01)
 maxDate:'+1970/01/02'//tommorow is maximum date calendar
});
```
设置下划线提示
```javascript
jQuery('#datetimepicker_mask').datetimepicker({
 timepicker:false,
 mask:true, // '9999/19/39 29:59' - digit is the maximum possible for a cell
});
```
设置指定日期的可选时间
```javascript
var logic = function( currentDateTime ){
  // 'this' is jquery object datetimepicker
  if( currentDateTime.getDay()==6 ){
    this.setOptions({
      minTime:'11:00'
    });
  }else
    this.setOptions({
      minTime:'8:00'
    });
};
jQuery('#datetimepicker_rantime').datetimepicker({
  onChangeDateTime:logic,
  onShow:logic
});
```
选择日期段
```javascript
jQuery(function(){
 jQuery('#date_timepicker_start').datetimepicker({
  format:'Y/m/d',
  onShow:function( ct ){
   this.setOptions({
    maxDate:jQuery('#date_timepicker_end').val()?jQuery('#date_timepicker_end').val():false
   })
  },
  timepicker:false
 });
 jQuery('#date_timepicker_end').datetimepicker({
  format:'Y/m/d',
  onShow:function( ct ){
   this.setOptions({
    minDate:jQuery('#date_timepicker_start').val()?jQuery('#date_timepicker_start').val():false
   })
  },
  timepicker:false
 });
});
```

####参数文档
<table class="table table-condensed table-bordered table-striped">
<thead>
<tr><th style="text-align: center;"><strong>参数名</strong></th><th style="text-align: center;"><strong>默认值</strong></th><th style="text-align: center;"><strong>描述</strong></th><th style="width: 200px; text-align: center;"><strong>示例</strong></th></tr>
</thead>
<tbody>
<tr id="lazyInit">
<td><a href="#lazyInit">lazyInit</a></td>
<td>false</td>
<td>Initializing plugin occurs only when the user interacts. Greatly accelerates plugin work with a large number of fields</td>
<td> </td>
</tr>
<tr id="value">
<td><a href="#value">value</a></td>
<td>null</td>
<td>当前日期时间，如果设置该项，input的value属性值会被忽略</td>
<td>
<pre><code data-language="javascript" class="rainbow">{value:<span class="string">'12.03.2013'</span>,
 format:<span class="string">'d.m.Y'</span>}</code></pre>
</td>
</tr>
<tr id="lang">
<td><a href="#lang">lang</a></td>
<td>en</td>
<td>Language i18n<br> 

<strong>ar</strong> -  Arabic
<br><strong>az</strong> -  Azerbaijanian (Azeri)
<br><strong>bg</strong> -  Bulgarian
<br><strong>bs</strong> -  Bosanski
<br><strong>ca</strong> -  Català
<br><strong>ch</strong> -  Simplified Chinese
<br><strong>cs</strong> -  Čeština
<br><strong>da</strong> -  Dansk
<br><strong>de</strong> -  German
<br><strong>el</strong> -  Ελληνικά
<br><strong>en</strong> -  English
<br><strong>en-GB</strong> -  English (British)
<br><strong>es</strong> -  Spanish
<br><strong>et</strong> -  "Eesti"
<br><strong>eu</strong> -  Euskara
<br><strong>fa</strong> -  Persian
<br><strong>fi</strong> -  Finnish (Suomi)
<br><strong>fr</strong> -  French
<br><strong>gl</strong> -  Galego
<br><strong>he</strong> -  Hebrew (עברית)
<br><strong>hr</strong> -  Hrvatski
<br><strong>hu</strong> -  Hungarian
<br><strong>id</strong> -  Indonesian
<br><strong>it</strong> -  Italian
<br><strong>ja</strong> -  Japanese
<br><strong>ko</strong> -  Korean (한국어)
<br><strong>kr</strong> -  Korean
<br><strong>lt</strong> -  Lithuanian (lietuvių)
<br><strong>lv</strong> -  Latvian (Latviešu)
<br><strong>mk</strong> -  Macedonian (Македонски)
<br><strong>mn</strong> -  Mongolian (Монгол)
<br><strong>nl</strong> -  Dutch
<br><strong>no</strong> -  Norwegian
<br><strong>pl</strong> -  Polish
<br><strong>pt</strong> -  Portuguese
<br><strong>pt-BR</strong> -  Português(Brasil)
<br><strong>ro</strong> -  Romanian
<br><strong>ru</strong> -  Russian
<br><strong>se</strong> -  Swedish
<br><strong>sk</strong> -  Slovenčina
<br><strong>sl</strong> -  Slovenščina
<br><strong>sq</strong> -  Albanian (Shqip)
<br><strong>sr</strong> -  Serbian Cyrillic (Српски)
<br><strong>sr-YU</strong> -  Serbian (Srpski)
<br><strong>sv</strong> -  Svenska
<br><strong>th</strong> -  Thai
<br><strong>tr</strong> -  Turkish
<br><strong>uk</strong> -  Ukrainian
<br><strong>vi</strong> -  Vietnamese
<br><strong>zh</strong> -  Simplified Chinese (简体中文)
<br><strong>zh-TW</strong> -  Traditional Chinese (繁體中文)
<br>

  
</td>
<td>
<pre><code data-language="javascript" class="rainbow"><span class="selector">$</span>.datetimepicker.<span class="function call">setLocale</span>(<span class="string">'ru'</span>);</code></pre>
</td>
</tr>
<tr id="format">
<td><a href="#format">format</a></td>
<td>Y/m/d H:i</td>
<td>格式化日期时间. <a href="http://php.net/manual/ru/function.date.php" target="_blank">更多</a>  特殊类型 <a href="#unixtime"><em>«unixtime»</em></a></td>
<td>
<pre><code data-language="javascript" class="rainbow">{format:<span class="string">'H'</span>}
{format:<span class="string">'Y'</span>}{format:<span class="string">'unixtime'</span>}</code></pre>
</td>
</tr>
<tr id="formatDate">
<td><a href="#formatDate">formatDate</a></td>
<td>Y/m/d</td>
<td>Format date for minDate and maxDate</td>
<td>
<pre><code data-language="javascript" class="rainbow">{formatDate:<span class="string">'d.m.Y'</span>}</code></pre>
</td>
</tr>
<tr id="formatTime">
<td><a href="#formatTime">formatTime</a></td>
<td>H:i</td>
<td> Similarly, formatDate . But for minTime and maxTime</td>
<td>
<pre><code data-language="javascript" class="rainbow">{formatTime:<span class="string">'H'</span>}</code></pre>
</td>
</tr>
<tr id="step">
<td><a href="#step">step</a></td>
<td>60</td>
<td>Step time</td>
<td>
<pre><code data-language="javascript" class="rainbow">{step:<span class="constant numeric">5</span>}</code></pre>
</td>
</tr>
<tr id="closeOnDateSelect">
<td><a href="#closeOnDateSelect">closeOnDateSelect</a></td>
<td>0</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{closeOnDateSelect:<span class="constant language">true</span>}</code></pre>
</td>
</tr>
<tr id="closeOnWithoutClick">
<td><a href="#closeOnWithoutClick">closeOnWithoutClick</a></td>
<td>true</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{ closeOnWithoutClick :<span class="constant language">false</span>}</code></pre>
</td>
</tr>
<tr id="validateOnBlur">
<td><a href="#validateOnBlur">validateOnBlur</a></td>
<td>true</td>
<td>Verify datetime value from input, when losing focus. If value is not valid datetime, then to value inserts the current datetime</td>
<td>
<pre><code data-language="javascript" class="rainbow">{ validateOnBlur:<span class="constant language">false</span>}</code></pre>
</td>
</tr>
<tr id="timepicker">
<td><a href="#timepicker">timepicker</a></td>
<td>true</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{timepicker:<span class="constant language">false</span>}</code></pre>
</td>
</tr>
<tr id="datepicker">
<td><a href="#datepicker">datepicker</a></td>
<td>true</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{datepicker:<span class="constant language">false</span>}</code></pre>
</td>
</tr>
<tr id="weeks">
<td><a href="#weeks">weeks</a></td>
<td>false</td>
<td>Show week number</td>
<td>
<pre><code data-language="javascript" class="rainbow">{weeks:<span class="constant language">true</span>}</code></pre>
</td>
</tr>
<tr id="theme">
<td><a href="#theme">theme</a></td>
<td>'default'</td>
<td>Setting a color scheme. Now only supported default and dark theme</td>
<td>
<pre><code data-language="javascript" class="rainbow">{theme:<span class="string">'dark'</span>}</code></pre>
</td>
</tr>
<tr id="minDate">
<td><a href="#minDate">minDate</a></td>
<td>false</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{minDate:<span class="constant numeric">0</span>} <span class="comment">// today</span>
{minDate:<span class="string">'2013/12/03'</span>}
{minDate:'<span class="keyword operator">-</span><span class="constant numeric">1970</span><span class="string regexp"><span class="string regexp open">/</span>01/02'} /<span class="string regexp close">/</span></span> yesterday
{minDate:<span class="string">'05.12.2013'</span>,formatDate:<span class="string">'d.m.Y'</span>}</code></pre>
</td>
</tr>
<tr id="maxDate">
<td><a href="#maxDate">maxDate</a></td>
<td>false</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{maxDate:<span class="constant numeric">0</span>}
{maxDate:<span class="string">'2013/12/03'</span>}
{maxDate:'<span class="keyword operator">+</span><span class="constant numeric">1970</span><span class="string regexp"><span class="string regexp open">/</span>01/02'} /<span class="string regexp close">/</span></span> tommorrow
{maxDate:<span class="string">'05.12.2013'</span>,formatDate:<span class="string">'d.m.Y'</span>}</code></pre>
</td>
</tr>
<tr id="startDate">
<td><a href="#starDate">startDate</a></td>
<td>false</td>
<td>calendar set date use starDate</td>
<td>
<pre><code data-language="javascript" class="rainbow">{startDate:<span class="string">'1987/12/03'</span>}
{startDate:<span class="keyword">new</span> <span class="entity function">Date</span>()}
{startDate:'<span class="keyword operator">+</span><span class="constant numeric">1970</span><span class="string regexp"><span class="string regexp open">/</span>01/02'} /<span class="string regexp close">/</span></span> tommorrow
{startDate:<span class="string">'08.12.1986'</span>,formatDate:<span class="string">'d.m.Y'</span>}</code></pre>
</td>
</tr>

<tr id="defaultDate">
<td><a href="#defaultDate">defaultDate</a></td>
<td>false</td>
<td>if input value is empty, calendar set date use defaultDate</td>
<td>
<pre><code data-language="javascript" class="rainbow">{defaultDate:<span class="string">'1987/12/03'</span>}
{defaultDate:<span class="keyword">new</span> <span class="entity function">Date</span>()}
{defaultDate:'<span class="keyword operator">+</span><span class="constant numeric">1970</span><span class="string regexp"><span class="string regexp open">/</span>01/02'} /<span class="string regexp close">/</span></span> tommorrow
{defaultDate:<span class="string">'08.12.1986'</span>,formatDate:<span class="string">'d.m.Y'</span>}</code></pre>
</td>
</tr>  
  
<tr id="defaultTime">
<td><a href="#defaultTime">defaultTime</a></td>
<td>false</td>
<td>if input value is empty, timepicker set time use defaultTime</td>
<td>
<pre><code data-language="javascript" class="rainbow">{defaultTime:<span class="string">'05:00'</span>}
{defaultTime:<span class="string">'33-12'</span>,formatTime:<span class="string">'i-H'</span>}</code></pre>
</td>
</tr>  
  
<tr id="minTime">
<td><a href="#minTime">minTime</a></td>
<td>false</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{minTime:<span class="constant numeric">0</span>,}<span class="comment">// now</span>
{minTime:<span class="keyword">new</span> <span class="entity function">Date</span>()}
{minTime:<span class="string">'12:00'</span>}
{minTime:<span class="string">'13:45:34'</span>,formatTime:<span class="string">'H:i:s'</span>}</code></pre>
</td>
</tr>
<tr id="maxTime">
<td><a href="#maxTime">maxTime</a></td>
<td>false</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{maxTime:<span class="constant numeric">0</span>,}
{maxTime:<span class="string">'12:00'</span>}
{maxTime:<span class="string">'13:45:34'</span>,formatTime:<span class="string">'H:i:s'</span>}</code></pre>
</td>
</tr>
<tr id="allowTimes">
<td><a href="#allowTimes">allowTimes</a></td>
<td>[]</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">{allowTimes:[
  <span class="string">'09:00'</span>,
  <span class="string">'11:00'</span>,
  <span class="string">'12:00'</span>,
  <span class="string">'21:00'</span>
]}</code></pre>
</td>
</tr>
<tr id="mask">
<td><a href="#mask">mask</a></td>
<td>false</td>
<td>Use mask for input. true - automatically generates a mask on the field 'format', Digit from 0 to 9, set the highest possible digit for the value. For example: the first digit of hours can not be greater than 2, and the first digit of the minutes can not be greater than 5</td>
<td>
<pre>{mask:'9999/19/39',format:'Y/m/d'}
{mask:true,format:'Y/m/d'} // automatically generate a mask 9999/99/99
{mask:'29:59',format:'H:i'} //
{mask:true,format:'H:i'} //automatically generate a mask 99:99</pre>
</td>
</tr>
<tr id="opened">
<td><a href="#opened">opened</a></td>
<td>false</td>
<td> </td>
<td> </td>
</tr>
<tr id="yearoffset">
<td><a href="#yearoffset">yearOffset</a></td>
<td>0</td>
<td>Year offset for Buddhist era</td>
<td> </td>
</tr>
<tr id="inline">
<td><a href="#inline">inline</a></td>
<td>false</td>
<td> </td>
<td> </td>
</tr>
<tr id="todayButton">
<td><a href="#todayButton">todayButton</a></td>
<td>true</td>
<td>Show button "Go To Today"</td>
<td> </td>
</tr>
<tr id="defaultSelect">
<td><a href="#defaultSelect">defaultSelect</a></td>
<td>true</td>
<td>Highlight the current date even if the input is empty</td>
<td> </td>
</tr>
<tr id="allowBlank">
<td><a href="#allowBlank">allowBlank</a></td>
<td>false</td>
<td>Allow field to be empty even with the option <a href="#validateOnBlur">validateOnBlur</a> in true</td>
<td> </td>
</tr>
<tr id="timepickerScrollbar">
<td><a href="#timepickerScrollbar">timepickerScrollbar</a></td>
<td>true</td>
<td> </td>
<td> </td>
</tr>
<tr id="onSelectDate">
<td><a href="#onSelectDate">onSelectDate</a></td>
<td>function(){}</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow"><span class="entity function">onSelectDate</span>:<span class="keyword">function</span>(ct,$i){
  <span class="function call">alert</span>(ct.<span class="function call">dateFormat</span>(<span class="string">'d/m/Y'</span>))
}</code></pre>
</td>
</tr>
<tr id="onSelectTime">
<td><a href="#onSelectTime">onSelectTime</a></td>
<td>function(current_time,$input){}</td>
<td> </td>
<td> </td>
</tr>
<tr id="onChangeMonth">
<td><a href="#onChangeMonth">onChangeMonth</a></td>
<td>function(current_time,$input){}</td>
<td> </td>
<td> </td>
</tr>
 <tr id="onChangeYear">
<td><a href="#onChangeYear">onChangeYear</a></td>
<td>function(current_time,$input){}</td>
<td> </td>
<td> </td>
</tr>
<tr id="onChangeDateTime">
<td><a href="#onChangeDateTime">onChangeDateTime</a></td>
<td>function(current_time,$input){}</td>
<td> </td>
<td> </td>
</tr>
<tr id="onShow">
<td><a href="#onShow">onShow</a></td>
<td>function(current_time,$input){}</td>
<td> </td>
<td> </td>
</tr>
<tr id="onClose">
<td><a href="#onClose">onClose</a></td>
<td>function(current_time,$input){}</td>
<td> </td>
<td><pre><code data-language="javascript" class="rainbow"><span class="entity function">onSelectDate</span>:<span class="keyword">function</span>(ct,$i){
  $i.<span class="function call">datetimepicker</span>(<span class="string">'destroy'</span>);
}</code></pre></td>
</tr>
<tr id="onGenerate">
<td><a href="#onGenerate">onGenerate</a></td>
<td>function(current_time,$input){}</td>
<td>trigger after construct calendar and timepicker</td>
<td> </td>
</tr>
<tr>
<td>withoutCopyright</td>
<td>true</td>
<td> </td>
<td> </td>
</tr>
<tr id="inverseButton">
<td><a href="#inverseButton">inverseButton</a></td>
<td>false</td>
<td> </td>
<td> </td>
</tr>
<tr id="scrollMonth">
<td><a href="#scrollMonth">scrollMonth</a></td>
<td>true</td>
<td> </td>
<td> </td>
</tr>
<tr id="scrollTime">
<td><a href="#scrollTime">scrollTime</a></td>
<td>true</td>
<td> </td>
<td> </td>
</tr>
<tr id="scrollInput">
<td><a href="#scrollInput">scrollInput</a></td>
<td>true</td>
<td> </td>
<td> </td>
</tr>
<tr id="hours12">
<td><a href="#hours12">hours12</a></td>
<td>false</td>
<td> </td>
<td> </td>
</tr>
<tr id="yearStart">
<td><a href="#yearStart">yearStart</a></td>
<td>1950</td>
<td>Start value for fast Year selector</td>
<td> </td>
</tr>
<tr id="yearEnd">
<td><a href="#yearEnd">yearEnd</a></td>
<td>2050</td>
<td>End value for fast Year selector</td>
<td> </td>
</tr>
<tr id="roundTime">
<td><a href="#roundTime">roundTime</a></td>
<td>round</td>
<td>Round time in timepicker, possible values: round, ceil, floor</td>
<td>
<pre><code data-language="javascript" class="rainbow">{roundTime:<span class="string">'floor'</span>}</code></pre>
</td>
</tr>
<tr id="dayOfWeekStart">
<td><a href="#dayOfWeekStart">dayOfWeekStart</a></td>
<td>0</td>
<td>
<p>Star week DatePicker. Default Sunday - 0.</p>
<p>Monday - 1 ...</p>
</td>
<td> </td>
</tr>
<tr id="className">
<td>className</td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr id="weekends">
<td><a href="#weekends">weekends</a></td>
<td>[]</td>
<td> </td>
<td>
<pre><code data-language="javascript" class="rainbow">[<span class="string">'01.01.2014'</span>,'<span class="constant numeric">02.01</span>.<span class="constant numeric">2014</span>','<span class="constant numeric">03.01</span>.<span class="constant numeric">2014</span>','<span class="constant numeric">04.01</span>.<span class="constant numeric">2014</span>','<span class="constant numeric">05.01</span>.<span class="constant numeric">2014</span>','<span class="constant numeric">06.01</span>.<span class="constant numeric">2014</span>']</code></pre>
</td>
</tr>
<tr id="disabledDates">
<td><a href="#disabledDates">disabledDates</a></td>
<td>[]</td>
<td><p>Disbale all dates in list</p></td>
<td>
<pre><code data-language="javascript" class="rainbow">{disabledDates: [<span class="string">'01.01.2014'</span>,'<span class="constant numeric">02.01</span>.<span class="constant numeric">2014</span>','<span class="constant numeric">03.01</span>.<span class="constant numeric">2014</span>','<span class="constant numeric">04.01</span>.<span class="constant numeric">2014</span>','<span class="constant numeric">05.01</span>.<span class="constant numeric">2014</span>','<span class="constant numeric">06.01</span>.<span class="constant numeric">2014</span>'], formatDate:<span class="string">'d.m.Y'</span>}</code></pre>
</td>
</tr>
<tr id="id">
<td>id</td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr id="style">
<td>style</td>
<td> </td>
<td> </td>
<td> </td>
</tr>
</tbody>
</table>
