现在我们的老虎机每次生存3个随机数，我们得去检查随机数是否全部相等的情况。

如果全部相等，我们应该提示用户他们赢了，并返回中奖号码，否则我们应该返回null。

null 是JavaScript中的一种数据类型，意味着空。

当这3个随机数相等的时候，判定用户赢。让我们创建一个if statement，用多个条件按顺序来检查它们是否相等。类似于：

if (slotOne === slotTwo && slotTwo === slotThree){

  return slotOne;

} else {

}

当3个随机数都一样的时候，我们把 "It's A Win" 追加到class logger的html中。

```javascript
<script>
  function runSlots() {
    var slotOne;
    var slotTwo;
    var slotThree;
    
    var images = ["//i.imgur.com/9H17QFk.png", "//i.imgur.com/9RmpXTy.png", "//i.imgur.com/VJnmtt5.png"];
    
    slotOne = Math.floor(Math.random() * (3 - 1 + 1)) + 1;
    slotTwo = Math.floor(Math.random() * (3 - 1 + 1)) + 1;
    slotThree = Math.floor(Math.random() * (3 - 1 + 1)) + 1;
    
    
    // 请把你的代码写在这条注释以下
    
    if (slotOne === slotTwo && slotTwo === slotThree){

      return slotOne;

      } else {
        return null;
      }
    
    // 请把你的代码写在这条注释以上
    
    if (slotOne !== undefined && slotTwo !== undefined && slotThree !== undefined){
      $(".logger").html(slotOne + " " + slotTwo + " " + slotThree);
    }
    
    $(".logger").append(" Not A Win");
    
    return [slotOne, slotTwo, slotThree];
  }

  $(document).ready(function() {
     $(".go").click(function() {
       runSlots();
     });
   });
</script>
 
<div>
 <div class = "container inset">
   <div class = "header inset">
     <img src="/images/freecodecamp_logo.svg" alt="learn to code JavaScript at Free Code Camp logo" class="img-responsive nav-logo">
     <h2>FCC Slot Machine</h2>
   </div>
   <div class = "slots inset">
     <div class = "slot inset">
       
     </div>
     <div class = "slot inset">
       
     </div>
     <div class = "slot inset">
       
     </div>
   </div>
   <br/>
   <div class = "outset">
     <button class = "go inset">
       Go
     </button>
   </div>
   <br/>
   <div class = "foot inset">
     <span class = "logger"></span>
   </div>
 </div>
</div>

<style>
 .container {
   background-color: #4a2b0f;
   height: 400px;
   width: 260px;
   margin: 50px auto;
   border-radius: 4px;
 }
 .header {
   border: 2px solid #fff;
   border-radius: 4px;
   height: 55px;
   margin: 14px auto;
   background-color: #457f86
 }
 .header h2 {
   height: 30px;
   margin: auto;
 }
 .header h2 {
   font-size: 14px;
   margin: 0 0;
   padding: 0;
   color: #fff;
   text-align: center;
 }
 .slots{
   display: flex;
   background-color: #457f86;
   border-radius: 6px;
   border: 2px solid #fff;
 }
 .slot{
   flex: 1 0 auto;
   background: white;
   height: 75px;
   margin: 8px;
   border: 2px solid #215f1e;
   border-radius: 4px;
 }
 .go {
   width: 100%;
   color: #fff;
   background-color: #457f86;
   border: 2px solid #fff;
   border-radius: 2px;
   box-sizing: none;
   outline: none!important;
 }
 .foot {
   height: 150px;
   background-color: 457f86;
   border: 2px solid #fff;
 }
 
 .logger {
   color: white;
   margin: 10px;
 }
 
 .outset {
   -webkit-box-shadow: 0px 0px 15px -2px rgba(0,0,0,0.75);
   -moz-box-shadow: 0px 0px 15px -2px rgba(0,0,0,0.75);
     box-shadow: 0px 0px 15px -2px rgba(0,0,0,0.75);
 }
 
 .inset {
   -webkit-box-shadow: inset 0px 0px 15px -2px rgba(0,0,0,0.75);
   -moz-box-shadow: inset 0px 0px 15px -2px rgba(0,0,0,0.75);
   box-shadow: inset 0px 0px 15px -2px rgba(0,0,0,0.75);
 }
</style>

```
