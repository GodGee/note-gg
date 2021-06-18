css

##### 1.button 中添加svg长宽过大，导致button文字不居中

​        svg {
​                    vertical-align: text-bottom; //主要代码
​                    margin-right: 8px;
​                }

```html
          <div class="downloadbtn">
                        <button class="js_download_adorawe_app" id="download_adorawe_app">
                            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <path d="M22.6666 22.6667H1.33325V16H3.99992V20H19.9999V16H22.6666V22.6667Z" fill="white" />
                                <path d="M12.0001 18.2667L5.4668 11.6001L7.33346 9.7334L12.0001 14.4001L16.6668 9.7334L18.5335 11.6001L12.0001 18.2667Z" fill="white" />
                                <path d="M13.3334 14.6663H10.6667V1.33301H13.3334V14.6663Z" fill="white" />
                            </svg>
                            <?= Yii::t("Goods_Sharing", "download_adorawe_app") ?>
                        </button>
                    </div>
                </div>
```

```css
     .download-adorawe-app-wrap {
            display: flex;
            align-items: center;
            height: 56px;
            margin-top: 12px;

            .downloadbtn {
                flex: 1 1 auto;
                padding: 8px 0;
                border-top: 1px solid transparent;

                button {
                    width: 100%;
                    height: 48px;
                    appearance: none;
                    border: none;
                    outline: 0;
                    background-color: #000;
                    color: #fff;
                    font-size: 18px;
                    cursor: pointer;
                    border-radius: 2px;
                    overflow: hidden;
                   
                }
                svg {
                    vertical-align: text-bottom;
                    margin-right: 8px;
                }
            }

            &.fixed {
                .downloadbtn {
                    position: fixed;
                    bottom: 0;
                    left: 0;
                    width: 100%;
                    z-index: 10;
                }
            }
        }
```

##### 2.一行三个元素，最后一个居右

​         flex: 1;//主要代码

![image-20210617111234432](C:\Users\SDLK\AppData\Roaming\Typora\typora-user-images\image-20210617111234432.png)

```html
 <div class="goods-info-flex">
                    <div class="title-and-reviews">
                        <h1 class="goodtitle">
                            Women's Fashion Watches
                        </h1>
                        <div style="display:none;" class="goodgrade js-goodsRate mt10"></div>
                    </div>
                    <div class="goods-price-wrap">
                        <p class="cur-price curPrice">
                            <span class="my-shop-price" data-oprice="10.96"><span class="icon">$</span>10.96</span>
                        </p>
                        <p class="market-price marketPrice" style="display:block;">
                            <span class="my-shop-price" data-oprice="28.64"><span class="icon">$</span>28.64</span>
                        </p>
                        <p class="like-icon-num">
                            <span class="like-icon">
                                <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                                    <rect opacity="0.01" width="16" height="16" fill="#9E9E9E" />
                                    <path fill-rule="evenodd" clip-rule="evenodd" d="M13.6388 7.99015L8.0001 13.6293L2.361 7.99015C0.990669 6.61982 0.990669 4.39808 2.361 3.02775C3.73133 1.65742 5.95308 1.65742 7.32341 3.02775L7.99994 3.70407L8.67643 3.02775C10.0227 1.68146 12.1908 1.65784 13.5659 2.95689L13.6388 3.02775C14.9851 4.37404 15.0087 6.54213 13.7097 7.91725L13.6388 7.99015Z" fill="#E63D2E" />
                                </svg>
                                <label>123</label>
                            </span>
                        </p>
                    </div>
                </div>
```

```css

.like-icon-num {
    flex: 1;
    .like-icon {
        display: flex;
        svg {
            margin: auto;
            margin-right: 2px;
        }
        label {
            margin-left: 2px;
            font-size: 12px;
            line-height: 22px;
            color: #999999;
            @media (max-width: 768px) {
                line-height: 18px;
            }
        }
    }
}
   .goods-info-flex {
            padding: 12px 0 0 0;
            border-bottom: 1px solid #eeeeee;

            @media (max-width: 767px) {
                display: block;
                padding-bottom: 0;
                border-bottom: 0;
            }

            .reviews-count {
                white-space: nowrap;
            }
        }

        .goods-price-wrap {
            // text-align: right;
            display: flex;
            align-items: center;
            margin-top: 33px;

            @media (max-width: 767px) {
                margin-top: 8px;
            }

            p {
                // text-align: right;
                line-height: 1em;
            }

            .market-price {
                display: none;
                text-decoration: line-through;
                font-size: 12px;
                color: #999;
                margin-left: 10px;
                align-self: flex-end;
            }

            .cur-price {
                font-size: 24px;
                color: #000;
                font-weight: 600;

                @media (max-width: 767px) {
                    font-size: 18px;
                }
            }

            .discount {
                display: none;
                padding: 6px 8.5px;
                background: #0d0d0d;
                color: white;
                font-size: 12px;
                margin-left: 8px;
            }
        }
```

##### 3.吸顶效果

```js
//吸顶
Sharing.ceiling = function(){
    let handle = {
        get addbtn (){
            return document.querySelector('#download_adorawe_app');
        },
        get target(){
            return document.querySelector('.download-adorawe-app-wrap');
        },
        fixed(state){
            this.fixed.timer && clearTimeout(this.fixed.timer);
            this.fixed.timer = setTimeout(() => {
                this.addbtn.parentElement.classList[state ? "add":"remove"]("fixed-download-btn");
            }, 100);
        },
        container: document.querySelector('#js-wrap-box')
    }
    if (typeof IntersectionObserver != "undefined") {
        let intersection = new IntersectionObserver(ioes => {
            if (window.innerWidth < 768) {
                handle.fixed (!ioes[ioes.length-1].isIntersecting);
            }
        },{threshold:1});
        intersection.observe(handle.target);
        let mutation = new MutationObserver(() => {
            intersection.observe (handle.target);
        });
        mutation.observe(handle.container, {childList: true});
    } else {
        let action = function () {
            if (window.innerWidth < 768) {
                let bound = handle.target.getBoundingClientRect();
                handle.fixed (bound.top + 52 > window.innerHeight || bound.top < 0)
            }
        }
        action ();
        $(window).scroll(action);
    }
    window.shouldFixedBtn = () => {
        if (window.innerWidth < 768) {
            let bound = handle.target.getBoundingClientRect();
            $('.site-state')[0].style.paddingBottom = '77px';
            return bound.top + 52 > window.innerHeight || bound.top < 0;
        }
        return false;
    }
    window.addEventListener('resize', () => {
        if (window.innerWidth >= 768) {
            handle.fixed (false);
        }
    })
}
```

```html
 <div class="download-adorawe-app-wrap">
                    <div class="downloadbtn">
                        <button class="js_download_adorawe_app" id="download_adorawe_app">
                            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <path d="M22.6666 22.6667H1.33325V16H3.99992V20H19.9999V16H22.6666V22.6667Z" fill="white" />
                                <path d="M12.0001 18.2667L5.4668 11.6001L7.33346 9.7334L12.0001 14.4001L16.6668 9.7334L18.5335 11.6001L12.0001 18.2667Z" fill="white" />
                                <path d="M13.3334 14.6663H10.6667V1.33301H13.3334V14.6663Z" fill="white" />
                            </svg>
                            <?= Yii::t("Goods_Sharing", "download_adorawe_app") ?>
                        </button>
                    </div>
                </div>
```

```css
       <div class="goods-saleinfo" id="js-wrap-box">
                <div class="goods-info-flex">
                    <div class="title-and-reviews">
                        <h1 class="goodtitle">
                            Women's Fashion Watches
                        </h1>
                        <div style="display:none;" class="goodgrade js-goodsRate mt10"></div>
                    </div>
                    <div class="goods-price-wrap">
                        <p class="cur-price curPrice">
                            <span class="my-shop-price" data-oprice="10.96"><span class="icon">$</span>10.96</span>
                        </p>
                        <p class="market-price marketPrice" style="display:block;">
                            <span class="my-shop-price" data-oprice="28.64"><span class="icon">$</span>28.64</span>
                        </p>
                        <p class="like-icon-num">
                            <span class="like-icon">
                                <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                                    <rect opacity="0.01" width="16" height="16" fill="#9E9E9E" />
                                    <path fill-rule="evenodd" clip-rule="evenodd" d="M13.6388 7.99015L8.0001 13.6293L2.361 7.99015C0.990669 6.61982 0.990669 4.39808 2.361 3.02775C3.73133 1.65742 5.95308 1.65742 7.32341 3.02775L7.99994 3.70407L8.67643 3.02775C10.0227 1.68146 12.1908 1.65784 13.5659 2.95689L13.6388 3.02775C14.9851 4.37404 15.0087 6.54213 13.7097 7.91725L13.6388 7.99015Z" fill="#E63D2E" />
                                </svg>
                                <label>123</label>
                            </span>
                        </p>
                    </div>
                </div>
                <div class="download-adorawe-app-wrap">
                    <div class="downloadbtn">
                        <button class="js_download_adorawe_app" id="download_adorawe_app">
                            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                                <path d="M22.6666 22.6667H1.33325V16H3.99992V20H19.9999V16H22.6666V22.6667Z" fill="white" />
                                <path d="M12.0001 18.2667L5.4668 11.6001L7.33346 9.7334L12.0001 14.4001L16.6668 9.7334L18.5335 11.6001L12.0001 18.2667Z" fill="white" />
                                <path d="M13.3334 14.6663H10.6667V1.33301H13.3334V14.6663Z" fill="white" />
                            </svg>
                            <?= Yii::t("Goods_Sharing", "download_adorawe_app") ?>
                        </button>
                    </div>
                </div>
            </div>



.fixed-download-btn {
    z-index: 99;
    box-sizing: border-box;
    position: fixed;
    top: 0; //吸顶 bottom: 0;// 吸底
    left: 0;
    padding: 4px 10px !important;
    width: 100%;
    background-color: #fff;
    border-top: 1px solid #eee !important;

    @supports (bottom: constant(safe-area-inset-bottom)) or
        (bottom: env(safe-area-inset-bottom)) {
        padding-bottom: ~"calc(constant(safe-area-inset-bottom) + 4px)" !important;
        padding-bottom: ~"calc(env(safe-area-inset-bottom) + 4px)" !important;
    }
}




  .site-state {
    padding:40px 0;
    .site-item{
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
    }
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    .pay-avai-main {
      width: 35px;
      height: 17.5px;
      &+.pay-avai-main {
        margin-left: 20px;
      }
    }
    .copyright {
      margin-left: 100px;
      font-size: 14px;
      color: #808080;
      &>a {
        color: #808080;
        text-decoration: underline;
      }
    }
  }
```

