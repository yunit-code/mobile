<template>
    <div idm-ctrl="idm_module" :id="moduleObject.id" :idm-ctrl-id="moduleObject.id">
        <div class="IHeaderBar_app flex_between">
            <div class="IHeaderBar_app_left">
                <img v-if="propData.logoImgSrc" :src="IDM.url.getWebPath(propData.logoImgSrc)" alt="">
                <img v-else :src="IDM.url.getModuleAssetsWebPath(require('../assets/cms-logo.png'),moduleObject)" alt="">
            </div>
            <div @click="handleRightClick" class="IHeaderBar_app_right">
                <SvgIcon icon-class="menu"></SvgIcon>
            </div>
            
        </div>
        <!-- drag container -->
        <div v-if="propData.isOpenBottomContainer" class="drag_container idm-navbar-bottom-animate" :style="bottomContainerStyle" :class="{ 'idm-navbar-bottom-fix': propData.bottomIsFixTop }" :idm-ctrl-id="moduleObject.id" idm-container-index="1" >
        </div>
    </div>
</template>
  
<script>
import adaptationScreenMixin from '../mixins/adaptationScreen'
import SvgIcon from '../icons/SvgIcon.vue';
import { menuData } from '../mock/mockDataCms.js'
import {  } from "vant";

export default {
    name: 'IHeaderBar',
    mixins: [adaptationScreenMixin],
    components: {
        SvgIcon
    },
    data() {
        return {
            moduleObject: {},
            propData: this.$root.propData.compositeAttr || {
                widthLogo: {
                    inputVal: 232,
                    selectVal: 'px'
                },
                heightLogo: {
                    inputVal: 40,
                    selectVal: 'px'
                }
            },
            bottomContainerStyle: {},
            bottomIsShow: true
        }
    },
    props: {
    },
    created() {
        this.moduleObject = this.$root.moduleObject;
        this.bottomIsShow = this.propData.defaultStatusBottomContainer;
        this.convertAttrToStyleObject();
        this.convertThemeListAttrToStyleObject()
        this.setBottomContainerIsShow();
    },
    watch: {
        
    },
    mounted() {
        
    },
    destroyed() { },
    methods: {
        handleRightClick() {
            if (this.propData.isOpenBottomContainer) {
                this.bottomIsShow = !this.bottomIsShow;
            } else {
                this.bottomIsShow = false;
            }
            this.setBottomContainerIsShow()
        },
        setBottomContainerIsShow() {
            if (this.propData.bottomIsFixTop) {
                this.bottomContainerStyle = {
                    top: this.propData.height
                }
            }
            if (!this.propData.bottomIsFixTop && this.propData.isFixTop) {
                this.bottomContainerStyle = {
                    marginTop: this.propData.height
                }
            }
            if (this.bottomIsShow) {
                setTimeout(() => {
                    this.bottomContainerStyle = {
                        ...this.bottomContainerStyle,
                        'min-height': '66px !important',
                        height: 'auto !important'
                    }
                }, 300)
            } else {
                this.bottomContainerStyle = {
                    ...this.bottomContainerStyle,
                    'min-height': '0px !important',
                    height: '0px !important'
                }
            }
            this.bottomContainerStyle = {
                ...this.bottomContainerStyle,
                'z-index': this.propData.bottomZIndex,
            }
        },
        /** * ???????????? */
        convertThemeListAttrToStyleObject() {
            const themeList = this.propData.themeList;
            if ( (!themeList) || !themeList.length ) {
                return
            }
            const themeNamePrefix = IDM.setting && IDM.setting.applications && IDM.setting.applications.themeNamePrefix ? IDM.setting.applications.themeNamePrefix : "idm-theme-";
            for (var i = 0; i < themeList.length; i++) {
                var item = themeList[i];
                
                if(item.key!=IDM.theme.getCurrentThemeInfo()){
                    //?????????????????????????????????????????????????????????????????????????????????????????????
                    continue;
                }
                let fontStyleObject = {
                    "color": item.mainColor ? item.mainColor.hex8 : "",
                }
                let fontStyleObjectButton = {
                    "color": '#fff',
                }
                let borderStyleObject = {
                    'border-color': item.mainColor ? item.mainColor.hex8 : "",
                }
                let backgroundBorderObject = {
                    'color': '#fff',
                    'background-color': item.mainColor ? item.mainColor.hex8 : "",
                    'border-color': item.mainColor ? item.mainColor.hex8 : ""
                }
                let backgroundBorderObjectHover = {
                    'color': '#fff',
                    'background-color': item.minorColor ? item.minorColor.hex8 : "",
                    'border-color': item.minorColor ? item.minorColor.hex8 : "",
                }
                IDM.setStyleToPageHead( "." + themeNamePrefix + item.key + " #" + (this.moduleObject.packageid || "module_demo") + " .IHeaderBar_app_right svg)", fontStyleObject );
            }
        },
        /**
         * ?????????????????????????????????prop????????????
         */
        propDataWatchHandle(propData) {
            this.propData = propData.compositeAttr || {};
            this.convertAttrToStyleObject();
        },
        /** * ?????????????????????????????? */
        convertAttrToStyleObjectContent() {
            var styleObjectIcon = {};
            var styleObjectLogo = {};
            for (const key in this.propData) {
                if (this.propData.hasOwnProperty.call(this.propData, key)) {
                    const element = this.propData[key];
                    if (!element && element !== false && element != 0) {
                        continue;
                    }
                    switch (key) {
                        case 'menuIconFontSize':
                            styleObjectIcon['font-size'] = this.getAdaptiveSize(element.inputVal) + element.selectVal;
                            break;
                        case 'menuIconFontColor':
                            if (element && element.hex8) {
                                styleObjectIcon["color"] = element.hex8;
                            }
                            break;
                        case 'widthLogo':
                            styleObjectLogo['width'] = this.getAdaptiveSize(element.inputVal,'',1) + element.selectVal;
                            break;
                        case 'heightLogo':
                            styleObjectLogo['height'] = this.getAdaptiveSize(element.inputVal,'',1) + element.selectVal;
                            break;
                    }
                }
            }
            window.IDM.setStyleToPageHead(this.moduleObject.id + ' .IHeaderBar_app_left img', styleObjectLogo);
            window.IDM.setStyleToPageHead(this.moduleObject.id + ' .IHeaderBar_app_right .svg-icon', styleObjectIcon);
        },
        convertAttrToStyleObject() {
            this.convertAttrToStyleObjectContent()
            var styleObject = {};
            if (this.propData.bgSize && this.propData.bgSize == "custom") {
                styleObject["background-size"] = (this.propData.bgSizeWidth ? this.propData.bgSizeWidth.inputVal + this.propData.bgSizeWidth.selectVal : "auto") + " " + (this.propData.bgSizeHeight ? this.propData.bgSizeHeight.inputVal + this.propData.bgSizeHeight.selectVal : "auto")
            } else if (this.propData.bgSize) {
                styleObject["background-size"] = this.propData.bgSize;
            }
            if (this.propData.positionX && this.propData.positionX.inputVal) {
                styleObject["background-position-x"] = this.propData.positionX.inputVal + this.propData.positionX.selectVal;
            }
            if (this.propData.positionY && this.propData.positionY.inputVal) {
                styleObject["background-position-y"] = this.propData.positionY.inputVal + this.propData.positionY.selectVal;
            }
            for (const key in this.propData) {
                if (this.propData.hasOwnProperty.call(this.propData, key)) {
                    const element = this.propData[key];
                    if (!element && element !== false && element != 0) {
                        continue;
                    }
                    switch (key) {
                        case "width":
                        case "height":
                            styleObject[key] = element;
                            break;
                        case "bgColor":
                            if (element && element.hex8) {
                                styleObject["background-color"] = element.hex8;
                            }
                            break;
                        case "box":
                            if (element.marginTopVal) {
                                styleObject["margin-top"] = `${element.marginTopVal}`;
                            }
                            if (element.marginRightVal) {
                                styleObject["margin-right"] = `${element.marginRightVal}`;
                            }
                            if (element.marginBottomVal) {
                                styleObject["margin-bottom"] = `${element.marginBottomVal}`;
                            }
                            if (element.marginLeftVal) {
                                styleObject["margin-left"] = `${element.marginLeftVal}`;
                            }
                            if (element.paddingTopVal) {
                                styleObject["padding-top"] = `${element.paddingTopVal}`;
                            }
                            if (element.paddingRightVal) {
                                styleObject["padding-right"] = `${element.paddingRightVal}`;
                            }
                            if (element.paddingBottomVal) {
                                styleObject["padding-bottom"] = `${element.paddingBottomVal}`;
                            }
                            if (element.paddingLeftVal) {
                                styleObject["padding-left"] = `${element.paddingLeftVal}`;
                            }
                            break;
                        case "bgImgUrl":
                            styleObject["background-image"] = `url(${window.IDM.url.getWebPath(element)})`;
                            break;
                        case "positionX":
                            //??????????????????

                            break;
                        case "positionY":
                            //??????????????????

                            break;
                        case "bgRepeat":
                            //????????????
                            styleObject["background-repeat"] = element;
                            break;
                        case "bgAttachment":
                            //????????????
                            styleObject["background-attachment"] = element;
                            break;
                        case "border":
                            if (element.border.top.width > 0) {
                                styleObject["border-top-width"] = element.border.top.width + element.border.top.widthUnit;
                                styleObject["border-top-style"] = element.border.top.style;
                                if (element.border.top.colors.hex8) {
                                    styleObject["border-top-color"] = element.border.top.colors.hex8;
                                }
                            }
                            if (element.border.right.width > 0) {
                                styleObject["border-right-width"] = element.border.right.width + element.border.right.widthUnit;
                                styleObject["border-right-style"] = element.border.right.style;
                                if (element.border.right.colors.hex8) {
                                    styleObject["border-right-color"] = element.border.right.colors.hex8;
                                }
                            }
                            if (element.border.bottom.width > 0) {
                                styleObject["border-bottom-width"] = element.border.bottom.width + element.border.bottom.widthUnit;
                                styleObject["border-bottom-style"] = element.border.bottom.style;
                                if (element.border.bottom.colors.hex8) {
                                    styleObject["border-bottom-color"] = element.border.bottom.colors.hex8;
                                }
                            }
                            if (element.border.left.width > 0) {
                                styleObject["border-left-width"] = element.border.left.width + element.border.left.widthUnit;
                                styleObject["border-left-style"] = element.border.left.style;
                                if (element.border.left.colors.hex8) {
                                    styleObject["border-left-color"] = element.border.left.colors.hex8;
                                }
                            }

                            styleObject["border-top-left-radius"] = element.radius.leftTop.radius + element.radius.leftTop.radiusUnit;
                            styleObject["border-top-right-radius"] = element.radius.rightTop.radius + element.radius.rightTop.radiusUnit;
                            styleObject["border-bottom-left-radius"] = element.radius.leftBottom.radius + element.radius.leftBottom.radiusUnit;
                            styleObject["border-bottom-right-radius"] = element.radius.rightBottom.radius + element.radius.rightBottom.radiusUnit;
                            break;
                        case "font":
                            IDM.style.setFontStyle(styleObject, element)
                            this.adaptiveFontSize(styleObject, element)
                            break;
                        case 'boxShadow':
                            styleObject['box-shadow'] = element;
                        break;
                    }
                }
            }
            window.IDM.setStyleToPageHead(this.moduleObject.id, styleObject);
        },
        /**
         * ?????????url????????????
         * ???????????????url????????????
         */
        commonParam() {
            let urlObject = IDM.url.queryObject();
            var params = {
                pageId: window.IDM.broadcast && window.IDM.broadcast.pageModule ? window.IDM.broadcast.pageModule.id : "",
                urlData: JSON.stringify(urlObject),
            };
            return params;
        },
        /**
         * ????????????
         */
        reload() {
            //???????????????
            this.getMenuData();
        },
        
        /**
         * ??????????????????????????????????????????
         */
        getExpressData(dataName, dataFiled, resultData) {
            //???defaultValue??????dataFiled??????
            var _defaultVal = undefined;
            if (dataFiled) {
                var filedExp = dataFiled;
                filedExp = dataName + (filedExp.startsWiths("[") ? "" : ".") + filedExp;
                var dataObject = { IDM: window.IDM };
                dataObject[dataName] = resultData;
                _defaultVal = window.IDM.express.replace.call( this, "@[" + filedExp + "]", dataObject );
            }
            //????????????????????????????????????
            if (this.propData.customFunction && this.propData.customFunction.length > 0) {
                var params = this.commonParam();
                var resValue = "";
                try {
                    resValue = window[this.propData.customFunction[0].name] && window[this.propData.customFunction[0].name].call(this, {
                        ...params,
                        ...this.propData.customFunction[0].param,
                        moduleObject: this.moduleObject,
                        expressData: _defaultVal, interfaceData: resultData
                    });
                } catch (error) {
                }
                _defaultVal = resValue;
            }

            return _defaultVal;
        },
        /**
         * ????????????????????????????????????
         * @param {
         *  type:"???????????????????????????????????????????????????????????????????????????????????????????????????type???linkageResult?????????????????????????????????linkageDemand?????????????????????????????????linkageReload??????????????????????????????
         * ???linkageOpenDialog?????????????????????linkageCloseDialog?????????????????????linkageShowModule?????????????????????linkageHideModule?????????????????????linkageResetDefaultValue?????????????????????"
         *  message:{??????????????????????????????????????????}
         *  messageKey:"???????????????key?????????????????????????????????????????????????????????????????????????????????key?????????????????????"
         *  isAcross:?????????true?????????????????????????????????????????????????????????false
         * } object 
         */
        receiveBroadcastMessage(object) {
            console.log("??????????????????", object)
        },
        /**
         * ????????????????????????????????????
         * @param {
         *  type:"?????????????????????????????????type???linkageResult?????????????????????????????????linkageDemand?????????????????????????????????linkageReload??????????????????????????????
         * ???linkageOpenDialog?????????????????????linkageCloseDialog?????????????????????linkageShowModule?????????????????????linkageHideModule?????????????????????linkageResetDefaultValue?????????????????????",
         *  message:{?????????????????????},
         *  rangeModule:"???????????????????????????????????????????????????????????????????????????????????????packageid?????????????????????????????????????????? this.$root.propData.compositeAttr["attrKey"]?????????attrKey?????????????????????bindKey???,?????????????????????['1','2']"",
         *  className:"???????????????????????????????????????????????????????????????????????????????????????"
         *  globalSend:?????????true????????????????????????????????????rangeModule?????????????????????className?????????????????????false
         * } object 
         */
        sendBroadcastMessage(object) {
            window.IDM.broadcast && window.IDM.broadcast.send(object);
        },
        /**
         * ????????????????????????????????????????????????
         * @param {
         *  type:"?????????????????????????????????pageCommonInterface????????????????????????????????????"
         *  key:"??????key?????????????????????????????????????????????????????????????????????????????????????????????"
         *  data:"????????????????????????????????? or ?????? or ??????"
         * }
         */
        setContextValue(object) {
            if (object.type != "pageCommonInterface") {
                return;
            }
            //??????????????????????????????????????????????????????????????????????????????????????????????????????
            if (object.key == this.propData.dataName) {
                // this.propData.fontContent = this.getExpressData(this.propData.dataName,this.propData.dataFiled,object.data);
                this.$set(this.propData,"fontContent",this.getExpressData(this.propData.dataName,this.propData.dataFiled,object.data));
            }
        }
    }
}
</script>
<style lang="scss">
.IHeaderBar_app{
    .IHeaderBar_app_left{
        img{
            width: 232px;
            height: 40px;
        }
    }
    
}
.idm-navbar-bottom-animate {
    overflow: hidden;
    height: 80px !important;
    transition: height 0.2s;
}
.idm-navbar-bottom-fix {
    position: fixed;
    left: 0;
    width: 100%;
}

</style>