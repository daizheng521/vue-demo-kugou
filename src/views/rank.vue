<template>
  <div class="container rank">
    <mt-cell  v-for="(item,index) in rankList" :title="item.title"  :to="item.location" is-link>
      <img slot="icon" :src="item.imgUrl" width="60" height="60">
    </mt-cell>
  </div>
</template>

<script type="es6">
  import { Cell,Indicator } from 'mint-ui'

  export default {
    data(){
      return {
        rankList:[]
      }
    },
    created(){
      this.getList();
    },
    methods:{
      getList(){
        Indicator.open({
          text: '加载中...',
          spinnerType: 'snake'
        });
        this.$http.get('http://lavyun.applinzi.com/apis/getPage.php?path=/rank/list').then((res)=>{
          Indicator.close();
          this.parseList(res.data);
        });
      },
      parseList(data){
        var div=document.createElement('div');
        div.innerHTML=data;
        var list=div.querySelectorAll('.panel-img-list li');
        for(var i=0;i<list.length;i++){
          var rank={};
          rank.title=list[i].querySelector('.panel-img-content p').innerText;
          rank.imgUrl=list[i].querySelector('.panel-img-left img').getAttribute('_src');
          rank.location="/rank/info/"+list[i].querySelector('a').href.substr(29);
          this.rankList.push(rank);
        }
      }
    }
  }
</script>
<style scoped>
  .mint-cell-title img{margin-right: 5px !important;}
</style>
