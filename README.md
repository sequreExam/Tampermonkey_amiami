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
    let img, newimg;
    let i;

    for(i = 0; i < lengthImage; i++)
    {
        img = images[i];
        // search()内の文字列がsrcに含まれていない時は-1が返ってくる
        if(img.src.search(/thumbnail/) != -1)
        {
            newimg = new Image();
            newimg.src = img.src.replace(/thumbnail/,"main");
            img.src = newimg.src;
            if(img.naturalWidth > img.naturalHeight)
            {
                img.height = img.naturalHeight * (290 / img.naturalWidth);
                img.width = 290;
            }
            else if(img.naturalHeight > img.naturalWidth)
            {
                img.width = img.naturalWidth * (290 / img.naturalHeight);
                img.height = 290;
            }
            else
            {
                img.height = 290;
                img.width = 290;
            }
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
