# RN-serverdan-vericekme

import React from 'react';
import { Container, Spinner, TextH3 } from "../UI";
import { Text,FlatList,
  Image,
  StyleSheet,
  Dimensions,
  View,TouchableOpacity ,
  ScrollView,
  TextInput
} from 'react-native';

import * as Permissions from 'expo-permissions';
import { BarCodeScanner } from 'expo-barcode-scanner';

import {window} from "../constants/Layout";

var {height, width } = Dimensions.get('window');

class DecodeScreen extends React.Component{
   
  // Component State
  state = { 
    barcode:[], 
  }
  
  async componentDidMount() { 

    
    const url = "http://turkcuyasamoykuleri.com/api/veri.json"
    return fetch(url)
    .then((response) => response.json())
    .then((responseJson) => {

      this.setState({
        isLoading: false,

        Barkode: responseJson.Barkode,   
      });

    })
    .catch((error) =>{
      console.error(error);
    });



  }

  

  _renderItemFood(item){
    
     
      return(
         
          <TouchableOpacity style={styles.divFood}>
            

            <View style={{height:((width/2)-20)-90, backgroundColor:'transparent', width:((width/2)-20)-10}} />
            <Text style={{fontWeight:'bold',fontSize:15,textAlign:'center'}}>{item.ID} </Text>
            
            <Text style={{fontSize:15,color:"green"}}>{item.Name}</Text>  
            <Text style={{fontSize:15,color:"green"}}>{item.Brand}</Text>  
            <Text style={{fontSize:15,color:"green"}}>{item.GlutenIcerigi}</Text>  
          </TouchableOpacity>
         
        
        )
     
  }


  
  render(){ 
      return (
  
  <ScrollView>
        <View style={{ flex: 1,backgroundColor:"#f2f2f2" }}>
          

          <View style={{width:width, borderRadius:20, paddingVertical:20, backgroundColor:'white'}}>
            <Text style={styles.titleCatg}>Sonuçlar</Text>
           
          
            <FlatList
               data={this.state.Barkode}
              numColumns={2}
              renderItem={({ item }) => this._renderItemFood(item)}
              keyExtractor = { (item,index) => index.toString() }
            />
            <View style={{height:20}} />
          </View>

        </View>
      </ScrollView>

      );
    }
 

  } 
export default DecodeScreen;


const styles = StyleSheet.create({
  imageBanner: {
    height:width/2,
    width:width-40,
    borderRadius:10,
    marginHorizontal:20
  },
  divCategorie:{
    backgroundColor:'red',
    margin:5, alignItems:'center',
    borderRadius:10,
    padding:10
  },
  titleCatg:{
    fontSize:30,
    fontWeight:'bold',
    textAlign:'center',
    marginBottom:10
  },
  imageFood:{
    width:((width/2)-20)-10,
    height:((width/2)-20)-30,
    backgroundColor:'transparent',
    position:'absolute',
    top:-45
  },
  divFood:{
    width:(width/2)-20,
    padding:10,
    borderRadius:10,
    marginTop:55,
    marginBottom:5,
    marginLeft:10,
    alignItems:'center',
    elevation:8,
    shadowOpacity:0.3,
    shadowRadius:50,
    backgroundColor:'white',
  }
});
