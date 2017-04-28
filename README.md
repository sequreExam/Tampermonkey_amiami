# Tampermonkey_amiami
    // ==UserScript==
    // @name         amiami
    // @version      1.0
    // @description  :Home Goto HelloWork; :HelloWork Goto Home;
    // @author       sequre
    // @match        http://*.amiami.jp/*
    // @grant        none
    // ==/UserScript==
    /* jshint -W097 */
    "use strict";

    // Your code here...
    // 画像を置き換える。
    var images = document.body.querySelectorAll(".product_img>a>img"); // images.length = 100
    var targetImageCount = document.getElementsByClassName("product_img").length; // targetImageCount = 100
    var img;

    //console.log("tIC = " + targetImageCount);
    for(var i = 0, m = images.length; i < m; i++)
    {
        img = images[i];
        // search()内の文字列がsrcに含まれていない時は-1が返ってくる
        if(img.src.search(/thumbnail/) != -1)
        {
            var newimg = new Image();
            // 最後の画像の時だけイベントリスナーを付ける
            if((i + 1) == m)
            {
                newimg.onload = function()
                {
                    for(var j = 0; j < m; j++)
                    {
                        var img2 = images[j];
                        if(img2.naturalWidth > img2.naturalHeight)
                        {
                            img2.height = img2.naturalHeight * (290 / img2.naturalWidth);
                            img2.width = 290;
                        }
                        else if(img2.naturalHeight > img2.naturalWidth)
                        {
                            img2.width = img2.naturalWidth * (290 / img2.naturalHeight);
                            img2.height = 290;
                        }
                        else
                        {
                            img2.height = 290;
                            img2.width = 290;
                        }
                        //console.log(img2.src + " : " + img2.naturalWidth + "x" + img2.naturalHeight + " → " + img2.width + "x" + img2.height);
                    }
                };
            }
            newimg.src = img.src.replace(/thumbnail/,"main");
            img.src = newimg.src;
        }
    }

    // セル内のulを透過させる
    var ul = document.getElementsByClassName("product_ul");
    for(var k = 0, n = ul.length; k < n; k++)
    {
        ul[k].setAttribute("style", "opacity: 0.5;");
    }
