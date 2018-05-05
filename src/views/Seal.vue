<template>
    <my-page title="印章生成">
        <div class="create-box">
            <div class="form-item">
                <ui-text-field v-model="text" hintText="输入你的名字（2~4个汉字）"/>
            </div>
            <div class="form-item">
                <ui-radio class="radio" label="随机" name="group" nativeValue="random" v-model="type"/>
                <ui-radio class="radio" label="阴刻" name="group" nativeValue="yin" v-model="type"/>
                <ui-radio class="radio" label="阳刻" name="group" nativeValue="yang" v-model="type"/>
            </div>
            <div class="form-item">
                <ui-raised-button label="生成印章" class="demo-raised-button" primary @click="make"/>
            </div>
            <ul class="result-list">
                <li class="item" v-for="result in results">
                    <div class="font-name">{{ result.fontName }}</div>
                    <img class="img" :src="result.image">
                </li>
            </ul>
        </div>
    </my-page>
</template>

<script>
    let fontArr = {
        '15402': '隶书',
        '126': '汉仪篆书繁',
        '8876': '方正藏体简体',
        '8943': '方正启体简体',
        '14237': '仿宋_GB2312',
        '14238': '幼圆',
        '14239': '方正粗谭黑简体',
        '15401': '楷体',
        '186': '方正剪纸简体',
        '8903': '方正豪体简体',
        '8875': '方正彩云_GBK',
        '8889': '方正粗宋_GBK',
        '8937': '方正隶书_GBK',
        '8940': '方正美黑_GBK',
        '8967': '方正水柱_GBK',
        '8981': '方正细珊瑚_GBK',
        '9003': '方正姚体_GBK',
        '265': '超世纪细印篆',
        '325': '中國龍金石篆',
        '6516': '北師大說文小篆',
        '8839': '華康新篆體',
        '8991': '方正小篆体',
        '9296': '金文大篆体',
        '9802': '金梅印篆字形空心'
    }
//    let fonturlarr = {
//        '15402': 'http://font.xiaa.net/zip/cn/15402.zip',
//        '126': 'http://font.xiaa.net/zip/cn/126.zip',
//        '8876': 'http://font.xiaa.net/zip/cn/8876.zip',
//        '8943': 'http://font.xiaa.net/zip/cn/8943.zip',
//        '14237': 'http://font.xiaa.net/zip/cn/14237.zip',
//        '14238': 'http://font.xiaa.net/zip/cn/14238.zip',
//        '14239': 'http://font.xiaa.net/zip/cn/14239.zip',
//        '15401': 'http://font.xiaa.net/zip/cn/15401.zip',
//        '186': 'http://font.xiaa.net/zip/cn/186.zip',
//        '8903': 'http://font.xiaa.net/zip/cn/8903.zip',
//        '8875': 'http://font.xiaa.net/zip/cn/8875.zip',
//        '8889': 'http://font.xiaa.net/zip/cn/8889.zip',
//        '8937': 'http://font.xiaa.net/zip/cn/8937.zip',
//        '8940': 'http://font.xiaa.net/zip/cn/8940.zip',
//        '8967': 'http://font.xiaa.net/zip/cn/8967.zip',
//        '8981': 'http://font.xiaa.net/zip/cn/8981.zip',
//        '9003': 'http://font.xiaa.net/zip/cn/9003.zip',
//        '265': 'http://font.xiaa.net/zip/cn/265.zip',
//        '325': 'http://font.xiaa.net/zip/cn/325.zip',
//        '6516': 'http://font.xiaa.net/zip/cn/6516.zip',
//        '8839': 'http://font.xiaa.net/zip/cn/8839.zip',
//        '8991': 'http://font.xiaa.net/zip/cn/8991.zip',
//        '9296': 'http://font.xiaa.net/zip/cn/9296.zip',
//        '9802': 'http://font.xiaa.net/zip/cn/9802.zip'
//    }
    let vttfarr = {
        '15402': '/ttf/cn/15402.ttf',
        '126': '/ttf/cn/126.ttf',
        '8876': '/ttf/cn/8876.TTF',
        '8943': '/ttf/cn/8943.ttf',
        '14237': '/ttf/cn/14237.ttf',
        '14238': '/ttf/cn/14238.ttf',
        '14239': '/ttf/cn/14239.ttf',
        '15401': '/ttf/cn/15401.ttf',
        '186': '/ttf/cn/186.ttf',
        '8903': '/ttf/cn/8903.TTF',
        '8875': '/ttf/cn/8875.ttf',
        '8889': '/ttf/cn/8889.ttf',
        '8937': '/ttf/cn/8937.ttf',
        '8940': '/ttf/cn/8940.ttf',
        '8967': '/ttf/cn/8967.ttf',
        '8981': '/ttf/cn/8981.ttf',
        '9003': '/ttf/cn/9003.ttf',
        '265': '/ttf/cn/265.TTF',
        '325': '/ttf/cn/325.TTF',
        '6516': '/ttf/cn/6516.ttf',
        '8839': '/ttf/cn/8839.TTC',
        '8991': '/ttf/cn/8991.ttf',
        '9296': '/ttf/cn/9296.ttf',
        '9802': '/ttf/cn/9802.ttf'
    }
    let encodingarr = {
        '15402': 'ucs-2',
        '126': 'ucs-2',
        '8876': 'ucs-2',
        '8943': 'ucs-2',
        '14237': 'ucs-2',
        '14238': 'ucs-2',
        '14239': 'ucs-2',
        '15401': 'ucs-2',
        '186': 'ucs-2',
        '8903': 'ucs-2',
        '8875': 'ucs-2',
        '8889': 'ucs-2',
        '8937': 'ucs-2',
        '8940': 'ucs-2',
        '8967': 'ucs-2',
        '8981': 'ucs-2',
        '9003': 'ucs-2',
        '265': 'ucs-2',
        '325': 'big5',
        '6516': 'big5',
        '8839': 'ucs-2',
        '8991': 'ucs-2',
        '9296': 'ucs-2',
        '9802': 'ucs-2'
    }
    let txtencodearr = {
        '15402': 'gb2312',
        '126': 'gb2312',
        '8876': 'gb2312',
        '8943': 'gb2312',
        '14237': 'gb2312',
        '14238': 'gbk',
        '14239': 'gb2312',
        '15401': 'gbk',
        '186': 'gb2312',
        '8903': 'gb2312',
        '8875': 'gbk',
        '8889': 'gbk',
        '8937': 'gbk',
        '8940': 'gbk',
        '8967': 'gbk',
        '8981': 'gbk',
        '9003': 'gbk',
        '265': 'big5',
        '325': 'big5',
        '6516': 'big5',
        '8839': 'big5',
        '8991': 'gb2312',
        '9296': 'gb2312',
        '9802': 'big5'
    }
//    let yztypearr = {
//        '15402': '1',
//        '126': '0',
//        '8876': '1',
//        '8943': '0',
//        '14237': '1',
//        '14238': '0',
//        '14239': '1',
//        '15401': '0',
//        '186': '1',
//        '8903': '0',
//        '8875': '1',
//        '8889': '0',
//        '8937': '1',
//        '8940': '0',
//        '8967': '1',
//        '8981': '0',
//        '9003': '1',
//        '265': '0',
//        '325': '1',
//        '6516': '0',
//        '8839': '1',
//        '8991': '0',
//        '9296': '1',
//        '9802': '0'
//    }

    export default {
        data () {
            return {
                text: '云设工具',
                type: 'random',
                results: []
            }
        },
        mounted() {
            this.init()
        },
        methods: {
            init() {
                this.gen('云设工具')
            },
            make() {
                if (!this.chinesetest(this.text) || this.text.length > 4 || this.text.length < 2) {
                    alert('你的名字不能为空，必须为二到四个汉字!')
                    return false
                }
                this.gen(this.text)
            },
            chinesetest(s) {
                for (let i = 0; i < s.length; i++) {
                    if (s.charCodeAt(i) < 0x4E00 || s.charCodeAt(i) > 0x9FA5) {
                        return false
                    }
                }
                return true
            },
            gen(text) {
                let yztypeval
                if (this.type === 'random') {
                    yztypeval = 0
                } else if (this.type === 'yin') {
                    yztypeval = 1
                } else {
                    yztypeval = 2
                }
                this.results = []
                for (let id in fontArr) {
                    let src = 'http://www.xiaa.net/yz/snapshot_yz.php?uname=' + encodeURIComponent(text) + '&id=' + id + '&vttf=' + encodeURIComponent(vttfarr[id]) + '&encoding=' + encodingarr[id] + '&txtencode=' + txtencodearr[id] + '&yztype=' + (yztypeval === 0 ? Math.ceil(Math.random() * 2) : yztypeval)
                    this.results.push({
                        fontName: fontArr[id],
                        image: src
                    })
                }
            }
        }
    }
</script>

<style lang="scss" scoped>
    @import "../scss/var";

    .form-item {
        margin-bottom: 16px;
        .radio {
            margin-right: 16px;
        }
    }
    .result-list {
        @include clearfix;
        .item {
            float: left;
            margin-right: 16px;
            margin-bottom: 16px;
            text-align: center;
        }
        .font-name {
            margin-bottom: 8px;
        }
    }
    .table {
    }
    .th {
        text-align: center;
    }
    .form-inline {
        margin-bottom: 16px;
    }
</style>
