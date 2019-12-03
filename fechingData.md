## fechingData 
Seperti biasa kamu bisa membuat project baru atau di project lama mari kita langsung mulai praktekin saja dan nanti saya jelasing alurnya 

buat file src/screen dengan nama file Home.js
##### home.js
```
import React from 'react'
import { View, Text, StyleSheet, FlatList } from 'react-native'

class Home extends React.Component {

  render () {
    return (
      <View style={styles.Component}>
      <FlatList/>
      </View>
    )
  }
}
export default Home

const styles = StyleSheet.create({
  Component: {
    width: '100%',
    height: '100%',
    alignItems: 'center',
    paddingTop:30
})
```


lalu buat file di src/redux/type dengan nama file index.js
##### index.js

```
export const GET_DATA = 'GET_DATA'
```
file ini bisa di sebut sebagai Store dari si Rredux, Tempat yang digunakan menyimpan state aplikasimu. Hanya satu store saja yang wajib dimiliki dalam aplikasimu.

##### src/redux/action index.js

```
import {
  GET_DATA
} from '../type/index'

export const getData = payload => {
  return {
    type: GET_DATA,
    payload: payload
  }
}
```
file ini digunakan untuk muatan informasi yang mengirim data dari aplikasi Anda ke store Anda. fungsi dari payload adalah parameter dari functionya itu sendiri.

##### src/redux/reducer index.js
```
import {GET_DATA} from '../type/index'
import {combineReducers} from 'redux'

const isState = {
    dataSource:[]
}

const Reducer = (state = isState, action)=>{
    switch (action.type) {
        case GET_DATA:
            return {...state, dataSource:action.payload}
        default:
            return state
    }
};

export default combineReducers({Reducer})
```
Pada contoh di atas, file action di gunakan untuk mengangkut informasi yang mengirimkan data dari aplikasimu kepada store, Action hanya berbentuk javascript object pada umumnya. Dan membutuhkan properti bernama type untuk mengindikasikan hal yang akan terjadi (valuenya bebas) dan properti bernama apapun dan berapapun jumlahnya sesuai data yang akan dikirim ke store.

Fungsi combineReducers mengambil beberapa fungsi reducer sebagai argumen dan berubah menjadi fungsi reducer tunggal. Kami menempatkan fungsi reducer sebagai penghitung untuk counterReducer dan nama untuk namesReducer.

nah untuk folder Redux sudah selesai itu baru dasarnya doang masih ada lagi insyaAllah nanti saya akan perbaharui source code dengan secepatnya. lanjut ke folder screen kalian tinggal tambahin-tambahin aja seperti di bawah ini.

##### screenHome.js
```
  import React from 'react'
  import { View, Text, StyleSheet, FlatList } from 'react-native'
+ import { connect } from 'react-redux'
+ import {
    getData
  } from '../redux/action'

class Home extends React.Component {
  
+ componentDidMount(){
    this._fetchItem()
  }
  
+ _fetchItem = async () => {
  try {
    let response = await fetch(
      'https://masukajakan.000webhostapp.com/brand.json'
    )
    let responseJson = await response.json()
    await this.props.getData(responseJson)
  } catch (error) {
    console.error(error)
    }
  }


+ renderItem({item}){
    return(
       <View>
        <Text style={{fontSize:16}}>{item.name_brand}</Text>
      </View>
    )
  }
  
  render () {
    return (
      <View style={styles.Component}>
        <Text style={{marginBottom:10, fontSize:16, fontWeight:'bold'}}>List to Redux.</Text>
      + <FlatList
              data={this.props.dataSource}
              renderItem={this.renderItem}
              keyExtractor={(item, index) => index.toString()}
              refreshing={true}
          />
      </View>
    )
  }
}
+ const mapStateToProps = state => {
  let { dataSource } = state.Reducer
  return { dataSource }
  }
+ export default connect(
  mapStateToProps,
  { getData }
)(Home)

const styles = StyleSheet.create({
  Component: {
    width: '100%',
    height: '100%',
    alignItems: 'center',
    paddingTop:30
  }
})
```
contoh di atas kita menambahkan

import liblary dari react-redux yaitu connect() yang dimana connect bergunanya untuk menghubungkan react dengan store redux dan juga kita memanggil reducer
menambahkan function untuk fetch data API dan juga flatlist untuk menampilkan data API atau anda bisa menggunakan mhetod map
dan menambahkan variabel mapStateToProps, Fungsi ini memilih bagian dari kondisi Redux dan meneruskannya sebagai props ke komponen yang terhubung. Itu berarti bahwa struktur state yang didapatkan komponen Anda, bukan struktur yang sama dengan state Redux.

##### app.js
```
...
+ import {Provider} from 'react-redux'
+ import {createStore, applyMiddleware} from 'redux'
+ import Reducer from './src/redux/reducer/Reducer'
+ import Thunk from 'redux-thunk'

class Home extends React.Component{
 render(){
    return(
      //membuat stote Reducer dan memperluas komunikasi secara asynchronous dengan API eksternal untuk mengambil atau save 	data
	    + <Provider store={createStore(Reducer,{},applyMiddleware(Thunk))}>
	      <Route/>
	    + </Provider>
	)
    }
}
```
contoh diatas yang kita tambahkan adalah

import liblary Redux yaitu createStore yang digunakan untuk membuat store dan applyMiddleware yang digunakan sebagai memperluas Redux dengan fungsionalitas khusus.
import file Reducer
import Redux-thunk yang digunakan untuk berkomunikasi secara asynchronous dengan API eksternal untuk mengambil atau save data atau mengirim tindakan yang mengikuti siklus hidup permintaan ke API eksternal.
