<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="less">
html,body{
  height: 100%;
}
.fl{
  float: left;
}
.fr{
  float: right;
}
.clearfix::after{
  display: block;
  content: '';
  clear: both;
}
.auto-img{
  width: 100%;
  display: block;
}
.one-text{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}
p{
  margin: 0;
}
</style>
