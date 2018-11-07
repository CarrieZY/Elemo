# elemo

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).


## 一个让人烦躁的问题
> 在商家页面的商家实景图片那里   使用better-scroll的横向滚动  

```
 mounted(){
        this.$nextTick(() =>{
            this._initScroll()
            this._initPics()
        })
    },
    watch:{
        'seller'(){
            this.$nextTick( () =>{
                this._initScroll()
                this._initPics()
            })
        }
    },
    methods:{
          _initScroll(){
            if(!this.scroll){
                this.$nextTick(() =>{
                    this.scroll = new BScroll(this.$refs.seller,{
                        click:true,
                        eventPassthrough:'vertical'
                    })
                })
            }else{
                this.scroll.refresh()
            }
        },
        _initPics(){
            if(this.seller.pics){
                let picWidth = 120 
                let margin = 6
                let width = (picWidth+margin)*this.seller.pics.length-margin
                this.$refs.piclist.style.width=width 
                if(!this.pics){
                    this.pics=new BScroll(this.$refs.picswrapper,{
                        eventPassthrough:'vertical'
                    })
                }else{
                    this.pics.refresh()
                }
            }
        }
    }


```    