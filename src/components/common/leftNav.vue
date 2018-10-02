<template>
  <div class="left-nav">
    <ul>
      <li>
        <i class="icon iconfont icon-wodezichan"></i>
        <div>CASH</div>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <div>STOR</div>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>PROD</div>
      </li>
      <li>
        <i class="icon iconfont icon-gongnengjianyi"></i>
        <div>SETT</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'leftNav'
}
</script>

<style>
.left-nav {
  color:#fff;
  font-size:10px;
  height:100%;
  background-color:#1D8CE0;
  float:left;
  width:5%;
}
.left-nav ul{
  padding:0px;
  margin:0px;
}
.left-nav li{
  list-style:none;
  text-align: center;
  border-bottom: 1px solid #20a0ff;
  padding:10px;
}
.iconfont{
  font-size:24px;
}
</style>
