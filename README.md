# canvas
简单实现通过鼠标点击生成一个canvas图形，当然可以选择起始点的x,y轴数据，使绘图区域的位置发生变化。
比如btn_map.html,点击开始绘制，通过鼠标点击事件，用数组jsonarray来存放每个点击数据的x,y,然后进行重绘drawPath()。

      function drawPath(){
	         	$("#map-area").children("div.cir").each(function(){
	         		$(this).remove();
	         	});
			    var canvas=document.getElementById("myCanvas");
			    var ctx=canvas.getContext("2d");
			    ctx.beginPath();
			    ctx.moveTo(arrX[0],arrY[0]);
			    var flag;
			    
			    arr={
                       "x" :arrX[0],
                       "y" :arrY[0]
                      }
			    jsonarray.push(arr);
			    
			    for(flag=1;flag<arrX.length;flag++)
			    {
			    	ctx.lineTo(arrX[flag],arrY[flag]);
			    	//alert(arrX[flag]+"--"+arrY[flag]);
			    	arr={
                       "x" :arrX[flag],
                       "y" :arrY[flag]
                      }
			    	jsonarray.push(arr);
                   
			    }
			    //ctx.fillStyle="#FF0000";//填充颜色
			    ctx.closePath();//关闭路径
			    ctx.fill();//填充路径
			    //ctx.stroke();//边框
			    
			
		    }  
        
重点是 **test_hover.html**
-
- 实现svg的hover变色 (并不能像一般DOM操作添加style)
- 四种边框超界
  + 上边框出界
  + 下边框出界
  + 左边框出界
  + 右边框出界
 
 **hover 事件**
     
     vInit();
     var svgLeft=$("svg").offset().left;//svg的左边距
     var svgTop=$("svg").offset().top;//svg的上边距
     var svgWidth=$("svg").outerWidth();//svg的宽度
     var svgHeight=$("svg").outerHeight();//svg的高度
     $(this).attr({"fill":getFillColor,"stroke":getStrokeColor});//填充 颜色和修改边框
    			
    			  
 **边框出界问题**
      
     function edgeCal(iTop,iLeft,svgLeft,svgTop,svgWidth,svgHeight,offsetTop,offsetLeft,oHeight,oWidth,dataWidth,dataHeight){
    			divTop=iTop;
    			divLeft=iLeft;
    			if(svgTop>iTop){//上边框出界
    				if(svgLeft>iLeft){//左边框出界
    					divTop=offsetTop+oHeight;
    					divLeft=offsetLeft+oWidth;
    				}else{
    					if((iLeft+dataWidth)>(svgLeft+svgWidth)){
    						divTop=offsetTop+oHeight;
    						divLeft=offsetLeft-dataWidth;
    					}else{
    						divTop=iTop+dataHeight+oHeight;
    					}
    				}
    			}else{
    				if(svgLeft>iLeft){//左边框出界
    					divLeft=offsetLeft+oWidth;
    					divTop=offsetTop-(dataHeight-oHeight)/2;
    					if((divTop+dataHeight)>(svgTop+svgHeight)){
    						divTop=offsetTop-dataHeight;
    					}
    				}else{
    					if((iLeft+dataWidth)>(svgLeft+svgWidth)){//右边框出界
    						divLeft=offsetLeft-dataWidth;
    						divTop=offsetTop-(dataHeight-oHeight)/2;
    						if((divTop+dataHeight)>(svgTop+svgHeight)){
    							divTop=offsetTop-dataHeight;
    						}
    					}
    				}
    			}
    		}   
        
#室内平面图#
/setPosition/用svg-editor 实现添加落位

![室内图](https://github.com/ivybaby/canvas/blob/master/setPosition/HC8DT%7DE~3LGRJ%5D~%5DXOHU5JT.jpg)

本文就展示最重要的代码，添加落位，也就是添加楼层单元信息

     //添加一个落位
    function SaveDicrection(unitid) {
        //选择unit时设置id,未选时保存页面上的已有元素(用于删除功能)
        if (unitid != null && unitid != "" && unitid.length > 35) {
            //获取选中的SVG
             
            var doc = document.getElementById('svgedit').contentDocument;//用于取子ID
            var svgSelect = doc.getElementById('elem_id');
            if (svgSelect != null) {
                var svg = doc.getElementById(svgSelect.value);
                svg.setAttribute('id', unitid.replace(/\,/g, '—'));//一次选择多个资源 所有,换成+
                ///alert(unitid.replace(/\,/g, '—'));
                var svgCanvas = new embedded_svg_edit(document.getElementById('svgedit'));
                svgCanvas.getSvgString()(handleSvgData);
            }
        } else {
            alert('请选择预添加的单元！');
            return;
        }
    }
   
保存落位


    //保存所有落位
    function saveSvg() {
        var svgCanvas = new embedded_svg_edit(document.getElementById('svgedit'));
        svgCanvas.getSvgString()(handleSvgData);
    }
    
    
加载落位

    var width = document.getElementById('svgWidthHeight').getAttribute('width');
                    var height = document.getElementById('svgWidthHeight').getAttribute('height');

                    
                    //设置元素
                    //匹配:<svg width="xxx" height="xxx" 
                    var svgElementReg = /(\s*\<\s*svg\s*width\s*\=\s*)(\")(\d+)(\")(\s*)(height)(\s*\=\s*\")(\d+)(\")/;
                    var group = data.Data.SVGElement.match(svgElementReg);
                    var svgWHstr = group[0];
                    var imgWHstr = group[0].replace(group[3], width).replace(group[8], height);
                    var elements = data.Data.SVGElement.replace(svgWHstr, imgWHstr);

                    //console.log(elements);
                    
                    var svgCanvas = new embedded_svg_edit(document.getElementById('svgedit'));
                    svgCanvas.setSvgString(elements);

由于原始完整的html文件遗失，所以部分.net语言展现，如果所需，自己提取。

具体的请看：https://github.com/ivybaby/canvas/tree/master/2-canvas

转载请注明出处：https://github.com/ivybaby/canvas/tree/master/2-canvas
        
