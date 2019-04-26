# G2

![](https://github.com/Chencb1991/G2/blob/master/QQ%E6%88%AA%E5%9B%BE20190426141727.png)

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


> vue cli

```
<template>
  <div class="mains">
   <div class="alig2" style="padding: 2em 0;width: 1500px;margin: auto;">
     <div id="mountNode"></div>

     <div id="lessNode" style="margin-top: 50px"></div>
   </div>
  </div>
</template>

<script>
export default {
  name: "mains",
  props: {
    msg: String
  },
  data(){
    return{
      moredata:'',
      lessdata:''
    }
  },
  mounted(){
    this.init()
  },
  methods:{
    init(){
      var that = this;
      this.$axios.get('/api', {
　　    params: { type: 'QHCC',
          sty: 'QHSYCC',
          stat: '3',
          fd: '2019-04-25',
          mkt: '069001008',
          code: 'ma1909',
          sc: 'MA'}
        })
      .then(function (response) {
         //console.log(response.data.slice(1).substring(0,response.data.length-2));
         let datass = response.data.slice(1).substring(0,response.data.length-2);
         console.log(JSON.parse(datass));
         let datasmore = JSON.parse(datass)[0].净多头龙虎榜;
         let datasless = JSON.parse(datass)[0].净空头龙虎榜;
         let datasempty = JSON.parse(datass)[0].空头增仓龙虎榜;
         let datasemptyL = JSON.parse(datass)[0].空头减仓龙虎榜;
         let datasover = JSON.parse(datass)[0].多头增仓龙虎榜;
         let datasoverL = JSON.parse(datass)[0].多头减仓龙虎榜;
         /////////////////////////////////////////////
         let datasmorecol = [];//净多头龙虎榜
         let dataslesscol = [];//净空头龙虎榜
         let arrempty = []; //空头增仓龙虎榜
         let arremptyL = []; //空头减仓龙虎榜
         let arreover = []; //多头增仓龙虎榜
         let arreoverL = []; //多头减仓龙虎榜
         /////////////////////////////////////////////
         for(var i=0;i<datasmore.length;i++){
            let datasmorearr = datasmore[i].split(",");
             datasmorecol.push({'id':datasmorearr[0],'name':datasmorearr[1]})     
         };
         for(var i=0;i<datasless.length;i++){
            let dataslessarr = datasless[i].split(",");
             dataslesscol.push({'id':dataslessarr[0],'name':dataslessarr[1]})     
         };

         for(var i=0;i<datasempty.length;i++){
            let datasemptyarr = datasempty[i].split(",");
             arrempty.push({'id':datasemptyarr[0],'name':datasemptyarr[1],'morenum':Number(datasemptyarr[4]),'totalnum':datasemptyarr[2],'title':'空头增仓龙虎榜'})     
         };//空头增

         for(var i=0;i<datasemptyL.length;i++){
            let datasemptyarrL = datasemptyL[i].split(",");
             arremptyL.push({'id':datasemptyarrL[0],'name':datasemptyarrL[1],'morenum':Number(datasemptyarrL[4]),'totalnum':datasemptyarrL[2],'title':'空头减仓龙虎榜'})     
         };//空头减

         for(var i=datasover.length-1;i>0;i--){
            let datasoverarr = datasover[i].split(",");
             arreover.push({'id':datasoverarr[0],'name':datasoverarr[1],'morenum':Number(datasoverarr[4]),'totalnum':datasoverarr[2],'title':'多头增仓龙虎榜'})     
         };//多头增

         for(var i=datasoverL.length-1;i>0;i--){
            let datasoverarrL = datasoverL[i].split(",");
             arreoverL.push({'id':datasoverarrL[0],'name':datasoverarrL[1],'morenum':Number(datasoverarrL[4]),'totalnum':datasoverarrL[2],'title':'多头减仓龙虎榜'})     
         };//多头减

         console.log(arreover.concat(arreoverL));
         that.moredata = arreover.concat(arreoverL);//多头多-空
         that.lessdata= arrempty.concat(arremptyL);
         that.more();
         that.less()
        //  console.log(datasmorecol); 
        //  console.log(dataslesscol); 
        //  console.log(arrempty); 
        //  console.log(arreover); 
        //  console.log(arremptyL); 
        // console.log(arreoverL); 
      })
      .catch(function (error) {
      console.log(error);
      });
      },

      more(){
        var chart = new G2.Chart({
          container: 'mountNode',
          forceFit: true,
          padding: 'auto'
        });

        chart.source(this.moredata);
        chart.scale('morenum', {
          alias: '持仓',
          tickInterval: 300
        });
        chart.axis('name', {
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

        chart.axis('morenum', {
          label: null,
          title: {
            offset: 30
          }
        });
        chart.legend(false);
       chart.interval().position('name*morenum').opacity(1).color('morenum', function(val){if (val <=0) {
            return '#36c361';
          }
          return '#ff5957';
        }).label('name*morenum', function(name, morenum) {
          var offset = 15;
          if (morenum < 0) {
            offset *= -1;
          }
          return {
            useHtml: true,
            htmlTemplate: function htmlTemplate(text, item) {
              var d = item.point;
              if (d.value>=0) {
                return '<div class="g2-label-spec"><span class="g2-label-spec-value"> ' + d.morenum + ' </span></div>';
              }
              return '<span class="g2-label">' + d.morenum + '</span>';
            },
            offset: offset
          };
        });

        chart.render();
      },


      less(){
        var chart = new G2.Chart({
          container: 'lessNode',
          forceFit: true,
          // height: 380,
          padding: 'auto'
        });

        chart.source(this.lessdata);
        chart.scale('morenum', {
          alias: '持仓',
          tickInterval: 300
        });
        chart.axis('name', {
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

        chart.axis('morenum', {
          label: null,
          title: {
            offset: 30
          }
        });
        chart.legend(false);
       chart.interval().position('name*morenum').opacity(1).color('morenum', function(val){if (val <=0) {
            return '#36c361';
          }
          return '#ff5957';
        }).label('name*morenum', function(name, morenum) {
          var offset = 15;
          if (morenum < 0) {
            offset *= -1;
          }
          return {
            useHtml: true,
            htmlTemplate: function htmlTemplate(text, item) {
              var d = item.point;
              if (d.value>=0) {
                return '<div class="g2-label-spec"><span class="g2-label-spec-value"> ' + d.morenum + ' </span></div>';
              }
              return '<span class="g2-label">' + d.morenum + '</span>';
            },
            offset: offset
          };
        });

        chart.render();
      }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
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

```





