運用Azure影像辨識技術解析時尚品牌的重點色系與流行趨勢

- 專題目標：
分析時尚品牌的顏色選擇：通過影像辨識技術，解析各大品牌在不同季度的色彩選擇。
發掘年度流行色彩：統整多個品牌的色彩數據，識別出當年度的流行趨勢。
- 方法：
資料蒐集：
利用關鍵字搜尋時裝周品牌圖片（由專業攝影師拍攝）。
影像分析：
使用Azure影像辨識技術自動判讀圖片中的顏色資訊。
數據統整與分析：
統計不同品牌的重點色系，並與前幾季度進行比較。
分析當年度多個品牌的共同色系，推測年度流行色。
- 專題重要性：
解決廣告代理商的困難：
傳統上，廣告代理商需依賴時尚雜誌的分析撰寫文案，這耗時且受限。此專題提供了利用AI影像辨識技術進行更快更準確的分析方法，節省時間並提高效率。
創建廣告代理商的私用數據庫：
此專題可作為廣告代理商的專屬數據庫，透過累積的顏色分析數據，不僅能提高分析的精確性，還能針對特定的精品品牌客戶進行競品分析，為廣告策略提供更有力的支持。
品牌分析與流行趨勢預測：
揭示品牌設計風格的變遷與策略考量，並幫助時尚產業提前掌握色彩流行趨勢。
效率提升：
節省人工分析時間，增加數據分析的準確性。
- 預期成果：
深入的品牌色彩報告：提供品牌的色彩選擇洞見。
年度流行色彩預測：為業界提供有價值的趨勢預測。
廣告代理商的專屬工具：支持廣告策略和競品分析，提升行銷效果。

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
