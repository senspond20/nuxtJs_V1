<template>
    <div class="container">
        <h1>안녕하세요</h1>
        <table class ="calendar" id ="cal1"></table>
    </div>
</template>

<style>
    .container {display: flex; justify-content: center; align-items: center;}
    .calendar { background-color: rgba(0,0,0,0.045); margin: 0 auto; width: 250px; height: 250px;}
    .calendar td {text-align: center; cursor: pointer;}
    .calendar button {
        background-color: rgba(0,0,0,0.015);
        color : #7e1e2f;
        width: 100%;
        height: 100%;
        border: none;
        /* border: 1px solid #d5d5d5; */
        border-radius: 6px;
        cursor: pointer;
    }
    .calendar button:hover {  background-color:  #d5d5d5;}
    .calendar button:active {border :none;}
    .calendar tr:first-child {
        background-color:  rgba(255,255,255,0.90);
        height: 45px;
        border-radius: 40px;
    }
    .calendar tr:first-child td:nth-child(2) {
        font-size: 16px;
        border-bottom: 1px solid rgba(0, 0, 0, 0.045);
        color: #7e1e2f;
    }
    .calendar #today{ display: inline-block; background-color :rgb(131, 49, 49); width: 20px; height: 20px; border-radius: 50%; color : #fff;}
    .calendar tr:nth-child(2) td:nth-child(1) { font-weight: 500;  color: #bb233f; } 
</style>


<script>
function init(){
    document.addEventListener("DOMContentLoaded", function(){
        var cal =new Calendar(new Date(), "cal1");
        cal.build();
    });
}
function Calendar(Date,  CalTableId) {
    this.tbl = document.getElementById(CalTableId); 
    this.isFirst = true;
    this.today = Date;
  };
  Calendar.prototype.setPrev = function(){
    var d = this.today;
    this.today = new Date(d.getFullYear(), d.getMonth() - 1, d.getDay());
    this.build();
  };
  Calendar.prototype.setNext = function(){
    var d = this.today;
    this.today = new Date(d.getFullYear(), d.getMonth() + 1, d.getDay());
    this.build();
  };
  Calendar.prototype.build = function(){
    var i = 0;
    var row = null;  // 행
    var cell = null; // 열
    var t = this.tbl;
    var year = this.today.getFullYear();
    var month = this.today.getMonth();
  
    // 달력이 처음실행되면
    if(this.isFirst){
          // 1. 해더 행을 그린다.
          row = t.insertRow();
          cell = row.insertCell();

          var prev = "prev" + t.id;
          var next = "next" + t.id;
  
          cell.innerHTML = "<button id =" + prev + ">&lt;</button>"; // 이전 달 버튼
          document.getElementById(prev)
                .addEventListener('click',()=>{
                this.isFirst = false;
                this.setPrev();
          })
          
          cell = row.insertCell();
          cell.colSpan = 5;
          cell.innerHTML =  "<div id= head" + t.id + ">" + year + "년" + (month + 1) + "월</div>";
  
          cell = row.insertCell();
          cell.innerHTML = "<button id =" + next + ">&gt;</button>"; // 다음 달 버튼
          document.getElementById(next)
              .addEventListener('click',()=>{
              this.isFirst = false;
              this.setNext();
         })
  
          // 2. 요일 행을 그린다.
          var week = ["일","월","화","수","목","금","토"];
          row = t.insertRow();
          for(i= 0; i < week.length; i++){
            cell = row.insertCell()
            cell.textContent = week[i];
          }
     // 아니면
    }else{
          // 기존 테이블에 있던 달력 숫자 (2행부터) 삭제
          while (t.rows.length > 2) {
            t.deleteRow(t.rows.length - 1);
          }
          this.isFirst = false;
    }
  
    if(!this.isFirst){
        document.getElementById('head' + t.id).innerHTML =
        "<div id= head" + t.id + ">" + year + "년" + (month + 1) + "월</div>";
    }
  
      var nMonth = new Date(year, month, 1);      // 이번달의 첫번째날
      var lastDate = new Date(year, month + 1, 0); //이번달의 마지막날
  
      row = t.insertRow();
      var cnt = 0;
  
      for (i = 0 ; i < nMonth.getDay(); i++) {
        cell = row.insertCell();
        cnt++;
      }
      //달력 출력
      for (i = 1; i <= lastDate.getDate(); i++) {
        cell = row.insertCell();
  
        var now = new Date();
        var nowYear = now.getFullYear();
        var nowMonth = now.getMonth();
        var nowdate = now.getDate();
  
        if(nowYear == year && nowMonth == month && nowdate == i){
          cell.innerHTML = "<span id ='today'>" + i + "</span>";
        }else{
          cell.textContent = i;
        }
        if (cnt++ && cnt % 7 == 0)      
             row = t.insertRow();
        }
  }
  init();

</script>
