- 2021.03.16. Chapter01~05 
- 2021.03.17. Chapter06
2021.03.18. Chapter07
2021.03.19. Chapter08
2021.03.22.~23. Chapter09
2021.03.24.~25. firebaseTest
2021.03.26. 클론프로젝트



### 백그라운드 이미지 설정

```tsx
import {ImageBackground} from 'react-native';
<ImageBackground style={{flex: 1}} source={require('./images/backImage.png')} />
```



### 상단바 제거

- 참고: https://www.python2.net/questions-72779.htm

```tsx
import {StatusBar} from 'react-native';
<StatusBar hidden={true} /> 
```

### 버튼 생성 및 테스트 

```tsx
import {TouchableOpacity} from 'react-native';

const onPress = () => 
 <WebView source={{ uri: 'https://dorm.pusan.ac.kr/dorm/function/mealPlan/20000403' }}  />

<TouchableOpacity style={styles.button} onPress={onPress}>
            <Text>here</Text>
</TouchableOpacity>
```

