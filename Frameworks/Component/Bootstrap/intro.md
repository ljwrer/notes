
<div class="code-desc co">
              <p align="left">在制作Web页面时，前端人员都习惯为网站设置一个<span style="color:#B22222;"><strong>全局样式（reset.css）</strong></span>。那么在Bootstrap框架中也不例外。Bootstrap框架的核心是轻量的CSS基础代码库，他并没有一味的重置样式，而是注重各浏览器基础表现，降低开发难度。大部分前端人员都具有归零的思想，但实际你会发现，归零之后的样式在开发过程中会存在着潜在的问题，例如，在全局样式表中将<span style="color:#B22222;"><strong>em</strong></span>变成一个普通标记，明明应该是斜体，怎么就没效果了呢？</p>

<p align="left">Bootstrap框架在这一部分做了一定的变更，不再一味追求归零，而是更注重重置可能产生问题的样式（如，body,form的margin等），保留和坚持部分浏览器的基础样式，解决部分潜在的问题，提升一些细节的体验，具体说明如下：</p>

<ul style="list-style-type:circle;margin-left:30px;">
	<li align="left"><strong>移除body的margin声明</strong></li>
	<li align="left"><strong>设置body的背景色为白色</strong></li>
	<li align="left"><strong>为排版设置了基本的字体、字号和行高</strong></li>
	<li align="left"><strong>设置全局链接颜色，且当链接处于悬浮“:hover”状态时才会显示下划线样式</strong></li>
</ul>

<p align="left"><strong>小伙伴们，你们可以单击查看右侧“<span style="color:#B22222;">style.css</span>”文件，来查看上面所说的全局样式（这个是从下载的boostrap.css中摘取出来的）。</strong></p>

<p align="left"><strong><a href="http://img.mukewang.com/5410fd59000150f607950426.jpg"><img alt="" src="http://img.mukewang.com/5410fd59000150f607950426.jpg" style="width: 350px;"></a></strong></p>

<p align="left">其实Bootstrap的全局样式采用了<span style="color:#000000;">“<strong>normalize.css</strong>”</span>，但并没有一味的全部使用此重置样式，而是在此基础上进行了一些改良，让其更适合Bootstrap的设计思想。</p>

<p>你可以通过“normalize.less”（LESS版本）或“_normalize.scss”（Sass版本）进行深入的了解。</p>

<p>&nbsp;</p>
</div>