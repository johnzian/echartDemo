<template>
  <div class="hello">
    <div id="myChart" :style="{width: '1024px', height: '600px'}"></div><!-- canvas的区域 -->
  </div>
</template>

<script>
const API_PROXY = 'https://bird.ioliu.cn/v1/?url='//跨域代理
export default {
  name: 'HelloWorld',
  data () {
    return {
      list : [],//全部列表
      users:[],//用户列表
      userobj:[]//用户订单
    }
  },
  methods:{
    getData(){//获取数据
      this.axios.get(API_PROXY + 'http://120.77.171.228:7001/api/v1/mcht/bill')
      .then((res)=>{
        this.list=res.data.data;
        console.log(this.list);
        for(var i=0;i<this.list.length;i++){//获得总共多少个用户
          if(this.users.indexOf(this.list[i].user_id)==-1){
            this.users.push(this.list[i].user_id);
            this.userobj.push([])
          }
        }
        for(var i=0;i<this.list.length;i++){//把每个用户的消费记录排序好
          for(var j=0;j<this.users.length;j++){
            if(this.list[i].user_id==this.users[j]){
              this.userobj[j].push(this.list[i])
            }
          }
        }


        let myChart = this.$echarts.init(document.getElementById('myChart'))
        myChart.setOption({
            title: {
                text: '消费记录'
            },
            tooltip: {
                trigger: 'axis'
            },
            legend: {
                data:[]
            },
            grid: {
                left: '3%',
                right: '4%',
                bottom: '3%',
                containLabel: true
            },
            yAxis: {
                type: 'value'
            },
            xAxis: {
                type: 'category',
                boundaryGap: false,
                data: []
            },
            series: [
            ]
        })
        console.log(this.userobj);
        let data=[];
        data=[];
        for(var i=0;i<this.users.length;i++){
          data.push(String(this.users[i]))
        }
        myChart.setOption({
            legend: {
                data:data
            },
        })
        data=[];
        for(var j=0;j<this.userobj.length;j++){
          for(var i=0;i<this.userobj[j].length;i++){
            // var date=formatDate(this.userobj[j][i].biz_time);
            // console.log(date);
            data.push(this.userobj[j][i].biz_time)
          }
        }
        data.sort((a, b) => new Date(a) - new Date(b))
        myChart.setOption({
                xAxis: {
                    data: data
                },
        })
        data=[];
        var obj={};
        for(var j=0;j<this.userobj.length;j++){
          obj={};
          obj.name=String(this.userobj[j][0].user_id);
          obj.type='line';
          obj.stack='总量';
          obj.data=[]
          for(var i=0;i<this.userobj[j].length;i++){
              obj.data.push(this.userobj[j][i].order_amount)
          }
        data.push(obj)
        }
        console.log(data);
        myChart.setOption({
          series: data
        })
      })
    },
    paixu(){//排序用户
      console.log(this.list);
      let myChart = this.$echarts.init(document.getElementById('myChart'))
      myChart.setOption({
          title: {
              text: '消费记录'
          },
          tooltip: {
              trigger: 'axis'
          },
          legend: {
              data:this.users
          },
          grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
          },
          yAxis: {
              type: 'value'
          },
          xAxis: {
              type: 'category',
              boundaryGap: false,
              data: []
          },
          series: [
          ]
      })
      for(var i=0;i<this.userobj.length;i++){
        var user=this.userobj[i];
        console.log(1);
        // data.push(this.userobj[0][i].biztime)
      }
      // myChart.setOption({

      // })
    },
    drawLine(){//画图
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById('myChart'))
        // 绘制图表
        myChart.setOption({
          title: {
              text: '消费记录'
          },
          tooltip: {
              trigger: 'axis'
          },
          legend: {
              data:this.users
          },
          grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
          },
          // xAxis: {
          //     type: 'category',
          //     boundaryGap: false,
          //     data: ['周一','周二','周三','周四','周五','周六','周日']
          // },
          // yAxis: {
          //     type: 'value'
          // },
          // series: [
          //     {
          //         name:'邮件营销',
          //         type:'line',
          //         stack: '总量',
          //         data:[120, 132, 101, 134, 90, 230, 210]
          //     },
          //     {
          //         name:'联盟广告',
          //         type:'line',
          //         stack: '总量',
          //         data:[220, 182, 191, 234, 290, 330, 310]
          //     },
          //     {
          //         name:'视频广告',
          //         type:'line',
          //         stack: '总量',
          //         data:[150, 232, 201, 154, 190, 330, 410]
          //     },
          //     {
          //         name:'直接访问',
          //         type:'line',
          //         stack: '总量',
          //         data:[320, 332, 301, 334, 390, 330, 320]
          //     },
          //     {
          //         name:'搜索引擎',
          //         type:'line',
          //         stack: '总量',
          //         data:[820, 932, 901, 934, 1290, 1330, 1320]
          //     }
          // ]
        });
    }
  },
  mounted(){
    this.getData()
    // this.drawLine()
    // this.paixu()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
</style>
