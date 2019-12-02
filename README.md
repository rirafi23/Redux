# Redux
Redux adalah pustaka JavaScript open-source untuk mengelola state aplikasi. Sebelumnya apabila kita ingin membuat state, di file satu.js misalnya kita tidak akan bisa memanggil state tersebut ke file dua.js dengan nilai yang sama persis. hal ini dikarenakan state hanya bisa dilakukan di file itu sendiri. dengan adanya Redux Kita cuma membuat satu state tapi bisa di panggil di semua screen dengan value yang sama.
## Kosnsep Redux
Redux memiliki konsep yaitu state yang berada di setiap komponen akan dimasukkan ke dalam store menjadi satu. tinggal semua state yang didalam store akan dipanggil kembali ke setiap komponen-komponen yang memerlukan state tersebut.
## ![redux](https://user-images.githubusercontent.com/52732798/69915451-8d2b8e00-1481-11ea-986a-0ba681ef8299.png) 
jadi di redux memiliki beberapa component diantarannya
+ action

konsep action cukup Sederhana jadi action merupakan sebuah object yang memiliki property type. yang dimana object action ini nantinya akan dikirim ke Store, untuk kemudian nanti di olah oleh Reducer.
```
const changeUsername = {
    type: 'CHANGE_USERNAME',
    data: 'Pondok It'
}
```
Penjelasan : dari index.js yang kita buat tadi bisa terlihat bahwa action creator hanyalah fungsi yang mengembalikan nilai action. Action inilah yang akan di teruskan ke reducer untuk di olah oleh store.

+ Store

Store memegang seluruh state pada aplikasi Anda. Satu-satunya cara untuk mengubah keadaan di dalamnya adalah dengan mengirimkan tindakan padanya. Store bukan kelas. Ini hanya sebuah objek dengan beberapa metode di atasnya. 
Sangat mudah untuk membuat store jika Anda memiliki reducer. Di bagian sebelumnya, kami menggunakan combineReducers() untuk menggabungkan beberapa reducers menjadi satu.

```
//applyMiddleware untuk memperluas Redux dengan fungsionalitas khusus
//redux thunk untuk mengambil atau menyimpan data
const store={createStore(Reducer,{},applyMiddleware(Thunk))}
```

jadi fungsi applyMiddleware(Thunk) untuk memperluar pengambilan dan penyimpanan data 

+ reducer

Reducer menentukan bagaimana keadaan aplikasi berubah sebagai response terhadap actions yang dikirim ke store. Ingatlah bahwa actions hanya menggambarkan apa yang terjadi, tetapi jangan menggambarkan bagaimana keadaan aplikasi berubah.

```
import { combineReducers } from 'redux'
// import bisa di tambah dan juga diubah menurut types
import { CHANGE_NAME, CHANGE_UMUR, CHANGE_ALAMAT } from '../types/index'
 
 
//membuat state
const initialState = {
  name: '',
  umur: '',
  alamat: ''
}
 
//dibawah ini cuman manajement state
export const HomeReducer = (state = initialState, action) => {  
//membuat kondisi jika type sesuai dengan action.type yang telah kita buat difolder action maka akan mengembalikan action.payload / statenya itu sendiri
  switch (action.type) {
    case CHANGE_NAME:
      return { ...state, name: action.payload }
    case CHANGE_UMUR:
      return { ...state, umur: action.payload }
    case CHANGE_ALAMAT:
      return { ...state, alamat: action.payload }
    default:
      return state
  }
}
 
// supaya reducer bisa digunakan makan dipakailah combineReducers
export default combineReducers({
  HomeReducer
})
```
jadi si reducer ini memberikan respone action terhadap state yang di kirim ke store.
combineReducers ini berguna untuk membagi fungsi reducing menjadi terpisah, masing-masing mengelola statenya sendiri

itulah adalah perkenalan Tentangg redux. untuk bahan-bahan yang dibutuh ada di list bawah.

## installation Redux

> react-native init Redux

> npm install react-navigation --save

> npm install react-native-gesture-handler react-native-reanimated --save

> npm install react-navigation-stack --save

> npm install redux --save

> npm install react-redux --save

> npm install redux-thunk --save

#### kita mulai dengan membuat folder src/screen lalau buat file 

screenInput.js
```
import React from 'react'
import {View, ScrollView, Text, StyleSheet, TextInput, Dimensions, TouchableOpacity} from 'react-native'

//mengatur width Text
const {width:WIDTH} = Dimensions.get('window')
class Login extends React.Component{
    render(){
        return(
            <ScrollView>
            <View style={styles.Component}>
                
                <View style={styles.Card}>
                    <Text style={styles.TextTitle}>REDUX.</Text>
                    <Text style={styles.TextLogin}>username : </Text>
                    <TextInput
                        style={styles.TextInput}
                        placeholder="username"
                    />
                    <Text style={styles.TextLogin}>alamat : </Text>
                    <TextInput
                        style={styles.TextInput}
                        placeholder="alamat"
                    />
                    <Text style={styles.TextLogin}>hobby : </Text>
                    <TextInput
                        style={styles.TextInput}
                        placeholder="hobby"
                    />
                    <Text style={styles.TextLogin}>sekolah atau kantor : </Text>
                    <TextInput
                        style={styles.TextInput}
                        placeholder="sekolah atau kantor"
                    />
                    <TouchableOpacity style={styles.Touchable} activeOpacity={0.8} 
					onPress={()=>this.props.navigation.navigate('Home')}>
                        <Text style={styles.TextMyAccount}>NEXT</Text>
                    </TouchableOpacity>
                </View>
                
            </View>
            </ScrollView>
        )
    }
}
export default Login

const styles = StyleSheet.create({
    Component:{
        flex:1,
        width:'100%',
        height:600, //full screen '100%'
        justifyContent:'center',
        alignItems:'center'
    },
    Card:{
        width:'90%',
        paddingVertical:20,
        alignItems:'center',
        paddingHorizontal:10,
        backgroundColor:'#fff',
        elevation:20,
        borderBottomLeftRadius:30,
        borderTopEndRadius:30
    },
    TextInput:{
        width:'100%',
        height:50,
        borderBottomWidth:2,
        borderBottomColor:'#d3d3d3',
        paddingHorizontal:15,
    },
    TextLogin:{
        fontSize:15,
        marginTop:30,
        width: WIDTH - 80

    },
    TextTitle:{
        fontSize:20,
        fontWeight:'bold',
        width:WIDTH - 80
    },
    Touchable:{
        width:'80%',
        height:45,
        backgroundColor:'orange',
        justifyContent:'center',
        alignItems:'center',
        borderRadius:10,
        marginTop:30,
    },
    TextMyAccount:{
        fontSize:18,
        fontWeight:'bold',
        color:'#fff'
    }
})
```
screenTampil.js
```
import React from 'react'
import {View, Text, StyleSheet} from 'react-native'

class Home extends React.Component{
    render(){
        return(
            <View style={styles.Component}>
                <Text style={styles.TextTitle}>BIODATA PAKAI REDUX.</Text>
                <View style={styles.Card}>
                <Text style={styles.TextBio}>usernam : </Text>
                <Text style={styles.TextBio}>alamat : </Text>
                <Text style={styles.TextBio}>hobby : </Text>
                <Text style={styles.TextBio}>asal sekolah atu kantor : </Text>
                </View>
            </View>
        )
    }
}
export default Home

const styles = StyleSheet.create({
    Component:{
        width:'100%',
        height:'100%',
        justifyContent:'center',
        alignItems:'center'
    },
    Card:{
        width:'90%',
        paddingVertical:30,
        paddingHorizontal:10,
        backgroundColor:'#fff',
        elevation:10,
        borderBottomLeftRadius:20,
        borderBottomRightRadius:20
    },
    TextBio:{
        fontSize:16,
        marginVertical:10
    },
    TextTitle:{
        fontSize:20,
        fontWeight:'bold',
        marginBottom:50,
    }
})
```

hubungakan kedua screen dengan navigation di folder src/route

index.js
```
import {createAppContainer} from 'react-navigation'
import {createStackNavigator} from 'react-navigation-stack'

//import file.js
import Login from '../screen/login'
import Home from '../screen/Home'
const Route = createStackNavigator({
    Login:{
        screen:Login,
        navigationOptions:{
            header:null
        }
    },
    Home:{
        screen:Home,
        navigationOptions:{
            header:null
        }
    }
})

export default createAppContainer(Route)
``` 
kita cobaa apakah navigation nya berjalan atau tidk dengan mengedit source code di file app.js
```
import React from 'react'
import Route from './src/route/index'

class App extends React.Component{
  render(){
    return(
      <Route/>
    )
  }
}
export default App;
```
cobalah kalian react-native run-android 
### sekarang kita mulai membuat Redux untuk kedua screen tersebuta 
#### buatlah folder src/redux/type dengan file index.js
```
export const CHANGE_USERNAME = "CHANGE_USERNAME"
export const CHANGE_ALAMAT = "CHANGE_ALAMAT"
export const CHANGE_HOBBY = "CHANGE_HOBBY"
export const CHANGE_SEKOLAH_KANTOR= "CHANGE_SEKOLAH_KANTOR"
````
di file index.js kita export variabel yang dibutuhkan.

#### buat folder src/redux/action lalau buat file index.js
```
// memanggil file type
import {
  CHANGE_USERNAME,
  CHANGE_ALAMAT,
  CHANGE_HOBBY,
  CHANGE_SEKOLAH_KANTOR
} from '../type/index'

export const changeUsername = payload => {
  return {
    type: CHANGE_USERNAME,
    payload: payload
  }
}
export const changeAlamat = payload => {
  return {
    type: CHANGE_ALAMAT,
    payload: payload
  }
}
export const changeHobby = payload => {
  return {
    type: CHANGE_HOBBY,
    payload: payload
  }
}
export const changeSekolahKantor = payload => {
  return {
    type: CHANGE_SEKOLAH_KANTOR,
    payload: payload
  }
}

```
file ini digunakan untuk muatan informasi yang mengirim data dari aplikasi Anda ke store Anda.
fungsi dari payload adalah parameter dari functionya.

#### buat folder src/redux/reducer lalau buat file index.js
```
import {CHANGE_USERNAME, CHANGE_ALAMAT, CHANGE_HOBBY, CHANGE_SEKOLAH_KANTOR} from '../type/index'
import {combineReducers} from 'redux'

const isState = {
    username:'',
    alamat:'',
    hobby:'',
    sekolah_kantor:''
}

const Reducer = (state = isState, action)=>{
    switch (action.type) {
        case CHANGE_USERNAME:
            return {...state, username: action.payload}
        case CHANGE_ALAMAT:
            return {...state, alamat: action.payload}
        case CHANGE_HOBBY:
            return {...state, hobby: action.payload}
        case CHANGE_SEKOLAH_KANTOR:
            return {...state, sekolah_kantor: action.payload}
        default:
            return state
    }
};

export default combineReducers({Reducer})`
```
file ini adalah store nya, semua state yang dibutuhkan kita simpan disini.
di file ini kita buat variabel dengan isi state. dan variabel untuk action untuk menampilak suatu data
expor default combineReducers ini di gunakan untuk mengekpor variabel Reducer masing-masing sesui dengan state nya


nah untuk folder Redux sudah selesai itu baru dasarnya doang masih ada lagi insyaAllah nanti saya akan perbaharui source code dengan secepatnya. lanjut ke folder screen kalian tinggal tambahin-tambahin aja seperti di bawah ini.

screenInput.js
```
...
+ import { connect } from 'react-redux'
+ import {changeUsername, changeAlamat, changeHobby, changeSekolahKantor} from '../redux/action'

		...

                    <TextInput
                        style={styles.TextInput}
                        placeholder="username"
                      + onChangeText={(text)=>this.props.changeUsername(text)}
                    />
                    <Text style={styles.TextLogin}>alamat : </Text>
                    <TextInput
                        style={styles.TextInput}
                        placeholder="alamat"
                      + onChangeText={(text)=>this.props.changeAlamat(text)}
                    />
                    <Text style={styles.TextLogin}>hobby : </Text>
                    <TextInput
                        style={styles.TextInput}
                        placeholder="hobby"
                      + onChangeText={(text)=>this.props.changeHobby(text)}
                    />
                    <Text style={styles.TextLogin}>sekolah atau kantor : </Text>
                    <TextInput
                        style={styles.TextInput}
                        placeholder="sekolah atau kantor"
                      + onChangeText={(text)=>this.props.changeSekolahKantor(text)}
                    />
                    
		...
		    
+ const mapStateToProps = state =>{
    let {username, alamat, hobby, sekolah_kantor} = state.Reducer
    return {username, alamat, hobby, sekolah_kantor}
  }
+ export default connect(mapStateToProps,{changeUsername, changeAlamat, changeHobby, changeSekolahKantor})(Login)
...
```

screenHome.js
```
...
import { connect } from 'react-redux'
import {changeUsername, changeAlamat, changeHobby, changeSekolahKantor} from '../redux/action'

	...
		
              + <Text style={styles.TextBio}>usernam : {this.props.username}</Text>
              + <Text style={styles.TextBio}>alamat : {this.props.alamat}</Text>
              + <Text style={styles.TextBio}>hobby : {this.props.hobby}</Text>
              + <Text style={styles.TextBio}>asal sekolah atu kantor : {this.props.sekolah_kantor}</Text>
               
	...
	
const mapStateToProps = state =>{
    let {username, alamat, hobby, sekolah_kantor} = state.Reducer
    return {username, alamat, hobby, sekolah_kantor}
}
export default connect(mapStateToProps,{changeUsername, changeAlamat, changeHobby, changeSekolahKantor})(Home)
...
```
disini kita menambahkan liblary react {connect} yang dimana di gunakan untuk menghubungkan dengan Redux, dan kita menmabhakan mapStateToProps yang di gunakan untuk menarik state objek reducer dari store global dan menempatkannya ke properti komponen

app.js
```
...
import {Provider} from 'react-redux'
import {createStore, applyMiddleware} from 'redux'
import Reducer from './src/redux/reducer/Reducer'
import Thunk from 'redux-thunk'

...
      //membuat stote Reducer dan memperluas komunikasi secara asynchronous dengan API eksternal untuk mengambil atau save data
    + <Provider store={createStore(Reducer,{},applyMiddleware(Thunk))}>
      <Route/>
    + </Provider>
...
```
silahkan kalian coba dan ulangi dan pahami alurnya tampa copy paste sebnayak 7X pasti nanti anda bakal terbiasa 
