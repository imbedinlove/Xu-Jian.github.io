// tag 排序 加载时候的默认值...



$(function tagDeafultFilter(){
	var tagDeafult = $("#ymlTagSortDefault").text();
    //alert( tagDeafult );

    if      ( tagDeafult == "abc" )  { tagAbcfilter();  }
    else if ( tagDeafult == "num" )  { tagNumfilter();  }
    else                            { tagTimefilter(); }

});








	<script>

	  var posts = [
		{% for post in site.posts %}
		  {
			"title": "{{ post.title }}",
			"categories": "{{ post.categories }}",
			"url": "{{ post.url }}"
		  }
		  {% unless forloop.last %},{% endunless %}
		{% endfor %}
	  ]; 

	console.log( posts );
	</script>








// open links(do not need pjax) in new tab.
// 正文 也就是 所有的post 会有各种链接. 这里需要过滤.
//	var afterPjax = function() {
//		$('#contentDivDiv').find('a').filter(function() { return this.hostname != window.location.hostname; }).attr('target', '_blank');
//	};
//	afterPjax();
// 这才是最关键的 . 特别是 后面那个 fragment 参数...



	

	



/*





	// 定义变量???  来获取 html里面的数据.			
	var catt =  $(".catt").data('catefi');
	 // console.log("catt: "+catt);

	var catefilter = $(".cateLI").data('catefilter');
	  // console.log("catefilter: "+catefilter);

	var filenameDiv = $('#filenameDiv')


	// 最外层div开始. 过滤所有的 链接 a .. 主要的是 filenameDiv下的链接. 
	// 如果 这些链接不 包含 本博客 链接 前面的固定部分. 那么就把这些链接设置成 在blank 在新窗口打开.
	var afterPjax = function() {
			$('#bigDiv').find('a').filter( function(){
					return this.hostname != window.location.hostname;	
				}).attr('target','_blank');
		};

	afterPjax();

	// 这个是 进度条
	$(document).on({
		'pjax:click': function(){  	NProgress.start();  },
		'pjax:end':   function(){   NProgress.done();   }
	  });

	// 这个是 pjax 主要部分..  那个区域的 ....
	$(document).pjax("#filenameDiv", '#contentDiv', 
		{ fragment: '#contentDiv', timeout:3000}
	  );	

	
*/

////////////////////////////////////////////////////////////////////////////////////////////////////////////////	




// 点击大类 对 filename 进行flex 自定义排序.这个函数 ...   大体应该和 filenameDiv 默认时候的自定义排序差不多的..
function cateFilenameFlex() {
	$ (".blog-list-container4").each(function( index ) {
		   // console.log( index + ": " + $( this ).text()   );               
		   // console.log( index + ": " + $( this ).children("span").text() + $( this ).children("span").text().length )
		   // 上面 获取整个元素的内容.  下面获取子元素span. 
			var orderValue = $( this ).find('span').text();
			var orderLength = $( this ).find('span').text().length;
			if ( orderLength != "0" ) {
				//console.log( orderValue );
				//console.log( $(this ).css("order") );
				$(this ).css("order", orderValue); 
					//console.log( $(this ).css("order") );
				}
		});

}



<ul id="cateDivFlex" class="CLcateDivFlex" style="overflow: scroll;" >	
{% for someCate in site.categories %}
    {% assign cateClicked = someCate | first %}    
    {% assign catePosts = someCate | last %}
    <!-- 下面的class/id 是给大类排序用的 -->
        <li style="order: 100; padding-bottom:5px;"; class="cate-status" id="{{cateClicked}}" style="width: 90%, background-color:red;";> 
           <a href onclick="catefilter('{{ cateClicked }}'); return false;" >			
            {{ cateClicked }}<sup>{{catePosts | size }}</sup>
           </a>
        </li>
    
{% endfor %}
</ul>





<!-- ----------------------------------------------------------------------------------------------- -->
<!-- 	filenameDiv1 显示所有文章 默认的时间顺序 + flex自定义排序 ✔︎ -->
	<div class="blog-list-container" id="all-container" ; style="overflow: scroll;">
		<ul class="blog-list" ; id="filenameDivFlex" style="display: flex ; flex-direction: column">
		{% for post in site.posts reversed %}
			<li style="order: 219"  class="fileNameFlexOrder" id="{{post.title}}";  >
			  <a href="{{ post.url }}" 
			  	 onclick="filenameActiveCT('{{post.tags}}', '{{post.categories}}'); filenameActiveThis(); catefilterPart('{{ post.categories }}');" >
	         	 <i class="fa fa-angle-double-right" aria-hidden="true"></i>			  
			 	 {{ post.title }} 
			  <sup class="fileNameDate hidden">{{ post.date | date: "%Y-%m-%d" }}</sup></a>
			</li>
		{% endfor %}
		</ul>
	</div>

<!--   -->
<!-- ----------------------------------------------------------------------------------------------- -->

<!-- filenameDiv2  某标签下的文章-->
	{% for tag in site.tags %}
		{% assign t = tag | first %}      <!--   这是第一个 标签 -->
		{% assign posts = tag | last %}   <!--   这是 所有文章 -->	
	<div class="blog-list-container2 hidden" id="{{ t }}-container">
	  <ul class="blog-list" id="overflowHeighr2" style="display: flex ; flex-direction: column; overflow: scroll;" >
	    {% for post in posts reversed %}
	      {% if post.tags contains t %}
	          <li >
	            <a href="{{ post.url }}" onclick="filenameActiveCT('{{post.tags}}', '{{post.categories}}'); filenameActiveThis();"
	            style="order: 219" class="blog-list-container22"  id="{{ post.title }}-tag" >
	            <i class="fa fa-angle-double-right" aria-hidden="true"></i>
	          	{{ post.title }}
	          <sup class="fileNameDate hidden">{{ post.date | date: "%Y-%m-%d"  }}</sup></a>
	        </li>
	      {% endif %}
	    {% endfor %}
	  </ul>
	</div>
	{% endfor %}

<!-- ----------------------------------------------------------------------------------------------- -->
<!-- filenameDiv3: 	某大类下的所有文章. -->
{% for CatePosts in site.categories %}
	{% assign CateName = CatePosts | first %} 
	{% assign Cateposts = CatePosts | last %}
	<div class="blog-list-container3 hidden" id="{{CateName}}-filenames" style="display: flex ; flex-direction: column; overflow: scroll;" >	
		{% for zz in Cateposts  reversed %}
	    	{% if zz.categories contains CateName %}
					<li style="order: 219"; class="blog-list-container4" id="{{zz.title}}-filename" >
			    	  <a href="{{zz.url}}" onclick="filenameActiveCT('{{zz.tags}}', '{{zz.categories}}'); filenameActiveThis();">
						<i class="fa fa-angle-double-right" aria-hidden="true"></i>
							{{zz.title}}
						<sup class="fileNameDate hidden">{{ zz.date | date: "%Y-%m-%d" }}</sup></a>
					</li>
	      {% endif %}
	  {% endfor %}
	</div>  	
{% endfor %}	









/*

// 这个函数 传入的变量是 cateDiv 里面的 点击某个分类时候的 分类名:   cateClicked
function catefilter(cateClicked) { showTagsandPosts(cateClicked); delRows(); setActiveCate(cateClicked); }
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//  选取遍历 catediv 下的 所有li  然后给选定的大类 添加active  点击大类也要清楚 tagDiv 下的所有active....
function setActiveCate(cateClicked) {
	// 清除所有大类高亮+设置某个大类
	$("#cateDiv li").each( function() {  	$(this).removeClass('active');        });
	$("#"+ cateClicked).addClass('active');
	$("#"+ cateClicked +"-num").addClass('active');
	// console.log( $("#"+ cateClicked) )
	// 下面是清楚 tagDiv 的active 
	$("#tagDiv li").each( function() {  	$(this).removeClass('active');  	});
	// 下面是清除 filename 的高亮
	$("#filenameDiv li").each( function() {  	$(this).removeClass('active');  	});
}
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 下面是 点击大类 过滤出对应的tag 和 filename..  这里  也要进行yml 默认tag 排序的判断
function showTagsandPosts(cateClicked) {
	$(".tagsDiv1-allTags").attr("class","tagsDiv1-allTags hidden");       //$(".tagsDiv1-allTags").css("display","none");
	$(".tagDiv2-catetags").attr("class","tagDiv2-catetags hidden");
	$("#"+ cateClicked +"-cate").attr("class","tagDiv2-catetags");         // 把这个元素的class 改成 ... 没有hidden的.
	// 下面是 filenameDiv 的 
	$(".blog-list-container").attr("class","blog-list-container hidden");
	$(".blog-list-container2").attr("class","blog-list-container2 hidden");
	$(".blog-list-container3").attr("class","blog-list-container3 hidden");
	$("#"+ cateClicked +"-filenames").attr("class","blog-list-container3");   		
}	
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 根据元素的 内容长度 来删除 空行. 这里要用 正则式 进行筛选. 而不是长度. 正则式要筛选出来 而不是删除...
// function trimStr(str){return str.replace(/(^\s*)|(\s*$)/g,"");}
function delRows() {
		$(".delEmptyrow-C").each(  function(index){
			var thisALLText = $(this).text();
			var thisSpanText = $(this).find("span").text();
			var textSpanLength =　 $(this).find("span").text().length ;
			//console.log(thisSpanText+"标签长度"+textSpanLength);
			if ( $(this).find("span").text().length == 0 )	{	$(this).attr("class","delEmptyrow-C hidden");	}
		});
	}

// 上面是 cate 过滤
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 下面是 tag 过滤

function filter(tag) {
  setActiveTag(tag);
  showfilenameDiv2(tag);
  tagFilenameFlex();
}
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
function showfilenameDiv2(tag){	
	if( tag == "allPosts") {
			// 显示 div1 就可以了... 隐藏div2 div3
			$(".blog-list-container").attr("class","blog-list-container");
			$(".blog-list-container2").attr("class","blog-list-container2 hidden");	
			$(".blog-list-container3").attr("class","blog-list-container3 hidden");	
			$("#cateDiv li").each( function() {  	$(this).removeClass('active');        }); // 移除 cateDiv    下所有的 active...
		}
	else{
			// 先隐藏 div1 ,这个div是要一直隐藏的.
			$(".blog-list-container").attr("class","blog-list-container hidden");
			// 再隐藏 div2 全部 .这里有 每个标签下的各自的文章.只需要显示某个标签就行
			$(".blog-list-container2").attr("class","blog-list-container2 hidden");
			// 这里 也要排错 div3... 不然你点过大类下某个标签.这里会残留
			$(".blog-list-container3").attr("class","blog-list-container3 hidden");
			// 最后显示 div2 下符合条件的id . 
			$("#"+ tag + "-container").attr("class","blog-list-container2");
		}
}
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//  选取遍历 tagdiv 下的 所有li  然后给选定的大类 添加active. tag 有好几个的.
function setActiveTag(tag) {
	// 显示所有标签是时候 要去除cate + tag + filename的 所有高亮
	$("#tagDiv li").each( function() {  	$(this).removeClass('active'); 	});       // 移除tagDiv      下所有的 active... 
	$("#filenameDiv li").each( function() {  	$(this).removeClass('active'); 	});   // 移除filenameDiv 下所有的 active... 
	$("#"+ tag +"-item").addClass('active');    // 默认情况 排序1 下的 active
	$("#"+ tag +"-item2").addClass('active');   // 默认情况 排序1 下的 active
	$("#"+ tag +"-item3").addClass('active');   // 默认情况 排序1 下的 active
	$("#"+ tag +"-cate22").addClass('active');   //  大类过滤出来的 active
}
// 这里只解决了默认情况下的 active...  大类下 还是不行??
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
function tagFilenameFlex() {
	$ (".blog-list-container22").each(function( index ) {
	   // console.log( index + ": " + $( this ).text()   );               
	   // console.log( index + ": " + $( this ).children("span").text() + $( this ).children("span").text().length )
	   // 上面 获取整个元素的内容.  下面获取子元素span. 
		var orderValue = $( this ).children('span').text();
		var orderLength = $( this ).children('span').text().length;
		if ( orderLength != "0" ) {
			//console.log( orderValue );
			//console.log( $(this ).css("order") );
			$(this ).css("order", orderValue); 
				//console.log( $(this ).css("order") );
			}
	});
}


////////////////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 下面是 file过滤


////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 显隐 文件时间
function fileNameDate() {
  // console.log( $(".fileNameCustonOrder").css("display")   );
  // 判断 某元素显示隐藏状态. 显示:block  隐藏:none
  var fileNameSortNumStatus = $(".fileNameDate").css("display");
    if ( fileNameSortNumStatus ==  "none" ) {
         $("#filenameDivDate").removeClass("fa fa-eye-slash")  ;
         $("#filenameDivDate").addClass("fa fa-eye");
         $(".fileNameDate").removeClass("hidden");
       } else {
         $("#filenameDivDate").removeClass("fa fa-eye")  ;
         $("#filenameDivDate").addClass("fa fa-eye-slash");
         $(".fileNameDate").addClass("hidden");
       }
}
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 点击某个文件名 高亮: 文件名+ 对应tag + 对应cate ....用 onclick 传入文件名 ...然后取消所有active 然后设置某个active...
function filenameActiveCT( afileTag,afileCate ) {
	setActiveCate(afileCate);   // 必须先设置大类高亮. 因为大类高亮会清除所有的 tag/filename 高亮. 在cateFilter 里.
	setActiveTag(afileTag);     // 第二个执行   函数在tagfilter里
}

function filenameActiveThis () {
	var selector = '#filenameDiv li';
	$(selector).on('click', function(){
		$(selector).removeClass('active');
		$(this).addClass('active');
	});
}
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 点击文件名. 显示出该文件名类下的所属 tag.. 类似点击某大类.. 这里只需要显示改大类下的标签就可以. 不用显示该大类的 文章....
function catefilterPart( afilename ) {
	$(".tagsDiv1-allTags").attr("class","tagsDiv1-allTags hidden");
	//$(".tagsDiv1-allTags").css("display","none");
	$(".tagDiv2-catetags").attr("class","tagDiv2-catetags hidden");
	$("#"+ afilename +"-cate").attr("class","tagDiv2-catetags"); 
	// 把这个元素的class 改成 ... 没有hidden的.
		
delRows();  //这个是 cateFilter.js 下的 删除空行的 函数..

}

////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 显示所有标签和文章
function showAllTagsandPosts () {
	$(".tagsDiv1-allTags").attr("class","tagsDiv1-allTags");               //  默认所有标签时间排序
	$(".tagDiv2-catetags").attr("class","tagDiv2-catetags hidden"); 	   //  大类过滤出来的 tag
	// 下面是 filenameDiv的if过滤 
	$(".blog-list-container").attr("class","blog-list-container");
	$(".blog-list-container2").attr("class","blog-list-container2 hidden"); 
	$(".blog-list-container3").attr("class","blog-list-container3 hidden"); 
	// 显示所有标签是时候 要去除cate + tag + filename的 所有高亮
	$("#cateDiv li").each( function()     { $(this).removeClass('active'); });
	$("#tagDiv li").each( function()      { $(this).removeClass('active'); });
	$("#filenameDiv li").each( function() { $(this).removeClass('active'); });
}







*/





// 这个文件是给排序用的.  cateDiv + tagDiv + filenameDiv 名称排序.
// cateDiv 大类文件数量排序: 也就是隐藏 默认的 custom 排序div 显示num排序div
  function cateNumfilter() {
    $("#cateDivFlex").attr("class","CLcateDivFlex hidden");
    $(".CLcateDiv1-numberSort").attr("class","CLcateDiv1-numberSort");
  }



////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 下面是 cateDiv 下的 custom 自定义排序.  和onready.js 几乎一样...
function cateCustomfilter(){		
  //console.log( $("#ymlCateName").text() );
  var xx= $("#ymlCateName").text();
  //console.log(xx);
  // 成功取到 cateDiv 下的 yml 里的自定义数据(字符串...)   → Web-1,Misc-2,-3
  var yy = xx.split(",")
  // console.log(yy);
  // 用split 把字符串 通过指定的分割符号 变为 数组  → ["Web-1", "Misc-2", "-3"]
  //console.log(yy.length);   // 取得数组的长度,用于循环
  for (i=0; i<yy.length; i++) {
           // alert(yy[0]); 
           // 这个会显示出 第一个数据  Web-1.
          // 把这个数据再次分割. 用 - 作为分隔符
          var temp = yy[i].split('^');
              //alert(temp[0]); 
              //alert(temp[1]);
              var tempName = temp[0];
              var tempOrder = temp[1];

          $("#"+ tempName ).css("order",tempOrder); 

          // 然后这个 Web就可以拿来 作为 cateDiv 里面对应的id了. 把这个id 的 flex order 改成 数字.
          // 这里顺序就修改成功了....
          // 下面就是怎么实现 一载入就 修改顺序....
      }

    $(".CLcateDivFlex").attr("class","C.cateDivFlex");
    // 处理加载主页时候. 会闪的问题...  先隐藏掉默认排序... 等js排序好之后再显示就可以了..
    $(".CLcateDiv1-numberSort").attr("class","CLcateDiv1-numberSort hidden");
    // 隐藏掉 num排序的div
}



  
////////////////////////////////////////////////////////////////////////////////////////////////////////////////
// 下面是 tagDiv 的排序..
// 字母排序: →     隐藏 time + num div... ; 显示abcdiv   这里还要隐藏 tag 过滤出来的那个div
function tagAbcfilter(){
    $(".tagDiv2-catetags").attr("class","tagDiv2-catetags hidden");        //隐藏 tag过滤出来的某类下的标签 
    $("#alltagsContainer").attr("class","tagsDiv1-allTags hidden");        //隐藏 time div
    $("#tagDiv1-numberSort").attr("class","CLtagDiv1-numberSort hidden");  //隐藏  num div
    $("#tagDiv1-letterSort").attr("class","tagDiv1-letterSort");           //显示  abc div
}

// Time 排序: → 隐藏abc + num ; 显示time
function tagTimefilter(){
    $(".tagDiv2-catetags").attr("class","tagDiv2-catetags hidden");        //隐藏 tag过滤出来的某类下的标签   
    $("#tagDiv1-numberSort").attr("class","CLtagDiv1-numberSort hidden");  //隐藏  num div
    $("#tagDiv1-letterSort").attr("class","tagDiv1-letterSort hidden");    //隐藏  abc div
    $("#alltagsContainer").attr("class","tagsDiv1-allTags");               //显示 time div    
}

// 数量排序: → 隐藏 abc + time ; 显示num 
function tagNumfilter(){
    $(".tagDiv2-catetags").attr("class","tagDiv2-catetags hidden");        //隐藏 tag过滤出来的某类下的标签   
    $("#alltagsContainer").attr("class","tagsDiv1-allTags hidden");        //隐藏 time div    
    $("#tagDiv1-letterSort").attr("class","tagDiv1-letterSort hidden");    //隐藏  abc div
    $("#tagDiv1-numberSort").attr("class","CLtagDiv1-numberSort");         //显示  num div
}







  <style>
        #forkongithub a{background:#111;color:#fff;
        text-decoration:none;font-family:arial,sans-serif;text-align:center;
        font-weight:bold;padding:5px 40px;font-size:0.5rem;line-height:1rem;
        position:relative;transition:0.5s;}

        #forkongithub a:hover{background:#c11;color:#fff;}

        #forkongithub a::before,#forkongithub a::after{content:"";width:100%;display:block;position:absolute;top:1px;left:0;height:1px;background:#fff;}
        #forkongithub a::after{bottom:1px;top:auto;}

        @media screen and (min-width:80px){
            #forkongithub{position:fixed;display:block;top:0;right:0;width:150px;overflow:hidden;height:150px;z-index:9999;}
            #forkongithub a{width:66px;position:absolute;top:30px;right:-30px;
                transform:rotate(45deg);-webkit-transform:rotate(45deg);-ms-transform:rotate(45deg);-moz-transform:rotate(45deg);-o-transform:rotate(45deg);
                box-shadow:4px 4px 10px rgba(0,0,0,0.8);}}
    </style>
    <span id="forkongithub"><a href="https://github.com/Xu-Jian/Xu-Jian.github.io" target="'_blank">Fork me</a></span>



#slide {
    position: absolute;
    left: -100px;
    width: 100px;
    height: 100px;
    background: blue;
    -webkit-animation: slide 0.5s forwards;
    -webkit-animation-delay: 2s;
    animation: slide 0.5s forwards;
    animation-delay: 2s;
}

@-webkit-keyframes slide {
    100% { left: 0; }
}

@keyframes slide {
    100% { left: 0; }
}



