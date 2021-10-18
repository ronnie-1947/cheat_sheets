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

## Types of Navigation

1. Stack Navigator
2. Drawer Navigator

## Steps Sack Navigation

1. Download Packages
```
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```
2. Make a new file in root folder
```
mkdir navigation
touch navigation/Navigator.js
```

3. Navigaror.js file.. Creates a jsx component
```
import {createStackNavigator} from 'react-navigation-stack'
import {createAppContainer} from 'react-navigation'

import Categories from '../screens/Categories'
import CategoryMeals from '../screens/CategoryMeals'
import MealDetail from '../screens/MealDetail'

const MealsNavigator = createStackNavigator({
    Categories, // Short form
    CategoryMeals: { // Long form
        screen: CategoryMeals
    },
    MealDetail

}, {
    defaultNavigationOptions: {
        headerStyle: {
            backgroundColor: 'pink'
        }
    },
    initialRouteName: 'Categories'
})

export default createAppContainer(MealsNavigator)
```
4. App.js file
```
import {enableScreens} from 'react-native-screens'
import Navigator from './navigation/MealsNavigator'

enableScreens()

export default function App() {

  return (
    <Navigator/>
  );
}
```

5. Push, pop, navigate, replace, goback are used to navigate between different pages
```
const Categories = ({navigation}) => {

    const param = navigation.getParam('itemId')
    return (
        <View style={styles.screen}>
            <Text>The Categories Screen</Text>
            <Button title="Go to meals" onPress={()=> navigation.navigate({
                routeName: 'CategoryMeals',
                params: {
                    categoryId: 1
                }
            })}/>
        </View>
    )
}


```

6. Setting Title using navigationOptions
```
const Categories = ({navigation}) => {

    const param = navigation.getParam('itemId')
    return (
        <View style={styles.screen}>
            <Text>The Categories Screen</Text>
            <Button title="Go to meals" onPress={()=> navigation.navigate({
                routeName: 'CategoryMeals',
                params: {
                    categoryId: 1
                }
            })}/>
        </View>
    )
}

Categories.navigationOptions = (data)=> {

    const param = data.navigation.getParam('title')
    return {
        headerTitle : Platform.OS==='android'?'Meals Categories': 'Meals',
        
    }
}
```

## Tabs Navigation


__________________________
# Components
1. FlatList
```
<FlatList 
    numColumns={2} 
    data=[{id: 'a'}] 
    renderItem=()=>(< />) 
    keyExtractor=(item)=>{item.id}
/>
```

2.ImageBackground
```
<ImageBackground />
```

__________________________

# Platform API
