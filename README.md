# react-native
### 1. 常见的造成react native闪退的原因   
  * lineHeight不是整数，造成ios闪退。使用Math.round将lineHeight转换为整数   
  * 点击数字时，会发生闪退 -- 将数字转换为字符串 String(num)   
  * 如果程序报pthread_create failed: couldn't allocate 1069056-bytes mapped space: Out of memory    
  在Manifest.xml文件中添加\<application android:largeHeap="true">
### 2. 父子组件传值问题   
  * 父组件向子组件传值   
    a. 通过props向子组件传值   
    通常情况下，子组件第一次接收到props并且渲染成功后，若props改变，此子组件也不会渲染，可以使用   
    componentWillReceiveProps方法来接收改变的props值，即nextProps    
      ``` javascript
      componentWillReceiveProps(nextProps){    
        this.setState({    
          no:nextProps.no    
        })   
      }
      ```
    b. 子组件向父组件传递信息
      通常情况下，可使用事件触发。   
      也可以采用通过触发父组件通过props传递的方法   
### 3. 当页面上有键盘时，当点击TouchableHighlight,第一次会收起键盘而不会触发方法（scrollview)    
   解决方案1:设置scrollView的keyboardShouldPersistTaps为handled,当触发其他操作前先关闭键盘    
   ``` react ntive    
   <ScrollView keyboardShouldPersistTaps="handled">   
     <TouchableHighlight onPress={()=>keybord.dismiss()}>    
     </TouchableHighlight>    
   </ScrollView>    
   ```    
   解决方案2:    
   ``` react ntive    
   <View onStartShouldSetResponderCapture={this._onStartShouldSetResponderCapture.bind(this)}>   
    <TextInput ref="searchButton"/>   
   </View>    
   _onStartShouldSetResponderCapture(){    
    const target = e.nativeEvent.target;    
    this.refs.searchButton.blur();    
   }    
   ```    
### 4. takeSnapshot生成图片的相关问题    
   ``` react native    
   var ReactNative = require('react-native');    
   ReactNative.takeSnapshot(this.refs.location, {format: 'png', quality: 1}).then(    
    ()=>{this.setState({uri:uri})     
   ).catch(    
    ()=>{}    
   )     
   ```    
   如果采用上述调用方式，程序会报 ReactNative.findNodeHandle is not a function错误    
   解决方法:    
   ``` react native     
   import { NativeModules, findNodeHandle } from "react-native";    
   const { RNViewShot } = NativeModules;    
   export const dirs = {     
    // cross platform    
    CacheDir: RNViewShot.CacheDir,    
    DocumentDir: RNViewShot.DocumentDir,    
    MainBundleDir: RNViewShot.MainBundleDir,    
    MovieDir: RNViewShot.MovieDir,    
    MusicDir: RNViewShot.MusicDir,    
    PictureDir: RNViewShot.PictureDir,    
    // only Android    
    DCIMDir: RNViewShot.DCIMDir,     
    DownloadDir: RNViewShot.DownloadDir,    
    RingtoneDir: RNViewShot.RingtoneDir,    
    SDCardDir: RNViewShot.SDCardDir,    
   };     
   export function takeSnapshot(     
    view: number | ReactElement<any>,    
    options?: {    
    width?: number,    
    height?: number,    
    path?: string,    
    format?: "png" | "jpg" | "jpeg" | "webm",    
    quality?: number,    
    result?: "file" | "base64" | "data-uri",     
    snapshotContentContainer?: bool     
   } = {}): Promise<string> {     
    if (typeof view !== "number") {    
     const node = findNodeHandle(view);    
     if (!node) return Promise.reject(new Error("findNodeHandle failed to resolve view="+String(view)));    
     view = node;     
    }     
    return RNViewShot.takeSnapshot(view, options);     
   }    
   export default { takeSnapshot, dirs };    
   ```      
   注意：当需要生成的内容被scrollview包裹时，而scrollView设置flex:1时，可能会造成截屏的效果。
### 5. navigator动画卡顿
   解决方案:当页面切换动画完成后再获取数据并渲染    
   ``` react native    
   this.dealForward=this.props.navigator.navigationContext.addListener('didfocus',(newRouter)=>{    
    this.fetchData();    
    this.dealForward.remove();    
   })    
   ```    
### 6. 复制粘贴的内容带样式    
   解决方案：可以把粘贴板的内容拿出来再放进去    
   ``` react native    
   async _setClipboardContent() {    
    try {    
     var content =await Clipboard.getString();   
     Clipboard.setString(content);    
    } catch(e) {    
     this.setState({ content: e.message });   
    }    
   }    
  ```    
### 7. navigator同级目录只记录两级    
### 8. ios上listView 或scrollView 的属性removeClippedSubviews设置为true，render的时候，listView或scrollView不显示，需要碰/滑一下才会显示。    
   解决方案： ios上设置removeClippedSubviews为false    
