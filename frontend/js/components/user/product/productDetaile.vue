<template >
        <div id="productDetail">


            
            
            <div class="productDetailWrapper">
                <div class="prodcuctName">
                    <h1>{{productDet.title}}</h1>
                </div>
                <div class="productDetails">
                    <div class="productDetailsWrap">







                        <div class="productImgAndOther">
                            <div class="productImg">
                                <img @click="zoomInOnPhoto($event)" class='photo' :src="getImgSrc()" alt="">
                               <!--  <div class="productImage"></div> -->
                                <div @click='zoomIn()'><img class='zoomSign' src="/images/mag.png" alt=""><p>بزرگنمایی</p></div>
                                




                                <div class="glider-contain">
                                    <div class="glider">
                                        <div><img @click="changeImage($event)" :src="getImgSrc()" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/1.jpg" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/ours1.png" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/3.jpg" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/4.jpg" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/1.jpg" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/2181-moon-black-and-white-wallpaper-file-hd.jpg" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/bandRole.png" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/signupBack2.jpg" alt=""></div>
                                        <div><img @click="changeImage($event)" src="/images/abchasb.png" alt=""></div>
                                        <!-- <div><img @click="changeImage($event)" src="/images/ours1.png" alt=""></div> -->
                                    </div>
                                </div>




                                
                                <div class='numbers'><p>مانده:<span>{{productDet.stock}}</span></p></div>
                            </div>
                        </div>






                        <div class="productSingleDetailWrapper">
                            <div class="justReadableDetailWrapper">
                                <div class="price singleDetail">
                                    <div class='order1'><p>قیمت:</p></div>
                                    <div class='order2'><p>{{productDet.price}}</p></div>
                                </div>
                                <div class="price singleDetail">
                                    <div class='order1'><p>حداقل تعداد قابل خرید:</p></div>
                                    <div class='order2'><p>{{productDet.minimum_order}}</p></div>
                                </div>
                                <div class="price singleDetail">
                                    <div class='order1'><p>ارائه نمونه:</p></div>
                                    <div class='order2'><p>{{productDet.samples}}</p></div>
                                </div>
                                <div class="price singleDetail">
                                    <div class='order1'><p>امتیاز:</p></div>
                                    <div class='order2'><p>4.5</p></div>
                                </div>
                                <div class="price singleDetail">
                                    <div class='order1'><p>ساخت:</p></div>
                                    <div class='order2'><p>{{productDet.made_in}}</p></div>
                                </div>
                                <div class="rating">
                                    <div class="star">
                                        <star-rating :fixed-points="2" :increment="0.01"></star-rating>
                                    </div>
                                </div>
                                <input type="hidden" :value="payWay">
                                <input type="hidden" :value="packetType">
                                <input type="hidden" :value="sendWay">
                            </div>
                            <div class="contactUs">
                                <div class="contactUsWrapper">
                                    <button class="stelam" @click.prevent="getPriceRequest">استعلام قیمت</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <done-message></done-message>
            <consulate></consulate>
        </div>
</template>

<script>
// import ImageZoom from 'js-image-zoom'
import doneMessage from "../../user/template/doneMessage/doneMessage.vue"
import starRating from 'vue-star-rating'
import consulate from "../prodcuts/consulate.vue"
import {keepStay} from "../../user/mixIns/keepStay.js"
import {adjustElFromTop} from "../../user/mixIns/adjustElFromTop.js"
    export default {
        props:['productDet'],
        mounted(){
           new Glider(document.querySelector('.glider'), {
                slidesToShow: 'auto',
                slidesToScroll:'auto',
                draggable: true
            });
        },
        mixins:[keepStay,adjustElFromTop],
        components:{
            starRating,
            doneMessage,
            consulate
        },
        data(){
            return{
                payWay:null,
                packetType:null,
                sendWay:null
            }
        },
        methods:{
            zoomIn(){
                console.log('clicked')
                const photo=document.querySelector(".photo")
                const url=photo.getAttribute("src")
                const scrollFromTop=document.body.scrollTop+10
                
                this.$store.state.url=url
                const zoom=document.querySelector(".zoomIn")
                const img=zoom.querySelector("img")
                img.style.top=window.scrollY+"px"
                img.setAttribute("src",url)
                zoom.style.display='block'
                console.log(img.style.top)
            },
            zoomInOnPhoto(e){
                const el=e.target;
                const src=el.getAttribute("src")
                this.$store.state.url=src
                const zoom=document.querySelector(".zoomIn")
                const img=zoom.querySelector("img")
                img.style.top=window.scrollY+"px"
                img.setAttribute("src",src)
                zoom.style.display='block'
            },
            changeButtonColor(e){
                const targetBtn=e.target
                const nextEl=targetBtn.nextElementSibling   
                const preEl=targetBtn.previousElementSibling   
                            
                if(nextEl!=null)
                {
                    
                    nextEl.style.border="2px solid black"
                    nextEl.style.color="black"
                }
                else if(preEl!=null){
                    
                    preEl.style.border="2px solid black"
                    preEl.style.color="black"
                }
                targetBtn.style.border="2px solid orangered"
                targetBtn.style.color='orangered'
            },
            changePacketType(e){
                this.packetType=e.target.innerText
            },
            chageneSendWay(e){
                this.sendWay=e.target.innerText
            },
            changePayWay(e){
                
                this.payWay=e.target.innerText

            },
            getPriceRequest(){
                const consulet=document.querySelector(".consulate")
                const wrapper=document.querySelector(".consulateWrapper")
                consulet.style.display="block"
                this.adjustFromTop(wrapper,false,true)
                document.body.style.overflow="hidden"
                
                
            },
            sendPriceRequest(){
                const done=document.querySelector("#doneMessage")
                done.style.display='block'
                setTimeout(()=>{
                    done.style.display="none"
                },5000)

            },
            closeConsulate(){
                const consulet=document.querySelector(".productConsulate")
                consulet.style.display="none"

            },
            changeImage(e){
                const el=e.target
                console.log("hey")
                const img=el.querySelector("img")
                const src=el.getAttribute("src")
                const photo=document.querySelector(".photo")
                photo.setAttribute("src",src)

            },
            getImgSrc(){
                return this.productDet.product_image
            }
        }
    }
</script>


<style scoped>
.glider.draggable .glider-slide img {
    user-select: none;
    padding:5px;
    pointer-events: none;
    padding: 5px;
}
.photo:hover{
    cursor: pointer;
}
.splide img{
    width:100px !important
}
.productConsulate{
    position:absolute;
    background: rgba(0,0,0,0.6);
    height:100%;
    width:100%;
    top:0;
    left:0;
    display:none;
    z-index:668

}
.glider img{
    user-select: inherit; 
    pointer-events: all !important;
    height:150px;
}
.inputs{
    margin-top:5px
}
.productConsulateWrapper{
    position:fixed;
    background:#ffffff;
    padding:10px;
    transform: translateY(50%) translateX(-50%);
    left: 50%;
}
.sendReq{
    display:flex;
    justify-content: center;
}
.contactUsWrapper{
    width:100%;
    display:flex;
    justify-content: center;
}
.productImgAndOther{
    width:40%
}
    .writeAbleSignleItemTitle{
        width:50%
    }
    .writeAbleDetaileWrapper{
        display:flex;
        flex-direction:column;
        align-items: flex-end;
    }
    .writeAbleSignleItemBtns{
        margin-right:10px;
        width:50%;
        display:flex;
        justify-content: flex-end;
    }
    .writeAbleSignleItemBtns button{
        width:80px;
        margin:10px
    }
    .writeAbleSignleItem{
        display:flex;
        align-items: center;
        margin-top:20px;
        width:100%
    }
    .selectAbleBtns:focus{
        outline:none
    }
    .selectAbleBtns{
        background: white;
        padding:10px;
        border:2px solid black;
        font-size:13pt;
        font-weight: 600;
        transition: all 0.2s linear;
        color:black
    }
    #productDetail{
        border-bottom:1px solid rgb(199,199,199);
    }
    .productImg img{
        width: 100%;
        
        /* box-shadow:2px 2px 50px grey */
    }
    .photo{
        height:400px !important;
        object-fit:contain;
    }
    .productImg{
        display:flex;
        /* height: 400px; */
        justify-content: flex-start;
        align-items:center;
        flex-direction:column;
    }
    .productImg div{
        display: flex;
        align-items: flex-end;
        justify-content: center;
        

    }
    .zoomSign{
        cursor: pointer;
    }
    .productImg div p{
        font-size: 14px;
        font-family: serif;
    }
    .zoomSign{
        max-width:15px;
        max-height:15px;
        margin-top:5px;
        margin-right: 5px;
    }
    .prodcuctName{
        text-align:left;
        width:100%;
    }
    .prodcuctName h1{
        width:100%;
        text-align:right;
        padding-bottom: 10px;
        font-size: 17pt;
    font-weight: bold;
    }
    .productDetailWrapper{
        display:flex;
        
        padding:20px;
        flex-direction:column;
        justify-content: flex-start;
    }
    .productDetail{
        width:100%;
        display: flex;
        flex-direction:column;
        align-items: flex-start;
    }
    .productSingleDetailWrapper{
        width:60%;
        display:flex;
        flex-direction: column;
    }
    .justReadableDetailWrapper{
        width:100%;
        display:flex;
        flex-direction: column;
        align-items: center;

    }
    .productDetails{
        display:flex;
        justify-content:flex-start
    }
    .productDetailsWrap
    {
        display: flex;
        justify-content: flex-start;
        width:100%
    }
    .singleDetail{
        display:flex;
        width:100%;
        justify-content: flex-start;
        margin-top:10px;
    }
    .singleDetail p{
        max-width: max-content;
    }
    .order1{
        order:2;
        width:65%;
        display: flex;
        justify-content: flex-end;
    }
    .order2{
        order:1;
        width:35%;
        display: flex;
        justify-content: flex-end;
        word-wrap: break-word;
    }
    .singleDetail{
        padding:10px;
        margin-left:5px;
        
    }
    .productSingleDetailWrapper .singleDetail:nth-child(odd){
        background:#eeeeee
    }
    .rating{
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .rating div:nth-child(1)
    {
        margin-top:5px;
        
    }
    .rating{
        width:90%
    }
    .rating div:nth-child(2)
    {
        margin-top:5px;
    }
    .point p{
        font-size: 20pt;
    }
    .numbers{
        margin-top:20px;
    }
    .numbers p{
        font-size:18pt !important
    }
    .numbers span{
        color:rgb(240, 48, 48)
    }
    @media (max-width:650px)
    {
        .writeAbleSignleItem{
            flex-direction:column
        }
        .writeAbleSignleItemTitle{
            width:100%;
            order:1
        }
        .writeAbleSignleItemBtns{
            width:100%;
            order:2
        }
    }
    @media (max-width:800px)
    {
        .productDetailsWrap{
            flex-direction:column;
            align-items:center
        }
        .productImg{
            display:flex;
            align-items: center;
        }
        .productImgAndOther{
            width:100%
        }
        .productSingleDetailWrapper{
        width:100%;
        display:flex;
        flex-direction: column;
        }
    
    }

</style>