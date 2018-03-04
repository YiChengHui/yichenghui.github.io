## JavaScript

### 原生js事件绑定

    <ul class="lis">
        <li>第1个li</li>
        <li>第2个li</li>
        <li>第3个li</li>
        <li>第4个li</li>
        <li>第5个li</li>
        <li>第6个li</li>
        <li>第7个li</li>
        <li>第8个li</li>
        <li>第9个li</li>
        <li>第10个li</li>
    </ul>
``方法一：``
    
    for(var i=0;i<document.querySelector('.lis').children.length;i++){
        document.getElementsByTagName('li')[i].onclick=function () {
            console.log(this);
        }
    }
``方法二:``

    document.querySelector('.lis').addEventListener('click',function (e) {
        console.log(e.target||e.srcElement);
    });//利用的是事件委托，把事件绑定到父元素上
