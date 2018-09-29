# [星云链上的Dapp开发你会了么？如果还是不会那就在看看这个bank demo](https://www.jianshu.com/p/f96e0a71e2a4)
- 看到这篇文章不错，转过来做一些代码深度注释
- 感谢作者 @[蓝胖子的代码](https://www.jianshu.com/u/77d840271d80)

bank demo 是一个基于星云链智能合约编写的的Dapp，它的功能包括存储，取出，查询功能，而星云链是全球首个主网上线的区块链3.0公链，所有代码全部重新构建，比起以太坊更清晰，更简练，对开发者极度友好，智能合约语言基于当下最流行的javascript，此demo可以作为刚刚接触NAS 想开发Dapp的开发者作为学习参考，下面我就一步步带大家去了解怎样做出一个在星云链上的Dapp。
https://xingyun.io/dapp/nasbank/

作者：蓝胖子的代码
链接：[https://www.jianshu.com/p/f96e0a71e2a4](https://www.jianshu.com/p/f96e0a71e2a4)

第一步，Web页面制作。
从这个页面我们可以获取到的信息有以下信息：

1. 页面的背景
2. 顶部的名称，logo，接下来的提示语NOTE:Please install WebExtensionWallet to use bank demo 是因为还没有安装星云钱包插件给个提示（什么是星云钱包插件？文章后面会说到）
3. 三个编辑框 ，三个按钮 （save, balance，takeout）

下面是对应的布局代码：
```javascript
<body>
    <div class="contenner">
        <div class="logo">
            <div class="name">bank demo</div>
            <div class="img logo_rotate">
                <img src="img/logo.png" alt="">
            </div>
        </div>
        
        <!-- 用来提示是否安装钱包 -->
        <div class="noExtension hide" id="noExtension">
            NOTE: Please install <a target="_blank" href="https://github.com/ChengOrangeJu/WebExtensionWallet">WebExtensionWallet</a>  to use bank demo
        </div> 
        <div class="search">
            <input id="search_value" type="text">
            <button id=search>save</button>
        </div>
        <div class="search">
            <input id="balance_value" type="text">
            <button id=balance>balance</button>
        </div>
        <div class="search">
            <input id="takeout_value" type="text">
            <button id=takeout>takeout</button>
        </div>

        <!-- <div class="result_success hide">
            <div id=search_banner></div>
            <p id=search_result> wait for content </p>
            <div class="author">
                <i><p> Author:</p> <p id=search_result_author> dasdajkajksdhjasdkjahdkjad</p></i>
            </div>
        </div> -->

        <div class="result_faile hide">
            Failed to find related information. Do you want to <button id="add">add</button> infromation for "<i id="result_faile_add">asd</i>"?
        </div>

        <div class="add_banner hide">
            <input type="text" id="add_value" placeholder="input contents for your keyword">
            <button id="push">submit</button>
            </div>
    </div>
    <script src=lib/jquery-3.3.1.min.js></script>
    <script src=lib/nebPay.js></script>
    <script src=lib/bootstrap-4.0.0-dist/js/bootstrap.min.js></script>
    <script>

        var NebPay = require("nebpay");     //https://github.com/nebulasio/nebPay 直接调用nebpay.js的sdk
        var nebPay = new NebPay(); // 创建nebpay对象

        document.addEventListener("DOMContentLoaded", function() {
            
            // 在dom上下文生成后 将#search_value #search 这两个元素设置属性 disabled 表示不可用
            $("#search_value").attr("disabled",true)
            $("#search").attr("disabled",true)

            console.log("web page loaded...") // 提示页面加载中
            setTimeout(checkNebpay,100); // 大概100毫秒后 触发checkNebpay函数
        });

        function checkNebpay() {
            console.log("check nebpay") // 提示触发nebpay函数

            //to check if the extension is installed
            //if the extension is installed, var "webExtensionWallet" will be injected in to web page
            if(typeof(webExtensionWallet) === "undefined"){ // WebExtensionWallet 星云钱包插件
                //alert ("Extension wallet is not installed, please install it first.")
                $("#noExtension").removeClass("hide") // 如果插件不存在 即浏览器没有安装星云钱包 就会提示装一个钱包
            }else{
            
                // 如果钱包已经安装了 那么这两个输入框可编辑
                $("#search_value").attr("disabled",false)
                $("#search").attr("disabled",false)
            }
        }

    //var dappAddress = "n1uNSyJRWKDXUhoBiNsURz4GXnCkPYAc2pZ"; // 测试网合约地址
    var dappAddress = "n1s9jBuWJFARs7odyC37xxcJJUFP6MxwUKm"; // 主网合约地址

    var ex = 1000000000000000000;

    // 存钱功能
    $("#search").click(function(){
        
        console.log("********* call smart contract by \"call\" *****************")

        var to = dappAddress;
        var value = $("#search_value").val() // 搜索框内的值
        var callFunction = "save"
        var callArgs = "[" + value + "]"

        nebPay.call(to, value, callFunction, callArgs,{    //使用nebpay的call接口去调用合约,
            callback: null
            }
        );

    })

    // 余额
    $("#balance").click(function(){

        console.log("********* call smart contract by \"call\" *****************")
  
        var to = dappAddress;
        var value = "0"
        var callFunction = "balanceOf"
        var callArgs = ""

        nebPay.simulateCall(to, value, callFunction, callArgs,{    //使用nebpay的call接口去调用合约,
            callback: balanceCallback
            }
        );

    })

    function balanceCallback(resp) {
        
        console.log("response of balanceCallback resp: " + JSON.stringify(resp))

        var result = JSON.parse(resp.result);

        if (result === 'null'){
            $(".add_banner").addClass("hide");
            $(".result_success").addClass("hide");

            $("#result_faile_add").text($("#search_value").val())

            $(".result_faile").removeClass("hide");
        } else{

            try{
                result = JSON.parse(e.data.data.neb_call.result)
            }catch (err){

            }

            $("#balance_value").val(result.balance / ex)
        }
    }

    // 取款
    $("#takeout").click(function(){

        console.log("********* call smart contract by \"call\" *****************")

        var to = dappAddress;
        var value = "0"
        var callFunction = "takeout"
        var callArgs = "[" + $("#takeout_value").val() * ex + "]"

        nebPay.call(to, value, callFunction, callArgs,{    //使用nebpay的call接口去调用合约,
            callback: null
            }
        );

    })

</script>
</body>
```
其中我们最需要关注的有以下几点：
1. WebExtensionWallet 星云钱包插件
2. dappAddress 这个是合约地址，到时部署好了之后修改为你自己的就可以了
3. neb.js 下面会说到 调用智能合约时用到

#
第二步，编写智能合约

```javascript
'use strict';

var DepositeContent = function (text) {
 if (text) {
   var o = JSON.parse(text);
   this.balance = new BigNumber(o.balance);
   this.expiryHeight = new BigNumber(o.expiryHeight);
 } else {
   this.balance = new BigNumber(0);
   this.expiryHeight = new BigNumber(0);
 }
};

DepositeContent.prototype = {
 toString: function () {
   return JSON.stringify(this);
 }
};

var BankVaultContract = function () {
 LocalContractStorage.defineMapProperty(this, "bankVault", {
   parse: function (text) {
     return new DepositeContent(text);
   },
   stringify: function (o) {
     return o.toString();
   }
 });
};

// save value to contract, only after height of block, users can takeout
BankVaultContract.prototype = {
 init: function () {
   //TODO:
 },

 save: function (height) {
   var from = Blockchain.transaction.from;
   var value = Blockchain.transaction.value;
   var bk_height = new BigNumber(Blockchain.block.height);
   var orig_deposit = this.bankVault.get(from);
   if (orig_deposit) {
     value = value.plus(orig_deposit.balance);
   }
   var deposit = new DepositeContent();
   deposit.balance = value;
   deposit.expiryHeight = bk_height.plus(height);
   this.bankVault.put(from, deposit);
 },

 takeout: function (value) {
   var from = Blockchain.transaction.from;
   var bk_height = new BigNumber(Blockchain.block.height);
   var amount = new BigNumber(value);
   var deposit = this.bankVault.get(from);
   if (!deposit) {
     throw new Error("No deposit before.");
   }
   if (bk_height.lt(deposit.expiryHeight)) {
     throw new Error("Can not takeout before expiryHeight.");
   }
   if (amount.gt(deposit.balance)) {
     throw new Error("Insufficient balance.");
   }
   var result = Blockchain.transfer(from, amount);
   if (!result) {
     throw new Error("transfer failed.");
   }
   Event.Trigger("BankVault", {
     Transfer: {
       from: Blockchain.transaction.to,
       to: from,
       value: amount.toString()
     }
   });
   deposit.balance = deposit.balance.sub(amount);
   this.bankVault.put(from, deposit);
 },

 balanceOf: function () {
   var from = Blockchain.transaction.from;
   return this.bankVault.get(from);
 },

 verifyAddress: function (address) {
   // 1-valid, 0-invalid
   var result = Blockchain.verifyAddress(address);
   return {
     valid: result == 0 ? false : true
   };
 }
};

module.exports = BankVaultContract;

```
后面还有两部，但和代码关系不大，建议直接访问原文，[点击查看](https://www.jianshu.com/p/f96e0a71e2a4)

