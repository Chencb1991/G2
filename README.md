# G2

![](https://github.com/Chencb1991/G2/blob/master/QQ%E6%88%AA%E5%9B%BE20190422184301.png)

```
[
  [
    "1",
    "1000",
    "2000",
    "10000",
    "20000",
    "1555923872"
  ],
  [
    "2",
    "1000",
    "2500",
    "10000",
    "20000",
    "1555923872"
  ],
  [
    "3",
    "1200",
    "2000",
    "10000",
    "20000",
    "1555923872"
  ],
  [
    "4",
    "1800",
    "2000",
    "12000",
    "60000",
    "1555923872"
  ],
  [
    "5",
    "1000",
    "2500",
    "10000",
    "20000",
    "1555923872"
  ],
  [
    "6",
    "6000",
    "3000",
    "10000",
    "23000",
    "1555923872"
  ],
  [
    "7",
    "8000",
    "2500",
    "17000",
    "20000",
    "1555923872"
  ],
  [
    "8",
    "10000",
    "2000",
    "10000",
    "40000",
    "1555923872"
  ],
  [
    "9",
    "10000",
    "2000",
    "20000",
    "60000",
    "1555923872"
  ]
]
```

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,height=device-height">
    <title>某企业经营现金流（5年报趋势）</title>
    <style>::-webkit-scrollbar{display:none;}html,body{overflow:hidden;height:100%;margin:0;}</style>
</head>
<body>
<div id="mountNode"></div>
<script>/*Fixing iframe window.innerHeight 0 issue in Safari*/document.body.clientHeight;</script>
<script src="https://gw.alipayobjects.com/os/antv/pkg/_antv.g2-3.5.1/dist/g2.min.js"></script>
<script src="https://gw.alipayobjects.com/os/antv/pkg/_antv.data-set-0.10.1/dist/data-set.min.js"></script>
<script src="https://gw.alipayobjects.com/os/antv/assets/lib/jquery-3.2.1.min.js"></script>


<style>
    .g2-label {
        font-size: 12px;
        text-align: center;
        line-height: 0.5;
        color: #595959;
    }

    .g2-label-spec {
        font-size: 12px;
        text-align: center;
        line-height: 0.5;
        color: #595959;
        width: 400px !important;
    }

    .g2-label-spec-value {
        color: #ff5250;
        font-weight: bold;
    }
</style>
<script>
	

	$.ajax({
		type:'get',
		url:'/sel.php',
		dataType:'json',
		success:function(data){
			console.log(data);
			var product=[];
			var Matcha=[];
			var Matcha2=[];
			var Matcha3=[];
			for(var i=0;i<data.length;i++){
				product.push(data[i][0]);
				Matcha.push(data[i][2]);
				
				Matcha3.push({'value':data[i][2]-data[i][1],'year':data[i][0]});
			};

			console.log(Matcha3);
			 var chart = new G2.Chart({
			    container: 'mountNode',
			    forceFit: true,
			    height: window.innerHeight,
			    padding: 'auto'
			  });
			  chart.source(Matcha3);
			  chart.scale('value', {
			    alias: '现金流(亿)'
			  });
			  chart.axis('year', {
			    label: {
			      textStyle: {
			        fill: '#aaaaaa'
			      }
			    },
			    tickLine: {
			      alignWithLabel: false,
			      length: 0
			    }
			  });

			  chart.axis('value', {
			    label: null,
			    title: {
			      offset: 30
			    }
			  });
			  chart.legend(false);
			  chart.interval().position('year*value').opacity(1).color('value', function(val) {
			    if (val<=0) {
			      return '#36c361';
			    }
			    return '#ff5957';
			  }).label('year*value', function(year, value) {
			    var offset = 15;
			    if (value < 0) {
			      offset *= -1;
			    }
			    return {
			      useHtml: true,
			      htmlTemplate: function htmlTemplate(text, item) {
			        var d = item.point;
			        if (d.value>=0) {
			          return '<div class="g2-label-spec">多大于空<span class="g2-label-spec-value"> ' + d.value + ' </span></div>';
			        }
			        return '<span class="g2-label">' + d.value + '</span>';
			      },
			      offset: offset
			    };
			  });

			  chart.render();
			

		}
	});


  
</script>
</body>
</html>
```





