<template>
	<view>
		<!--
		<uni-nav-bar right-text=". . ." title="普通话单字拼音练习" @clickRight="popupMenuValue=!popupMenuValue" backgroundColor="#EBEBEB"/>
		-->
		<chunLei-popups v-model="popupMenuValue" :popData="popupMenuData" @tapPopup="tapPopup" :x="347" :y="0" placement="top-end"></chunLei-popups>
		<view class="mainContainer">
			<view class="pinyinDiaoMainContainer">
				<view class="pinyinDiaoLabelContainer" 
				      :style="{ width: screenWidth/3 - 4 + 'px', opacity: wordsArr[wordsArrIndex].pinyinArr.length >= 2 ? 1 : 0}">
					<view class="pinyinDiaoLabel">
						<text class="pinyinTextStyle">拼音</text>
						<text class="diaoTextStyle">音调</text>
					</view>
					<view class="pinyinDiaoInputBlock">
						<view class="pinyinInputStyle">
							<input style="padding: 5px; font-size: 16px; border: 1px #333333 solid; border-radius: 7px"  
								   maxlength="6"
								   v-model="wordsArr[wordsArrIndex].inputPinyinArr[1]" 
								   :disabled = "wordsArr[wordsArrIndex].isShowAnswer"
								   :style="{ backgroundColor: wordsArr[wordsArrIndex].isShowAnswer ? '#F0F0F0' : '',
											 borderColor: wordsArr[wordsArrIndex].isShowAnswer ? wordsArr[wordsArrIndex].inputCorrectedColorArr[1] : '#333333' }"
								   @input="changeInputHandler1"/>
						</view>
						<view class="diaoInputStyle">
							<xfl-select
								ref="select1"
								:list="diaoList"
								:clearable="false"
								:showItemNum="5"
								:listShow="false"
								:isCanInput="false"
								:style_Container="'width: 30px; height: 34px; font-size: 16px; text-align: center;'"
								:initValue="'0'"
								:disabled="wordsArr[wordsArrIndex].isShowAnswer ? true : false"
								:selectHideType="'hideAll'"
								@change="diaoChangeHandler1"
							/>
						</view>
					</view>
				</view>
				<view class="pinyinDiaoLabelContainer" :style="{ width: screenWidth/3 - 4 + 'px' }">
					<view class="pinyinDiaoLabel">
						<text class="pinyinTextStyle">拼音</text>
						<text class="diaoTextStyle">音调</text>
					</view>
					<view class="pinyinDiaoInputBlock">
						<view class="pinyinInputStyle">
							<input style="padding: 5px; font-size: 16px; border: 1px #333333 solid; border-radius: 7px" 
								   maxlength="6" 
								   v-model="wordsArr[wordsArrIndex].inputPinyinArr[0]" 
								   :disabled = "wordsArr[wordsArrIndex].isShowAnswer"
								   :style="{ backgroundColor: wordsArr[wordsArrIndex].isShowAnswer ? '#F0F0F0' : '',
											 borderColor: wordsArr[wordsArrIndex].isShowAnswer ? wordsArr[wordsArrIndex].inputCorrectedColorArr[0] : '#333333' }"
								   @input="changeInputHandler0"/>
						</view>
						<view class="diaoInputStyle">
							<xfl-select
								ref="select0"
								:list="diaoList"
								:clearable="false"
								:showItemNum="5"
								:listShow="false"
								:isCanInput="false"  
								:style_Container="'width: 30px; height: 34px; font-size: 16px; text-align: center;'"
								:initValue="'0'"
								:disabled="wordsArr[wordsArrIndex].isShowAnswer ? true : false"
								:style="{ backgroundColor: wordsArr[wordsArrIndex].isShowAnswer ? '#F0F0F0' : '' }"
								:selectHideType="'hideAll'"
								@change="diaoChangeHandler0"
							/>
						</view>
					</view>
				</view>
				<view class="pinyinDiaoLabelContainer" 
				      :style="{ width: screenWidth/3 - 4 + 'px', opacity: wordsArr[wordsArrIndex].pinyinArr.length == 3 ? 1 : 0 }">
					<view class="pinyinDiaoLabel">
						<text class="pinyinTextStyle">拼音</text>
						<text class="diaoTextStyle">音调</text>
					</view>
					<view class="pinyinDiaoInputBlock">
						<view class="pinyinInputStyle">
							<input style="padding: 5px; font-size: 16px; border: 1px #333333 solid; border-radius: 7px"
								   maxlength="6" 
								   v-model="wordsArr[wordsArrIndex].inputPinyinArr[2]" 
								   :disabled = "wordsArr[wordsArrIndex].isShowAnswer"
								   :style="{ backgroundColor: wordsArr[wordsArrIndex].isShowAnswer ? '#F0F0F0' : '',
											 borderColor: wordsArr[wordsArrIndex].isShowAnswer ? wordsArr[wordsArrIndex].inputCorrectedColorArr[2] : '#333333' }"
								   @input="changeInputHandler2"/>
						</view>
						<view class="diaoInputStyle">
							<xfl-select
								ref="select2"
								:list="diaoList"
								:clearable="false"
								:showItemNum="5"
								:listShow="false"
								:isCanInput="false"  
								:style_Container="'width: 30px; height: 34px; font-size: 16px; text-align: center;'"
								:initValue="'0'"
								:disabled="wordsArr[wordsArrIndex].isShowAnswer ? true : false"
								:selectHideType="'hideAll'"
								@change="diaoChangeHandler2"
							/>
						</view>
					</view>
				</view>
			</view>
			<view class="buttonsContainer">
				<view class="viewButtonStyle" style="background-color: yellow; border-radius: 8px;" @tap="prevWord">
					<text class="viewButtonText">
						上一个字
					</text>
				</view>
				<view class="viewButtonStyle" style="background-color: lightgreen; border-radius: 25px" 
				      :style="{ opacity: wordsArr[wordsArrIndex].isShowAnswer ? 0 : 1}" @tap="submitAnswer">
					<text class="viewButtonText">
						提交答案
					</text>
				</view>
				<view class="viewButtonStyle" style="background-color: yellow; border-radius: 8px" @tap="nextWord">
					<text class="viewButtonText">
						下一个字
					</text>
				</view>
			</view>
			<view class="showingWordContainer">
				<view class="clickedButtonStyle">
					<view class="addingCustomArrButtonStyle" 
					      :style="{ backgroundColor: isUsingCustomWordsArr ? 'orangered' : 'violet' }" @tap="addOrDeleteFromCustomArr">
						<text class="addingCustomArrButtonText">
							{{isUsingCustomWordsArr ? '从自定义列中移除' : '加到自定义生字列'}}
						</text>
					</view>
				</view>
				<view>
					<text style="font-size: 100px;" :style="{ color: wordsArr[wordsArrIndex].showAnswerWordColor }" >
						{{wordsArr[wordsArrIndex].word}}
					</text>
				</view>
				<view class="showedAnwserWordContainer">
					<view class="clickedButtonStyle">
						<view v-show="!wordsArr[wordsArrIndex].isShowAnswer"
							  :style="{ opacity: wordsArr[wordsArrIndex].showAnswerWordColor == 'green' ? 0 : 1 }"
							  class="checkAnswerButtonStyle" @tap="showAnswer">
							<text class="viewButtonText">
								查看答案
							</text>
						</view>
					</view>
					<view v-show="wordsArr[wordsArrIndex].isShowAnswer">
						<view class="showedAnwserWordStyle" v-for="(answer, i) in wordsArr[wordsArrIndex].pinyinArr" :key="i">
							<text>
								答案{{i+1}}: {{answer}}
							</text>
						</view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	const deviceH = uni.getSystemInfoSync().screenHeight
	const deviceW = uni.getSystemInfoSync().screenWidth
	 
	const basePx = 375
	 
	function px2dp(px) { 
	    return px *  deviceW / basePx 
	}
	
	import xflSelect from '../../components/xfl-select/xfl-select.vue'
	import hanziData from '../../hanzi_dict.js';
	
	export default {
		data() {
			return {
				arr: [],
				wordsArr: [],
				wordsArrIndex: 0,
				customWordsArr: [],
				customWordsArrIndex: 0,
				isUsingCustomWordsArr: false,
				tempArr: [],
				tempArrIndex: 0,
				popupMenuValue:false,
				popupMenuData:[
					{ title: '查看自定义生字列' },
					{ title: '' },
					{ title: '重新开始此生字列' },
					{ title: '' },
					{ title: '重新打乱此生字列' }
				],
				diaoList: ['0','1','2','3','4'],
				screenWidth: px2dp(deviceW)
			}
		},
		components: { xflSelect },
		methods: {
			onNavigationBarButtonTap : function (e) {
				this.popupMenuValue = !this.popupMenuValue
			},
		   onLoad() {
		   		//console.log('onLoad');  // 页面加载
				try {
				  this.isUsingCustomWordsArr = uni.getStorageSync("isUsingCustomWordsArr")
				  if(this.isUsingCustomWordsArr === undefined || 
					 this.isUsingCustomWordsArr === null ||
					 this.isUsingCustomWordsArr === ''){
					this.isUsingCustomWordsArr = false
					let hanziDict = hanziData.hanziDict
					let arr = []
					for(let word in hanziDict){
						arr.push({
							word: word, 
							pinyinArr: hanziDict[word], 
							isShowAnswer: false, 
							showAnswerWordColor: 'black', 
							inputPinyinArr: ["","",""],
							inputDiaoArr: ["0","0","0"],
							inputCorrectedColorArr: ['black', 'black', 'black'],
						})
					}
					this.wordsArr = arr
					this.saveToStorage()
				  } else {
					  if(this.isUsingCustomWordsArr) {
						  this.tempArr = uni.getStorageSync("wordsArr")
						  this.tempArrIndex = uni.getStorageSync("wordsArrIndex")	
						  this.wordsArr = uni.getStorageSync("customWordsArr")
						  this.wordsArrIndex = uni.getStorageSync("customWordsArrIndex")
						  this.popupMenuData[0] = { title: '切回主要的生字列' }
					  } else {
						  this.wordsArr = uni.getStorageSync("wordsArr")
						  this.wordsArrIndex = uni.getStorageSync("wordsArrIndex")	
						  this.tempArr = uni.getStorageSync("customWordsArr")
						  this.tempArrIndex = uni.getStorageSync("customWordsArrIndex")
						  this.popupMenuData[0] = { title: '查看自定义生字列' }
					  }
					  this.tempArr = uni.getStorageSync("tempArr")
					  this.tempArrIndex = uni.getStorageSync("tempArrIndex")
				  }
				} catch (e) {
				  this.showToast("Program Error!")
				}
		   	},
			/*
		   	onShow() {
		   		console.log('onShow');  // 页面显示
		   	},
		   	onReady() {
		   		console.log('onReady');  // 页面初次渲染完成
				//console.log(this.wordsArr);
		   	},
		   	onHide() {
		   		console.log('onHide');  // 页面隐藏
		   	},
		   	onUnload(){
		   		console.log('onUnload');  // 页面卸载
		   	},
			*/
			showToast(message) {
				uni.showToast({
					title: message,
					icon: 'none',
					position: 'bottom'
				})
			},
			tapPopup(e){
				if(e.title == '查看自定义生字列'){
					if(this.customWordsArr.length === 0){
						this.showToast("自定义生字列沒有生字")
					} else {
						this.isUsingCustomWordsArr = true
						this.popupMenuData[0] = { title: '切回主要的生字列' }	
						this.tempArr = this.wordsArr
						this.tempArrIndex = this.wordsArrIndex
						this.wordsArr = this.customWordsArr
						this.wordsArrIndex = this.customWordsArrIndex
						this.setSelectValues(this.wordsArrIndex)
					}
					this.saveToStorage()
				} else if(e.title == '切回主要的生字列'){
					this.isUsingCustomWordsArr = false
					this.popupMenuData[0] = { title: '查看自定义生字列' }
					this.customWordsArr = this.wordsArr
					this.customWordsArrIndex = this.wordsArrIndex
					this.wordsArr = this.tempArr
					this.wordsArrIndex = this.tempArrIndex
					this.setSelectValues(this.wordsArrIndex)
					this.saveToStorage()
				} else if(e.title == '重新开始此生字列') {
					var that = this
					uni.showModal({
						title: '',
						content: '确定是否重新开始',
						success: function (res) {
							if (res.confirm) {
								let newWordsArr = that.wordsArr
								for(let i in newWordsArr) {
									newWordsArr[i].isShowAnswer = false
									newWordsArr[i].showAnswerWordColor = 'black'
									newWordsArr[i].inputPinyinArr = ["","",""]
									newWordsArr[i].inputDiaoArr = ["0","0","0"],
									newWordsArr[i].inputCorrectedColorArr = ['black', 'black', 'black']
									that.setSelectValues(i)
								}
								that.wordsArr = newWordsArr
								that.wordsArrIndex = 0
								that.saveToStorage()
							} else if (res.cancel) {
								console.log('取消重新开始');
							}
						}
					});
				} else if(e.title == '重新打乱此生字列') {
					var that = this
					uni.showModal({
						title: '',
						content: '确定是否重新打乱列表\n(此操作无法复原)',
						success: function (res) {
							if (res.confirm) {
								let newWordsArr = that.wordsArr
								newWordsArr.sort(function() { return .5 - Math.random(); })
								that.wordsArr = newWordsArr
								that.wordsArrIndex = 0
								that.saveToStorage()
							} else if (res.cancel) {
								console.log('取消重新打乱列表');
							}
						}
					})
				}
			},
			changeInputHandler1(e){
				this.wordsArr[this.wordsArrIndex].inputPinyinArr[1] = (e.detail.value).toLowerCase()
			},
			diaoChangeHandler1({newVal, oldVal, index, orignItem}){
				this.wordsArr[this.wordsArrIndex].inputDiaoArr[1] = newVal
			},
			changeInputHandler0(e){
				this.wordsArr[this.wordsArrIndex].inputPinyinArr[0] = (e.detail.value).toLowerCase()
			},
			diaoChangeHandler0({newVal, oldVal, index, orignItem}){
				this.wordsArr[this.wordsArrIndex].inputDiaoArr[0] = newVal
			},
			changeInputHandler2(e){
				this.wordsArr[this.wordsArrIndex].inputPinyinArr[2] = (e.detail.value).toLowerCase()
			},
			diaoChangeHandler2({newVal, oldVal, index, orignItem}){
				this.wordsArr[this.wordsArrIndex].inputDiaoArr[2] = newVal
			},
			prevWord(){
				if(this.wordsArrIndex <= 0) {
					this.showToast('已经是第一个单字')
				}
				else {
					this.setSelectValues(--this.wordsArrIndex)
					this.saveToStorage()
				}
			},
			submitAnswer(){
				let answers = []
				for(let word of this.wordsArr[this.wordsArrIndex].pinyinArr) {
					answers.push({pinyinDiao: word, correctedAns: false})
				}
				this.wordsArr[this.wordsArrIndex].inputCorrectedColorArr = ['black', 'black', 'black']
				for(let i in answers){
					let isGetCorrectedAnswer = false
					for(let j in answers){
						if(this.wordsArr[this.wordsArrIndex].inputPinyinArr[i] + this.wordsArr[this.wordsArrIndex].inputDiaoArr[i] 
								== answers[j].pinyinDiao && !answers[j].correctedAns) {
							answers[j].correctedAns = true
							isGetCorrectedAnswer = true
							break
						}
					}
					if(!isGetCorrectedAnswer) {
						this.wordsArr[this.wordsArrIndex].inputCorrectedColorArr[i] = 'red'
					}
				}
				let isAllCorrect = true
				for(let ans of answers) if(ans.correctedAns == false) isAllCorrect = false
				if(isAllCorrect) {
					this.wordsArr[this.wordsArrIndex].showAnswerWordColor = 'green'
				}
				else {
					this.wordsArr[this.wordsArrIndex].showAnswerWordColor = 'red'

					if(answers.length == 1) {
						this.showToast('答案不正确，请重新输入')
					} else {
						this.showToast('一个或多个答案不正确，请重新输入')
					}
				}
				this.saveToStorage()
			},
			nextWord(){
				if(this.wordsArrIndex >= this.wordsArr.length-1) {
					this.showToast('已经是最后一个单字')
				}
				else {
					this.setSelectValues(++this.wordsArrIndex)
					this.saveToStorage()
				}
			},
			showAnswer(){
				if(this.wordsArr[this.wordsArrIndex].showAnswerWordColor != 'green')
					this.wordsArr[this.wordsArrIndex].isShowAnswer = true
				this.saveToStorage()
			},
			addOrDeleteFromCustomArr() {
				if(!this.isUsingCustomWordsArr){
					this.customWordsArr.push({word: this.wordsArr[this.wordsArrIndex].word,
										      pinyinArr: this.wordsArr[this.wordsArrIndex].pinyinArr,
										      isShowAnswer: false,
										      showAnswerWordColor: 'black',
										      inputPinyinArr: ["","",""],
										      inputDiaoArr: ["0","0","0"],
										      inputCorrectedColorArr: ['black', 'black', 'black']})
					this.showToast("已添加'" + this.wordsArr[this.wordsArrIndex].word + "'到自定义生字列")
				}
				else {
					this.wordsArr.splice(this.wordsArrIndex, 1)
					if(this.wordsArrIndex >= this.wordsArr.length) this.wordsArrIndex = this.wordsArr.length - 1
					if(this.wordsArr.length === 0){
						this.isUsingCustomWordsArr = false
						this.popupMenuData[0] = { title: '查看自定义生字列' }
						this.showToast("生字列沒有生字，自动切回到主要的生字列")
						this.wordsArr = this.tempArr
						this.wordsArrIndex = this.tempArrIndex
						this.setSelectValues(this.wordsArrIndex)
					}
				}
				this.saveToStorage()
			},
			setSelectValues(index) {
				this.$refs.select1.setInput(this.wordsArr[index].inputDiaoArr[1])
				this.$refs.select0.setInput(this.wordsArr[index].inputDiaoArr[0])
				this.$refs.select2.setInput(this.wordsArr[index].inputDiaoArr[2])
			},
			saveToStorage() {
				if(this.isUsingCustomWordsArr){
					uni.setStorageSync("wordsArr", this.tempArr)
					uni.setStorageSync("wordsArrIndex", this.tempArrIndex)	
					uni.setStorageSync("customWordsArr", this.wordsArr)
					uni.setStorageSync("customWordsArrIndex", this.wordsArrIndex)
					uni.setStorageSync("tempArr", this.tempArr)
					uni.setStorageSync("tempArrIndex", this.tempArrIndex)
				} else {
					uni.setStorageSync("wordsArr", this.wordsArr)
					uni.setStorageSync("wordsArrIndex", this.wordsArrIndex)	
					uni.setStorageSync("customWordsArr", this.tempArr)
					uni.setStorageSync("customWordsArrIndex", this.tempArrIndex)
					uni.setStorageSync("tempArr", this.tempArr)
					uni.setStorageSync("tempArrIndex", this.tempArrIndex)
				}
				uni.setStorageSync("isUsingCustomWordsArr", this.isUsingCustomWordsArr)
			}
		}
	}
</script>

<style>
	.mainContainer {
		display: flex;
        flex-direction: column;
        text-align: center;
        align-items: center;
		margin-left: 3px;
		margin-right: 3px;
    }
    .pinyinDiaoMainContainer {
		width: 100%;
		display: flex;
        flex-direction: row;
        justify-content: space-around;
        margin-top: 5px;
    }
    .pinyinDiaoLabelContainer {
        flex-direction: column;
    }
    .pinyinDiaoLabel {
        flex-direction: row;
    }
    .pinyinTextStyle {
        font-size: 18px;
        margin-left: 5rpx;
        margin-right: 40rpx;
    }
    .diaoTextStyle {
        font-size: 18px;
    }
    .pinyinDiaoInputBlock {
		display: flex;
        flex-direction: row;
        margin-top: 5px;
    }
    .pinyinInputStyle {
        width: 110px;
        margin-left: 1px;
        margin-right: 2px;
        text-align: center;
    }
    .diaoInputStyle {
        border-width: 1px;
        width: 50px;
        font-size: 18px;
        text-align: center;
		margin-right: 5px;
    }
    .buttonsContainer {
		display: flex;
        flex-direction: row;
        justify-content: space-around;
        margin-top: 5px;
    }
	.buttonsContainer :active {
		opacity: 0.7;
	}
    .viewButtonStyle {
        width: 100px;
        height: 30px;
		border: 1px #000000 solid;
		margin: 0 6px 0 6px;
		line-height: 27px;
    }
    .hiddenViewButtonStyle {
        width: 100px;
        height: 40px;
    }
    .viewButtonText {
        text-align: center;
        font-size: 20px;
    }
	.showingWordContainer {
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		align-items: center;
	}
	.addingCustomArrButtonStyle {
		width: 120px;
		height: 30px;
		margin: 0 2px 0 2px;
		line-height: 27px;
		border: 1px #000000 solid;
		border-radius: 25px;
	}
	.addingCustomArrButtonText {
		font-size: 14px;
		text-align: center;
	}
	.clickedButtonStyle :active {
		opacity: 0.7;
	}
    .showedAnwserButtonContainer {
        margin-left: 10px;
        margin-top: 45px;
        align-items: center;
        justify-content: center;
    }
    .showedAnwserButtonStyle {
        width: 100px;
        height: 40px;
        border-radius: 10px;
        justify-content: center;
        align-items: center;
        background-color: lightblue;
    }
    .showedAnswerButtonTextStyle {
        text-align: center;
        font-size: 20px;
    }
    .showedAnwserWordContainer {
		display: flex;
        flex-direction: column;
        justify-content: space-around;
		width: 120px;
		margin-left: 3px;
    }
	.checkAnswerButtonStyle {
		background-color: lightblue; 
		border-radius: 25px;
		border: 1px #000000 solid;
	}
    .showedAnwserWordStyle {
        font-size: 16px;
		width: 120px;
		margin-left: 5px;
		margin-bottom: 5px;
        justify-content: center;
        align-items: center;
		text-align: left;
    }
	
</style>
