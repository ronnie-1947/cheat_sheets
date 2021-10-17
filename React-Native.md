## Table Of Contents
- [Adding Fonts](#adding-fonts)
- [React-Native Navigation](#react-native-navigation)
    + 

# Adding Fonts

## By use of expo

1. Download the packages
```
npm i expo-font expo-app-loading
```
2. Create function fetchFonts
```
const fetchFonts = ()=> {
  return Font.loadAsync({
    'open-sans': require('./assets/fonts/OpenSans-Regular.ttf'),
    'open-sans-bold': require('./assets/fonts/OpenSans-Bold.ttf')
  })
}
```
3. Init function
```
const [dataLoaded, setDataLoaded] = useState(false)

if(!dataLoaded){
    return <AppLoading 
    startAsync={fetchFonts} 
    onFinish={()=>{setDataLoaded(true)}}
    onError={()=>{console.log('error')}}
    />
  }
```
4. Use fonts in stylesheet
```
const styles = StyleSheet.create({
    headerTitle: {
        color: 'black',
        fontSize: 18,
        fontFamily: 'open-sans-bold'
    }
})
```

______________________

# React Native Navigation

1. Download Packages
```
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```
2. Make a new file in root folder
```
mkdir navigation
touch navigation/Navigator.js
```