# javascript-
活动倒计时js（兼容ie8）
 //倒计时start
    var countDown='<div id="countDown"></div>';
    $('.zlb_pageh').append(countDown);
    $.ajax({
        url: '//ihr.zhaopin.com/turnlottery/getCurrTime.do',
        type: 'GET',
        dataType:'jsonp',
        success: function (d) {
            if (d.code == 200 && d.data && d.data != null) {
                var currentTime = (new Date(d.data.currTime)).getTime();
                doubleActivity(currentTime);
            }else{
                alert(d.message);
            }
        }
    });
    //doubleActivity(new Date());
    function doubleActivity(currentTime) {
        var zlbOne = $('.zlbMain01 div').eq(1).find('h4').text(),
        zlbTwo = $('.zlbMain01 div').eq(1).find('p').text(),
        giveTwo=zlbTwo.substring(1,zlbTwo.length),entText;
        zlbOne!=giveTwo?entText='':entText='<t style=" margin-left: 20px;color: #cd0e00;">秒杀名额已用尽！下次再试吧。</t>';

        var criterionT = new Date(currentTime);
        var startTime = new Date(criterionT.getFullYear(), criterionT.getMonth(), criterionT.getDate(), 0, 0, 0);
        var activeEnd = (new Date("2017/11/11 12:00:00")).getTime(),//秒杀活动已结束;

        tenTime = (startTime.getTime() + 10 * 60 * 60 * 1000),//上午10时
        tomorrowTime = (startTime.getTime() + 34 * 60 * 60 * 1000),//上午10时
        twelveTime = (startTime.getTime() + 12 * 60 * 60 * 1000 - 1),//上午11时59分59秒
        time;//当前时间
        function countDown(time, active) {
            setInterval(function () {
            var day = parseInt(time / 1000 / 60 / 60 / 24);
            var hour = parseInt(time / 1000 / 60 / 60 % 24);
            var minute = parseInt(time / 1000 / 60 % 60);
            var seconds = parseInt(time / 1000 % 60);
            active == 0 ? $('#countDown').html('距本次秒杀结束还有：<span class="hour">' + hour + '</span> 小时 <span class="minute">' + minute + '</span> 分钟 <span class="seconds">' + seconds + '</span> 秒'+entText) : $('#countDown').html('距下次秒杀还有：<span class="hour">' + hour + '</span> 小时 <span class="minute">' + minute + '</span> 分钟 <span class="seconds">' + seconds + '</span> 秒');
            time -= 1000;
            }, 1000);
        }

        if (currentTime >= activeEnd) {
            $('#countDown').html('秒杀活动已结束！<span class="hour">00</span> 小时 <span class="minute">00</span> 分钟 <span class="seconds">00</span> 秒');
        } else {
            if (currentTime >= tenTime && currentTime <= twelveTime) {
                time = twelveTime - currentTime;
                countDown(time, 0);
            } else if (currentTime >= twelveTime || currentTime < tomorrowTime) {
                time = tomorrowTime - currentTime;
                countDown(time, 1);
            }
        }
    }
