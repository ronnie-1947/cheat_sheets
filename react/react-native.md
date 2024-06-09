# React-Native

### Table Of Contents

* [Adding Fonts](react-native.md#adding-fonts)
* [React-Native Navigation](react-native.md#react-native-navigation) +

## Adding Fonts

### By use of expo

1. Download the packages

```
npm i expo-font expo-app-loading
```

1. Create function fetchFonts

```
const fetchFonts = ()=> {
  return Font.loadAsync({
    'open-sans': require('./assets/fonts/OpenSans-Regular.ttf'),
    'open-sans-bold': require('./assets/fonts/OpenSans-Bold.ttf')
  })
}
```

1. Init function

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

1. Use fonts in stylesheet

```
const styles = StyleSheet.create({
    headerTitle: {
        color: 'black',
        fontSize: 18,
        fontFamily: 'open-sans-bold'
    }
})
```

***

## React Native Navigation

### Types of Navigation

1. Stack Navigator
2. Drawer Navigator

### Steps Sack Navigation

1. Download Packages

```
expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

1. Make a new file in root folder

```
mkdir navigation
touch navigation/Navigator.js
```

1. Navigaror.js file.. Creates a jsx component

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

1. App.js file

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

1. Push, pop, navigate, replace, goback are used to navigate between different pages

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

1. Setting Title using navigationOptions

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

### Tabs Navigation

***

## Components

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

***

## Notifications

### Install Packages

```
expo install expo-notifications
expo install expo-permissions
```

### Change the package.json file

```
"android": {
    "useNextNotificationsApi": true
  }
```

### Ask for appropriate permissions

```
  const getNotiPermisson = async ()=>{

    const {status: existingStatus} = await Notifications.getPermissionsAsync()

    let finalStatus = existingStatus
    if(finalStatus !== 'granted'){
      const {status} = await Notifications.requestPermissionsAsync()
      finalStatus = status
    }
    if(finalStatus !== 'granted'){
      alert('Failed to get permissons')
      return
    }
  }
```

### Local Notification function

```
const triggerNotificationHandler = ()=> {
    Notifications.scheduleNotificationAsync({
      content: {
        title: "My first local notification",
        body: "This is the body of the notificaiton"
      },
      trigger: {
        seconds: 10
      }
    })
  }
```
