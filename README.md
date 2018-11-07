# elemo

> 基于Vue全家桶实现的外卖webAPP
> 如果觉得对您有帮助，您可以在右上角给我个star支持一下，谢谢！

## 项目说明
> 用到的技术栈  
vue2.x + vue-router2 + webpack + vue-cli3 + axios + stylus + flex 
> 由于是个人项目  所以我没有用 eslint  个人觉得由于格式报错比较烦  
> 但是eslint还是可以帮助及时找到报错信息的

## Build Setup

``` bash
# 克隆项目到本地以及配置公钥
查看文档 http://www.bootcss.com/p/git-guide/

# 安装依赖  
npm install

# 开启服务器
npm run dev

# build for production with minification
npm run build   打包项目生成dist文件夹    

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
>> 我这样做了还是不能滚动  改用swiper