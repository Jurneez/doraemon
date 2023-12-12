---
layout: post
title: input 回车触发事件
category: front 
---

<pre>
<input id="container_block_data" onkeyup="if(event.keyCode==13) {searchBlock()}" type="text">

<!-- 读取input中的值并赋值 -->
<script type="text/javascript">
    function searchBlock(){
        var container_block_data = document.getElementById("container_block_data");
        var sc = container_block_data.value
        alert(sc)
        container_block_data.value = "模版创建"
    } 
</script>
</pre>