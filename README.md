# Tampermonkey_amiami
amiamiの小さなアイコンじゃどんなフィギュアか分からないので、詳細ページの画像に置き換えました。
    // ==UserScript==
    // @name         amiami
    // @namespace    http://tampermonkey.net/
    // @version      0.1
    // @description  :Home Goto HelloWork; :HelloWork Goto Home;
    // @author       sequre
    // @match        https://*.amiami.jp/*
    // @grant        none
    // ==/UserScript==
    "use strict";

    // 画像を置き換える。
    const images = document.body.querySelectorAll(".product_img>a>img");
    const lengthImage = images.length;
    let newimg;
    let img;
    let i, j;

    const resizeImg = (img1) =>
    {
        let img2;
        for(j = 0; j < lengthImage; j++)
        {
            img2 = images[j];
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

    for(i = 0; i < lengthImage; i++)
    {
        img = images[i];
        // search()内の文字列がsrcに含まれていない時は-1が返ってくる
        if(img.src.search(/thumbnail/) != -1)
        {
            newimg = new Image();
            // 最後の画像の時だけイベントリスナーを付ける
            if((i + 1) == lengthImage)
            {
                newimg.onload = resizeImg(img);
            }
            newimg.src = img.src.replace(/thumbnail/,"main");
            img.src = newimg.src;
        }
    }

    // セル内のulを透過させる
    let ul = document.getElementsByClassName("product_ul");
    let lengthUl = ul.length;
    let k;
    for(k = 0; k < lengthUl; k++)
    {
        ul[k].setAttribute("style", "opacity: 0.5;");
    }
