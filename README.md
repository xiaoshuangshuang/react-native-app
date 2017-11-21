# react-native开发中遇见的问题以及解决方案
1. 常见的造成react native闪退的原因   
  * lineHeight不是整数，造成ios闪退。使用Math.round将lineHeight转换为整数   
  * 点击数字时，会发生闪退 -- 将数字转换为字符串 String(num)   
  * 如果程序报pthread_create failed: couldn't allocate 1069056-bytes mapped space: Out of memory    
  在Manifest.xml文件中添加\<application android:largeHeap="true">
2. 父子组件传值问题   
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
3. 当页面上有键盘时，当点击TouchableHighlight,第一次会收起键盘而不会触发方法（scrollview)    
   解决方案1:设置scrollView的keyboardShouldPersistTaps为handled,当触发其他操作前先关闭键盘   
   ``` react native    
   <ScrollView keyboardShouldPersistTaps="handled">   
     <TouchableHighlight onPress={()=>keybord.dismiss()}>    
     </TouchableHighlight>    
    </ScrollView>   
    ```   
    解决方案2:   
    ``` react native   
    <View onStartShouldSetResponderCapture={this._onStartShouldSetResponderCapture.bind(this)}>   
     <TextInput ref="searchButton"/>   
    </View>   
    _onStartShouldSetResponderCapture(){   
     const target = e.nativeEvent.target;   
     this.refs.searchButton.blur();   
    }   
    ```
  
    
