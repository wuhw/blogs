#css-gradient

主要有四种类型的渐变：linear-gradient,repeating-linear-gradient,radial-gradient,repeating-radial-gradient.

不同的浏览器有不同的语法，与css3标准语法也不相同

这里不描述超老版本的webkit的语法实现。  
各浏览器支持的新版语法只是前缀不同而已，各个浏览器的前缀分别是：-moz-,-webkit-,-o-,-ms-，以下一webkit来举例。重复渐变语法与非重复的语法相同，下面也不列举重复渐变的语法。

<table>
    <tr>
        <th>渐变类型</th>
        <th>各浏览器的新版语法(只是前缀不同)</th>
        <th>css3标准语法</th>
    </tr>
    <tr>
        <td>linear-gradient</td>
        <td>-webkit-linear-gradent([&lt;point&gt;||&lt;angle&gt;,]?&lt;stop&gt;,&lt;stop&gt;[,&lt;stop&gt;]*)</td>
        <td>linear-gradient([[&lt;angle&gt; | to &lt;side-or-corner&gt; ],]?&lt;color-stop&gt;[,&lt;color-stop&gt;]+)</td>
    </tr>
    <tr>
        <td>radial-gradient</td>
        <td>-webkit-radial-gradient([&lt;position&gt; || &lt;angle&gt;,]?[&lt;shape&gt; || &lt;size&gt;,]?&lt;color-stop&gt;,&lt;color-stop&gt;[,&lt;color-stop&gt;]*)</td>
        <td>radial-gradient([[&lt;shape&gt; || &lt;size&gt;] [at &lt;position&gt;]?,| at &lt;position&gt;,]?&lt;color-stop&gt;[,&lt;color-stop&gt;]+)</td>
    </tr>
</table>



