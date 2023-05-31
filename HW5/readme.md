* 構思:
* 在學行銷的時候，顏色是很重要的一部份，因為顏色可以最快傳遞品牌的概念、展現獨特的個性給消費者
* 特別是在時尚精品方面，每季時裝周的公佈都是品牌展現不同態度的時候，透過判讀不同品牌的重點色系，除了可以發現品牌在不同季度的風格，進行探討與研究，挖掘為何這次採用的主色調選擇如此
* 除此之外，統整所有品牌的顏色，也可以解析今年的重點色是什麼，是什麼顏色成為今年流行的趨勢
* 會選擇這個主題，也是因為時裝周的照片通常是會由專業攝影師露出，只要一搜尋品牌關鍵字，就算沒有親臨時裝周現場，便可以很快地進行統整，所以是很適合用藉由電腦判讀節省時間、增加效率的學習工具
-----
$(document).ready(function(){
    //do something
    $("#thisButton").click(function(){
        processImage();
    });
    });


function processImage() {
    
    //確認區域與所選擇的相同或使用客製化端點網址
    var url = "https://eastus.api.cognitive.microsoft.com/";
    var uriBase = url + "vision/v2.1/analyze";
    
    var params = {
        "visualFeatures": "Categories,Objects,Color",
        "details":"",
        "language": "en",
    };
    //顯示分析的圖片
    var sourceImageUrl = document.getElementById("inputImage").value;
    document.querySelector("#sourceImage").src = sourceImageUrl;
    //送出分析
    $.ajax({
        url: uriBase + "?" + $.param(params),
        // Request header
        beforeSend: function(xhrObj){
            xhrObj.setRequestHeader("Content-Type","application/json");
            xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
        },
        type: "POST",
        // Request body
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    .done(function(data) {
        //顯示JSON內容
        $("#responseTextArea").val(JSON.stringify(data, null, 2));
        $("#picDescription").empty();
        
        $("#picDescription").append("Color: "+data.color.dominantColors+"<br>");
        $("#picDescription").append("Object: "+data.objects[0].object+"<br>");
    })
    .fail(function(jqXHR, textStatus, errorThrown) {
        //丟出錯誤訊息
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
        alert(errorString);
    });
};
-------
* [Homework 5 codes](https://github.com/HsinYu-W/LAT/blob/main/HW5/main.js)
---------
*  [Valentino 2022 主視覺顏色分析](valentino.png)
* [Chanel 2022 主視覺顏色分析](chanel.png)
