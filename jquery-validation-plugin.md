##表单验证 formValidata
###1.jQuery Validation Plugin
功能: 检验字段合法性
依赖: jquery
使用方法:
①引入插件
```html
<script src="../js/jquery.js" type="text/javascript"></script>
<script src="../js/jquery.validate.js" type="text/javascript"></script>
<script src="../js/jquery.metadata.js" type="text/javascript"></script>
```

②在标签中声明验证规则
```html
<form id="signupForm" method="get" action="">
 <p>
     <label for="firstname">Firstname</label>
     <input id="firstname" name="firstname" class="required" />
 </p>
 <p>
  <label for="email">E-Mail</label>
  <input id="email" name="email" class="required email" />
 </p>
 <p>
  <label for="password">Password</label>
  <input id="password" name="password" type="password" class="{required:true,minlength:5}" />
 </p>
 <p>
  <label for="confirm_password">确认密码</label>
  <input id="confirm_password" name="confirm_password" type="password" class="{required:true,minlength:5,equalTo:'#password'}" />
 </p>
 <p>
     <input class="submit" type="submit" value="Submit"/>
 </p>
</form>
```
使用 `class="{}"` 的方式，必须引入包：jquery.metadata.js。
可以使用如下的方法，修改提示内容：
```html
class="{required:true,minlength:5,messages:{required:'请输入内容'}}"
```
在使用 equalTo 关键字时，后面的内容必须加上引号，代码如下所示：
```html
class="{required:true,minlength:5,equalTo:'#password'}"
```
③在js中设置验证规则
```javascript
$(function() {
 $("#signupForm").validate({
  rules: {
   firstname: "required",
   email: {
    required: true,
    email: true
   },
   password: {
    required: true,
    minlength: 5
   },
   confirm_password: {
    required: true,
    minlength: 5,
    equalTo: "#password"
   }
  },
  messages: {
   firstname: "请输入姓名",
   email: {
    required: "请输入Email地址",
    email: "请输入正确的email地址"
   },
   password: {
    required: "请输入密码",
    minlength: jQuery.format("密码不能小于{0}个字 符")
   },
   confirm_password: {
    required: "请输入确认密码",
    minlength: "确认密码不能小于5个字符",
    equalTo: "两次输入密码不一致不一致"
   }
  }
 });
});
```
`required:true` 必须有值。
`required:"#aa:checked"` 表达式的值为真，则需要验证。
`required:function(){}` 返回为真，表示需要验证。
后边两种常用于，表单中需要同时填或不填的元素。

默认提示:
```javascript
messages: {
    required: "This field is required.",
    remote: "Please fix this field.",
    email: "Please enter a valid email address.",
    url: "Please enter a valid URL.",
    date: "Please enter a valid date.",
    dateISO: "Please enter a valid date (ISO).",
    dateDE: "Bitte geben Sie ein gültiges Datum ein.",
    number: "Please enter a valid number.",
    numberDE: "Bitte geben Sie eine Nummer ein.",
    digits: "Please enter only digits",
    creditcard: "Please enter a valid credit card number.",
    equalTo: "Please enter the same value again.",
    accept: "Please enter a value with a valid extension.",
    maxlength: $.validator.format("Please enter no more than {0} characters."),
    minlength: $.validator.format("Please enter at least {0} characters."),
    rangelength: $.validator.format("Please enter a value between {0} and {1} characters long."),
    range: $.validator.format("Please enter a value between {0} and {1}."),
    max: $.validator.format("Please enter a value less than or equal to {0}."),
    min: $.validator.format("Please enter a value greater than or equal to {0}.")
}
```
引入中文提示库:
```html
<script src="../js/messages_cn.js" type="text/javascript"></script>
```
####常用方法及注意问题
1. 用其他方式替代默认的submit
```javascript
$(function() {
	$("#signupForm").validate({
        submitHandler:function(form){
            alert("submitted");   
            form.submit();
        }    
    });
});
```
使用ajax方式
```javascript
$(".selector").validate({     
	 submitHandler: function(form){      
		 $(form).ajaxSubmit();     
	 }  
});
```
可以设置 validate 的默认值，写法如下：
```javascript
$.validator.setDefaults({
	submitHandler: function(form) { alert("submitted!");form.submit(); }
});
```
如果想提交表单, 需要使用 `form.submit()`，而不要使用 `$(form).submit()`。
2. debug, 只验证不提交
```javascript
$(function() {
	$("#signupForm").validate({
        debug:true
    });
});
```
如果一个页面中有多个表单都想设置成为 debug，则使用：
```javascript
$.validator.setDefaults({
   debug: true
})
```
3. ignore：忽略某些元素不验证
```javascript
ignore: ".ignore"
```
4. 更改错误信息显示的位置
```javascript
errorPlacement：Callback
```
指明错误放置的位置，默认情况是：`error.appendTo(element.parent());`即把错误信息放在验证的元素后面。
```javascript
errorPlacement: function(error, element) {  
    error.appendTo(element.parent());  
}
```
实例:
```html
<tr>
    <td class="label"><label id="lfirstname" for="firstname">First Name</label></td>
    <td class="field"><input id="firstname" name="firstname" type="text" value="" maxlength="100" /></td>
    <td class="status"></td>
</tr>
<tr>
    <td style="padding-right: 5px;">
        <input id="dateformat_eu" name="dateformat" type="radio" value="0" />
        <label id="ldateformat_eu" for="dateformat_eu">14/02/07</label>
    </td>
    <td style="padding-left: 5px;">
        <input id="dateformat_am" name="dateformat" type="radio" value="1"  />
        <label id="ldateformat_am" for="dateformat_am">02/14/07</label>
    </td>
    <td></td>
</tr>
<tr>
    <td class="label">&nbsp;</td>
    <td class="field" colspan="2">
        <div id="termswrap">
            <input id="terms" type="checkbox" name="terms" />
            <label id="lterms" for="terms">I have read and accept the Terms of Use.</label>
        </div>
    </td>
</tr>

errorPlacement: function(error, element) {
    if ( element.is(":radio") )
        error.appendTo( element.parent().next().next() );
    else if ( element.is(":checkbox") )
        error.appendTo ( element.next() );
    else
        error.appendTo( element.parent().next() );
}
```
代码的作用是：一般情况下把错误信息显示在 `<td class="status"></td>` 中，如果是 radio 则显示在 `<td></td> `中，如果是 checkbox 则显示在内容的后面。
<table class="reference notranslate">
<tbody><tr>
	<th width="15%">参数</th>
	<th width="15%">类型</th>
    <th width="60%">描述</th>
	<th width="10%">默认值</th>
</tr>
<tr>
	<td>errorClass</td>
    <td>String</td>
	<td>指定错误提示的 css 类名，可以自定义错误提示的样式。</td>
    <td>"error"</td>
</tr>
<tr>
	<td>errorElement</td>
    <td>String</td>
	<td>用什么标签标记错误，默认是 label，可以改成 em。</td>
    <td>"label"</td>
</tr>
<tr>
	<td>errorContainer</td>
    <td>Selector</td>
	<td>显示或者隐藏验证信息，可以自动实现有错误信息出现时把容器属性变为显示，无错误时隐藏，用处不大。<br>errorContainer: "#messageBox1, #messageBox2"</td>
    <td></td>
</tr>
<tr>
	<td>errorLabelContainer</td>
    <td>Selector</td>
	<td>把错误信息统一放在一个容器里面。</td>
    <td></td>
</tr>
<tr>
	<td>wrapper</td>
    <td>String</td>
	<td>用什么标签再把上边的 errorELement 包起来。</td>
    <td></td>
</tr>
</tbody></table>
一般这三个属性同时使用，实现在一个容器内显示所有错误提示的功能，并且没有信息时自动隐藏。
```javascript
errorContainer: "div.error",
errorLabelContainer: $("#signupForm div.error"),
wrapper: "li"
```
5. 验证的触发方式修改
<table class="reference notranslate">
<tbody><tr>
	<th width="15%">触发方式</th>
	<th width="15%">类型</th>
    <th width="60%">描述</th>
	<th width="10%">默认值</th>
</tr>
<tr>
	<td>onsubmit</td>
    <td>Boolean</td>
	<td>提交时验证。设置为 false 就用其他方法去验证。</td>
    <td>true</td>
</tr>
<tr>
	<td>onfocusout</td>
    <td>Boolean</td>
	<td>失去焦点时验证（不包括复选框/单选按钮）。</td>
    <td>true</td>
</tr>
<tr>
	<td>onkeyup</td>
    <td>Boolean</td>
	<td>在 keyup 时验证。</td>
    <td>true</td>
</tr>
<tr>
	<td>onclick</td>
    <td>Boolean</td>
	<td>在点击复选框和单选按钮时验证。</td>
    <td>true</td>
</tr>
<tr>
	<td>focusInvalid</td>
    <td>Boolean</td>
	<td>提交表单后，未通过验证的表单（第一个或提交之前获得焦点的未通过验证的表单）会获得焦点。</td>
    <td>true</td>
</tr>
<tr>
	<td>focusCleanup</td>
    <td>Boolean</td>
	<td>如果是 true 那么当未通过验证的元素获得焦点时，移除错误提示。避免和 focusInvalid 一起用。</td>
    <td>false</td>
</tr>
</tbody></table>
6. 重置表单
```javascript
// 重置表单
$(function() {
	 var validator = $("#signupForm").validate({});
	 $("#reset").click(function() {
	     validator.resetForm();
	 });
 });
```
7. 异步验证

`remote：URL`
```javascript
remote: {
    url: "check-email.php",     //后台处理程序
    type: "post",               //数据发送方式
    dataType: "json",           //接受数据格式   
    data: {                     //要传递的数据
        username: function() {
            return $("#username").val();
        }
    }
}
```
8. radio 和 checkbox、select 的验证

radio 的 required 表示必须选中一个。
```html
<input  type="radio" id="gender_male" value="m" name="gender" class="{required:true}">
<input  type="radio" id="gender_female" value="f" name="gender">
```
checkbox 的 required 表示必须选中。
```html
<input type="checkbox" class="checkbox" id="agree" name="agree" class="{required:true}">
```
checkbox 的 minlength 表示必须选中的最小个数，maxlength 表示最大的选中个数，rangelength:[2,3] 表示选中个数区间。
```html
<input type="checkbox" class="checkbox" id="spam_email" value="email" name="spam[]" class="{required:true, minlength:2}" />
<input type="checkbox" class="checkbox" id="spam_phone" value="phone" name="spam[]">
<input type="checkbox" class="checkbox" id="spam_mail" value="mail" name="spam[]">
```
select 的 required 表示选中的 value 不能为空。
```html
<select id="jungle" name="jungle" title="Please select something!" class="{required:true}">
    <option value=""></option>
    <option value="1">Buga</option>
    <option value="2">Baga</option>
    <option value="3">Oi</option>
</select>
```
select 的 minlength 表示选中的最小个数（可多选的 select），maxlength 表示最大的选中个数，rangelength:[2,3] 表示选中个数区间。
```html
<select id="fruit" name="fruit" title="Please select at least two fruits" class="{required:true, minlength:2}" multiple="multiple">
    <option value="b">Banana</option>
    <option value="a">Apple</option>
    <option value="p">Peach</option>
    <option value="t">Turtle</option>
</select>
```
####jQuery.validate 中文 API
<table class="reference notranslate">
<tbody><tr>
	<th width="20%">名称</th>
	<th width="20%">返回类型</th>
    <th width="60%">描述</th>
</tr>
<tr>
	<td>validate(options)</td>
    <td>Validator</td>
	<td>验证所选的 FORM。</td>
</tr>
<tr>
	<td>valid()</td>
    <td>Boolean</td>
	<td>检查是否验证通过。</td>
</tr>
<tr>
	<td>rules()</td>
    <td>Options</td>
	<td>返回元素的验证规则。</td>
</tr>
<tr>
	<td>rules("add",rules)</td>
    <td>Options</td>
	<td>增加验证规则。</td>
</tr>
<tr>
	<td>rules("remove",rules)</td>
    <td>Options</td>
	<td>删除验证规则。</td>
</tr>
<tr>
	<td>removeAttrs(attributes)</td>
    <td>Options</td>
	<td>删除特殊属性并且返回它们。</td>
</tr>
<tr>
	<td colspan="3">自定义选择器</td>
</tr>
<tr>
	<td>:blank</td>
    <td>Validator</td>
	<td>没有值的筛选器。</td>
</tr>
<tr>
	<td>:filled</td>
    <td>Array &lt;Element&gt;</td>
	<td>有值的筛选器。</td>
</tr>
<tr>
	<td>:unchecked</td>
    <td>Array &lt;Element&gt;</td>
	<td>没选择的元素的筛选器。</td>
</tr>
<tr>
	<td colspan="3">实用工具</td>
</tr>
<tr>
	<td>jQuery.format(template,argument,argumentN...)</td>
    <td>String</td>
	<td>用参数代替模板中的 {n}。</td>
</tr>
</tbody></table>
#####Validator
validate 方法返回一个 Validator 对象。Validator 对象有很多方法可以用来引发校验程序或者改变 form 的内容，下面列出几个常用的方法。
<table class="reference notranslate">
<tbody><tr>
	<th width="20%">名称</th>
	<th width="20%">返回类型</th>
    <th width="60%">描述</th>
</tr>
<tr>
	<td>form()</td>
    <td>Boolean</td>
	<td>验证 form 返回成功还是失败。</td>
</tr>
<tr>
	<td>element(element)</td>
    <td>Boolean</td>
	<td>验证单个元素是成功还是失败。</td>
</tr>
<tr>
	<td>resetForm()</td>
    <td>undefined</td>
	<td>把前面验证的 FORM 恢复到验证前原来的状态。</td>
</tr>
<tr>
	<td>showErrors(errors)</td>
    <td>undefined</td>
	<td>显示特定的错误信息。</td>
</tr>
<tr>
	<td colspan="3">Validator 函数</td>
</tr>
<tr>
	<td>setDefaults(defaults)</td>
    <td>undefined</td>
	<td>改变默认的设置。</td>
</tr>
<tr>
	<td>addMethod(name,method,message)</td>
    <td>undefined</td>
	<td>添加一个新的验证方法。必须包括一个独一无二的名字，一个 JAVASCRIPT 的方法和一个默认的信息。</td>
</tr>
<tr>
	<td>addClassRules(name,rules)</td>
    <td>undefined</td>
	<td>增加组合验证类型，在一个类里面用多种验证方法时比较有用。</td>
</tr>
<tr>
	<td>addClassRules(rules)</td>
    <td>undefined</td>
	<td>增加组合验证类型，在一个类里面用多种验证方法时比较有用。这个是同时加多个验证方法。</td>
</tr>
</tbody></table>
#####内置验证方式
<table class="reference notranslate">
<tbody><tr>
	<th width="20%">名称</th>
	<th width="20%">返回类型</th>
    <th width="60%">描述</th>
</tr>
<tr>
	<td>required()</td>
    <td>Boolean</td>
	<td>必填验证元素。</td>
</tr>
<tr>
	<td>required(dependency-expression)</td>
    <td>Boolean</td>
	<td>必填元素依赖于表达式的结果。</td>
</tr>
<tr>
	<td>required(dependency-callback)</td>
    <td>Boolean</td>
	<td>必填元素依赖于回调函数的结果。</td>
</tr>
<tr>
	<td>remote(url)</td>
    <td>Boolean</td>
	<td>请求远程校验。url 通常是一个远程调用方法。</td>
</tr>
<tr>
	<td>minlength(length)</td>
    <td>Boolean</td>
	<td>设置最小长度。</td>
</tr>
<tr>
	<td>maxlength(length)</td>
    <td>Boolean</td>
	<td>设置最大长度。</td>
</tr>
<tr>
	<td>rangelength(range)</td>
    <td>Boolean</td>
	<td>设置一个长度范围 [min,max]。</td>
</tr>
<tr>
	<td>min(value)</td>
    <td>Boolean</td>
	<td>设置最小值。</td>
</tr>
<tr>
	<td>max(value)</td>
    <td>Boolean</td>
	<td>设置最大值。</td>
</tr>
<tr>
	<td>email()</td>
    <td>Boolean</td>
	<td>验证电子邮箱格式。</td>
</tr>
<tr>
	<td>range(range)</td>
    <td>Boolean</td>
	<td>设置值的范围。</td>
</tr>
<tr>
	<td>url()</td>
    <td>Boolean</td>
	<td>验证 URL 格式。</td>
</tr>
<tr>
	<td>date()</td>
    <td>Boolean</td>
	<td>验证日期格式（类似 30/30/2008 的格式，不验证日期准确性只验证格式）。</td>
</tr>
<tr>
	<td>dateISO()</td>
    <td>Boolean</td>
	<td>验证 ISO 类型的日期格式。</td>
</tr>
<tr>
	<td>dateDE()</td>
    <td>Boolean</td>
	<td>验证德式的日期格式（29.04.1994 或 1.1.2006）。</td>
</tr>
<tr>
	<td>number()</td>
    <td>Boolean</td>
	<td>验证十进制数字（包括小数的）。</td>
</tr>
<tr>
	<td>digits()</td>
    <td>Boolean</td>
	<td>验证整数。</td>
</tr>
<tr>
	<td>creditcard()</td>
    <td>Boolean</td>
	<td>验证信用卡号。</td>
</tr>
<tr>
	<td>accept(extension)</td>
    <td>Boolean</td>
	<td>验证相同后缀名的字符串。</td>
</tr>
<tr>
	<td>equalTo(other)</td>
    <td>Boolean</td>
	<td>验证两个输入框的内容是否相同。</td>
</tr>
<tr>
	<td>phoneUS()</td>
    <td>Boolean</td>
	<td>验证美式的电话号码。</td>
</tr>
</tbody></table>
#####validate ()的可选项
<table class="reference notranslate">
<tbody><tr>
	<th width="30%">描述</th>
	<th width="70%">代码</th>
</tr>
<tr>
	<td>debug：进行调试模式（表单不提交）。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate
</span><span class="pun">({</span><span class="pln">
	debug</span><span class="pun">:</span><span class="kwd">true</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>把调试设置为默认。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">.</span><span class="pln">validator</span><span class="pun">.</span><span class="pln">setDefaults</span><span class="pun">({</span><span class="pln">
	debug</span><span class="pun">:</span><span class="kwd">true</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>submitHandler：通过验证后运行的函数，里面要加上表单提交的函数，否则表单不会提交。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
	submitHandler</span><span class="pun">:</span><span class="kwd">function</span><span class="pun">(</span><span class="pln">form</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
		$</span><span class="pun">(</span><span class="pln">form</span><span class="pun">).</span><span class="pln">ajaxSubmit</span><span class="pun">();</span><span class="pln">
	</span><span class="pun">}</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>ignore：对某些元素不进行验证。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">"#myform"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
	ignore</span><span class="pun">:</span><span class="str">".ignore"</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>rules：自定义规则，key:value 的形式，key 是要验证的元素，value 可以是字符串或对象。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
	rules</span><span class="pun">:{</span><span class="pln">
		name</span><span class="pun">:</span><span class="str">"required"</span><span class="pun">,</span><span class="pln">
		email</span><span class="pun">:{</span><span class="pln">
			required</span><span class="pun">:</span><span class="kwd">true</span><span class="pun">,</span><span class="pln">
			email</span><span class="pun">:</span><span class="kwd">true</span><span class="pln">
		</span><span class="pun">}</span><span class="pln">
	</span><span class="pun">}</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>messages：自定义的提示信息，key:value 的形式，key 是要验证的元素，value 可以是字符串或函数。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
	rules</span><span class="pun">:{</span><span class="pln">
		name</span><span class="pun">:</span><span class="str">"required"</span><span class="pun">,</span><span class="pln">
		email</span><span class="pun">:{</span><span class="pln">
			required</span><span class="pun">:</span><span class="kwd">true</span><span class="pun">,</span><span class="pln">
			email</span><span class="pun">:</span><span class="kwd">true</span><span class="pln">
		</span><span class="pun">}</span><span class="pln">
	</span><span class="pun">},</span><span class="pln">
	messages</span><span class="pun">:{</span><span class="pln">
		name</span><span class="pun">:</span><span class="str">"Name不能为空"</span><span class="pun">,</span><span class="pln">
		email</span><span class="pun">:{</span><span class="pln">       
			required</span><span class="pun">:</span><span class="str">"E-mail不能为空"</span><span class="pun">,</span><span class="pln">
			email</span><span class="pun">:</span><span class="str">"E-mail地址不正确"</span><span class="pln">
		</span><span class="pun">}</span><span class="pln">
	</span><span class="pun">}</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>

<tr>
	<td>groups：对一组元素的验证，用一个错误提示，用 errorPlacement 控制把出错信息放在哪里。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">"#myform"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
	groups</span><span class="pun">:{</span><span class="pln">
		username</span><span class="pun">:</span><span class="str">"fname 
		lname"</span><span class="pln">
	</span><span class="pun">},</span><span class="pln">
	errorPlacement</span><span class="pun">:</span><span class="kwd">function</span><span class="pun">(</span><span class="pln">error</span><span class="pun">,</span><span class="pln">element</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
		</span><span class="kwd">if</span><span class="pln"> </span><span class="pun">(</span><span class="pln">element</span><span class="pun">.</span><span class="pln">attr</span><span class="pun">(</span><span class="str">"name"</span><span class="pun">)</span><span class="pln"> </span><span class="pun">==</span><span class="pln"> </span><span class="str">"fname"</span><span class="pln"> </span><span class="pun">||</span><span class="pln"> element</span><span class="pun">.</span><span class="pln">attr</span><span class="pun">(</span><span class="str">"name"</span><span class="pun">)</span><span class="pln"> </span><span class="pun">==</span><span class="pln"> </span><span class="str">"lname"</span><span class="pun">)</span><span class="pln">   
			error</span><span class="pun">.</span><span class="pln">insertAfter</span><span class="pun">(</span><span class="str">"#lastname"</span><span class="pun">);</span><span class="pln">
		</span><span class="kwd">else</span><span class="pln">    
			error</span><span class="pun">.</span><span class="pln">insertAfter</span><span class="pun">(</span><span class="pln">element</span><span class="pun">);</span><span class="pln">
	</span><span class="pun">},</span><span class="pln">
   debug</span><span class="pun">:</span><span class="kwd">true</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>Onubmit：类型 Boolean，默认 true，指定是否提交时验证。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">  
	onsubmit</span><span class="pun">:</span><span class="kwd">false</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>onfocusout：类型 Boolean，默认 true，指定是否在获取焦点时验证。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">   
	onfocusout</span><span class="pun">:</span><span class="kwd">false</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>onkeyup：类型 Boolean，默认 true，指定是否在敲击键盘时验证。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
   onkeyup</span><span class="pun">:</span><span class="kwd">false</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>onclick：类型 Boolean，默认 true，指定是否在鼠标点击时验证（一般验证 checkbox、radiobox）。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
   onclick</span><span class="pun">:</span><span class="kwd">false</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>focusInvalid：类型 Boolean，默认 true。提交表单后，未通过验证的表单（第一个或提交之前获得焦点的未通过验证的表单）会获得焦点。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
   focusInvalid</span><span class="pun">:</span><span class="kwd">false</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>focusCleanup：类型 Boolean，默认 false。当未通过验证的元素获得焦点时，移除错误提示（避免和 focusInvalid 一起使用）。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
   focusCleanup</span><span class="pun">:</span><span class="kwd">true</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>errorClass：类型 String，默认 "error"。指定错误提示的 css 类名，可以自定义错误提示的样式。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln"> 
	errorClass</span><span class="pun">:</span><span class="str">"invalid"</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>errorElement：类型 String，默认 "label"。指定使用什么标签标记错误。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate
   errorElement</span><span class="pun">:</span><span class="str">"em"</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>wrapper：类型 String，指定使用什么标签再把上边的 errorELement 包起来。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">
   wrapper</span><span class="pun">:</span><span class="str">"li"</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>errorLabelContainer：类型 Selector，把错误信息统一放在一个容器里面。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">"#myform"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">   
	errorLabelContainer</span><span class="pun">:</span><span class="str">"#messageBox"</span><span class="pun">,</span><span class="pln">
	wrapper</span><span class="pun">:</span><span class="str">"li"</span><span class="pun">,</span><span class="pln">
	submitHandler</span><span class="pun">:</span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln"> 
		alert</span><span class="pun">(</span><span class="str">"Submitted!"</span><span class="pun">)</span><span class="pln"> 
	</span><span class="pun">}</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>showErrors：跟一个函数，可以显示总共有多少个未通过验证的元素。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">".selector"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">  
	showErrors</span><span class="pun">:</span><span class="kwd">function</span><span class="pun">(</span><span class="pln">errorMap</span><span class="pun">,</span><span class="pln">errorList</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">
        $</span><span class="pun">(</span><span class="str">"#summary"</span><span class="pun">).</span><span class="pln">html</span><span class="pun">(</span><span class="str">"Your form contains "</span><span class="pln"> </span><span class="pun">+</span><span class="pln"> </span><span class="kwd">this</span><span class="pun">.</span><span class="pln">numberOfInvalids</span><span class="pun">()</span><span class="pln"> </span><span class="pun">+</span><span class="pln"> </span><span class="str">" errors,see details below."</span><span class="pun">);</span><span class="pln">
		</span><span class="kwd">this</span><span class="pun">.</span><span class="pln">defaultShowErrors</span><span class="pun">();</span><span class="pln">
	</span><span class="pun">}</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>errorPlacement：跟一个函数，可以自定义错误放到哪里。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">"#myform"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">  
	errorPlacement</span><span class="pun">:</span><span class="kwd">function</span><span class="pun">(</span><span class="pln">error</span><span class="pun">,</span><span class="pln">element</span><span class="pun">)</span><span class="pln"> </span><span class="pun">{</span><span class="pln">  
		error</span><span class="pun">.</span><span class="pln">appendTo</span><span class="pun">(</span><span class="pln">element</span><span class="pun">.</span><span class="pln">parent</span><span class="pun">(</span><span class="str">"td"</span><span class="pun">).</span><span class="kwd">next</span><span class="pun">(</span><span class="str">"td"</span><span class="pun">));</span><span class="pln">
   </span><span class="pun">},</span><span class="pln">
   debug</span><span class="pun">:</span><span class="kwd">true</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>success：要验证的元素通过验证后的动作，如果跟一个字符串，会当作一个 css 类，也可跟一个函数。</td>
    <td>
<pre style="white-space: pre-wrap;" class="prettyprint prettyprinted"><span class="pln">$</span><span class="pun">(</span><span class="str">"#myform"</span><span class="pun">).</span><span class="pln">validate</span><span class="pun">({</span><span class="pln">        
	success</span><span class="pun">:</span><span class="str">"valid"</span><span class="pun">,</span><span class="pln">
        submitHandler</span><span class="pun">:</span><span class="kwd">function</span><span class="pun">()</span><span class="pln"> </span><span class="pun">{</span><span class="pln"> 
			alert</span><span class="pun">(</span><span class="str">"Submitted!"</span><span class="pun">)</span><span class="pln"> 
		</span><span class="pun">}</span><span class="pln">
</span><span class="pun">})</span></pre>
	</td>
</tr>
<tr>
	<td>highlight：可以给未通过验证的元素加效果、闪烁等。</td>
    <td></td>
</tr>
</tbody></table>
#####addMethod(name,method,message)方法
参数 name 是添加的方法的名字。
参数 method 是一个函数，接收三个参数 (value,element,param) 。
value 是元素的值，element 是元素本身，param 是参数。
我们可以用 addMethod 来添加除内置的 Validation 方法之外的验证方法。比如有一个字段，只能输一个字母，范围是 a-f，写法如下：
```javascript
$.validator.addMethod("af",function(value,element,params){  
	if(value.length>1){
		return false;
	}
    if(value>=params[0] && value<=params[1]){
		return true;
	}else{
		return false;
	}
},"必须是一个字母,且a-f");
```
如果有个表单字段的 id="username"，则在 rules 中写：
```javascript
username:{
   af:["a","f"]
}
```
addMethod 的第一个参数，是添加的验证方法的名字，这时是 af。
addMethod 的第三个参数，是自定义的错误提示，这里的提示为:"必须是一个字母,且a-f"。
addMethod 的第二个参数，是一个函数，这个比较重要，决定了用这个验证方法时的写法。
如果只有一个参数，直接写，比如 af:"a"，那么 a 就是这个唯一的参数，如果多个参数，则写在 [] 里，用逗号分开。
#####meta String 方式
```javascript
$("#myform").validate({
   meta:"validate",
   submitHandler:function() { 
alert("Submitted!") }
});
```
```html
<script type="text/javascript" 
src="js/jquery.metadata.js"></script>
<script type="text/javascript" 
src="js/jquery.validate.js"></script>

<form id="myform">
  <input type="text" 
name="email" class="{validate:{ required:true,email:true }}">
  <input type="submit" 
value="Submit">
</form>
```
