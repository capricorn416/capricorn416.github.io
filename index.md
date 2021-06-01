# Vue
`
<div id='app'>
  {{ message }} 
</div>
`
`
<script>
  const app = new Vue({
    el:'#app',  //用于挂载要管理的数据
    data: { //定义数据
      message: 'Hello Vue'
    }
  })
</script>
`
