# Textures

## 시작에 앞서

지겨운 빨간색 큐브에 질리셨나요? 이제 텍스처를 추가할 시간입니다.

그러나 먼저, 텍스처가 무엇이고 우리는 실제로 이것들로 무엇을 할 수 있을까요?



## 텍스처..?

텍스처는 아마 알고 있듯이, 기하체의 표면을 덮을 이미지들입니다. 다양한 종류의 텍스처들은 기하체의 외관에 다양한 효과를 줄 수 있습니다. 단지 색상에 관한 것만이 아닙니다.

여기 조앙 파울로가 만든 유명한 문 텍스처를 사용하여 가장 흔한 종류의 텍스처들이 있습니다. 그의 작업이 마음에 든다면 Ko-fi를 사주거나 Patreon이 되어주세요.

### Color (or albedo)

알베도 텍스처는 가장 단순한 것입니다. 텍스처의 픽셀들을 가져와서 기하체에 적용하기만 합니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/000.jpg" alt="" width="188"><figcaption></figcaption></figure>

### Alpha

알파 텍스처는 흰색은 보이고 검은색은 보이지 않는 그레이스케일 이미지입니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/001.jpg" alt="" width="188"><figcaption></figcaption></figure>

### Height

Height 텍스처는 그레이스케일 이미지로서, 정점을 움직여 어떤 릴리프(relief, 입체감)를 생성합니다. 이를 보고 싶다면 세분화(subdivision)를 추가해야 합니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/002.png" alt="" width="188"><figcaption></figcaption></figure>

### Normal

Normal 텍스처는 작은 디테일을 추가합니다. 정점을 움직이지는 않지만, 빛이 마치 면이 다르게 방향지어진 것처럼 속이게 합니다. 노멀 텍스처는 기하학을 세분화할 필요 없이 좋은 성능으로 디테일을 추가하는데 매우 유용합니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/003.jpg" alt="" width="188"><figcaption></figcaption></figure>

### Ambient occlusion

Ambient occlusion 텍스처는 그레이스케일 이미지로서, 표면의 굴곡에 가짜 그림자를 생성합니다. 물리적으로 정확하지는 않지만, 확실히 대비를 만드는데 도움을 줍니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/004.jpg" alt="" width="188"><figcaption></figcaption></figure>

### Metalness

Metalness 텍스처는 금속적인 부분(흰색)과 비금속적인 부분(검은색)을 지정할 그레이스케일 이미지입니다. 이 정보는 반사를 생성하는데 도움을 줍니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/005.jpg" alt="" width="188"><figcaption></figcaption></figure>

### Roughness

Roughness는 Metalness과 함께 오는 그레이스케일 이미지로, 어느 부분이 거칠게(흰색) 처리되었고 어느 부분이 매끄럽게(검은색) 처리되었는지를 지정합니다. 이 정보는 빛을 분산시키는 데 도움을 줍니다. 카펫은 매우 거칠어서 빛 반사를 볼 수 없지만, 물의 표면은 매우 매끄러워서 빛이 반사되는 것을 볼 수 있습니다. 여기서, 나무는 명확한 코팅이 되어 있기 때문에 균일합니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/006.jpg" alt="" width="188"><figcaption></figcaption></figure>

### PBR&#x20;

이 텍스처들(특히 Metalness과 Roughness)은 우리가 PBR 원칙이라 부르는 것을 따릅니다. PBR은 물리 기반 렌더링(Physically Based Rendering)을 의미합니다. 실제와 같은 방향을 따라 현실적인 결과를 얻기 위해 많은 기술을 모은 것입니다.

다양한 다른 기술들이 있지만, PBR은 현실적인 렌더링을 위한 표준이 되어가고 있으며 많은 소프트웨어, 엔진, 라이브러리들이 이를 사용하고 있습니다.

지금 우리는 텍스처를 어떻게 로드하고 사용하는지, 어떤 변환을 적용할 수 있는지, 그리고 어떻게 최적화하는지에 초점을 맞출 것입니다. PBR에 대해 더 자세히는 나중 수업에서 살펴볼 것이지만, 궁금하다면 여기에서 더 배울 수 있습니다:

* [물리 기반 렌더링의 기본 이론](https://marmoset.co/posts/basic-theory-of-physically-based-rendering/)
* [물리 기반 렌더링, 당신도 할 수 있습니다](https://marmoset.co/posts/physically-based-rendering-and-you-can-too/)\


## 텍스처를 로드하는 방법

### 이미지의 URL 가져오기

텍스처를 로드하기 위해 우리는 이미지 파일의 URL이 필요합니다.

빌드 도구를 사용하고 있기 때문에, 이를 가져오는 두 가지 방법이 있습니다.

이미지 텍스처를 /src/ 폴더에 넣고 자바스크립트 의존성을 import하듯이 가져올 수 있습니다:

```javascript
import imageSource from './image.png'

console.log(imageSource)
```

또는 이미지를 /static/ 폴더에 넣고 이미지의 경로(URL에 /static을 제외하고)를 추가함으로써 접근할 수 있습니다:

```javascript
const imageSource = '/image.png'

console.log(imageSource)
```

이 /static/ 폴더는 Vite 템플릿의 설정 때문에 작동하는 것이니, 다른 종류의 번들러를 사용하고 있다면 프로젝트를 적응시켜야 할 수도 있습니다.

우리는 이 과정의 나머지 부분에서 /static/ 폴더 기법을 사용할 것입니다.

### 이미지 로딩하기

방금 본 문 텍스처를 /static/ 폴더에서 찾을 수 있으며, 이를 로딩하는 여러 가지 방법이 있습니다.

#### 네이티브 자바스크립트 사용하기

네이티브 자바스크립트를 사용할 때, 먼저 Image 인스턴스를 생성하고, load 이벤트를 리스닝한 다음, 그것의 src 속성을 변경하여 이미지 로딩을 시작해야 합니다:

```javascript
const image = new Image()
image.onload = () =>
{
    console.log('image loaded')
}
image.src = '/textures/door/color.jpg'
```

콘솔에 'image loaded'가 나타나는 것을 볼 수 있습니다. 보시다시피, 우리는 경로를 '/textures/door/color.jpg'로 설정했는데, 이때 /static 폴더를 경로에 포함시키지 않았습니다.

우리는 그 이미지를 직접 사용할 수 없습니다. 먼저 그 이미지로부터 Texture를 생성해야 합니다.

이는 WebGL이 GPU에 의해 접근될 수 있는 매우 특정한 형식을 필요로 하기 때문이며, 또한 몇 가지 변경사항이 텍스처에 적용될 것인데 예를 들어 밉맵핑(mipmapping) 같은 것들이지만, 이에 대해서는 조금 뒤에 더 자세히 살펴볼 것입니다.

Texture 클래스로 텍스처 생성하기:

```javascript
const image = new Image()
image.addEventListener('load', () =>
{
    const texture = new THREE.Texture(image)
})
image.src = '/textures/door/color.jpg'
```

\
이제 해야 할 일은 그 텍스처를 재질(material)에 사용하는 것입니다. 불행히도, 텍스처 변수는 함수 안에서 선언되었고 이 함수 밖에서는 접근할 수 없습니다. 이것은 스코프라고 불리는 자바스크립트의 제한입니다.

함수 안에서 메시(mesh)를 생성할 수도 있지만, 함수 밖에서 텍스처를 생성하고 이미지가 로드되면 텍스처의 needsUpdate 속성을 true로 설정함으로써 업데이트하는 더 나은 해결책이 있습니다:

```javascript
const image = new Image()
const texture = new THREE.Texture(image)
image.addEventListener('load', () =>
{
    texture.needsUpdate = true
})
image.src = '/textures/door/color.jpg'
```

이렇게 하면, 텍스처 변수를 바로 사용할 수 있고, 이미지가 로드될 때까지는 투명하게 보일 것입니다.

큐브에 텍스처를 보려면, color 속성을 map으로 대체하고 값으로 텍스처를 사용하세요:

```javascript
const material = new THREE.MeshBasicMaterial({ map: texture })
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/007.png" alt="" width="375"><figcaption></figcaption></figure>

큐브의 각 면에 문 텍스처가 보여야 합니다. 그러나 텍스처가 이상하게 회색빛으로 보입니다.

이는 이미지가 sRGB 색공간을 사용하여 인코딩되었지만 Three.js가 이를 인지하지 못하기 때문입니다.

이를 수정하기 위해서는 colorSpace를 THREE.sRGBColorSpace로 설정해야 합니다:

```javascript
const texture = new THREE.Texture(image)
texture.colorSpace = THREE.SRGBColorSpace
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/008.png" alt="" width="375"><figcaption></figcaption></figure>

이제 올바른 색상을 얻게 됩니다.

일반적인 아이디어는 재질의 map 또는 matcap 속성(나중에 보게 될)에 사용되는 텍스처들은 sRGB로 인코딩되어야 하며, 우리는 이러한 경우에만 colorSpace를 THREE.sRGBColorSpace로 설정해야 합니다.

색 공간에 대한 자세한 내용은 실제적인 렌더링 수업에서 다룹니다.

#### TextureLoader 사용하기

네이티브 자바스크립트 기술은 그리 복잡하지 않지만, TextureLoader를 사용한 더 간단한 방법이 있습니다.

TextureLoader 클래스를 사용하여 변수를 인스턴스화하고 .load(...) 메소드를 사용하여 텍스처를 생성하세요:

```javascript
const textureLoader = new THREE.TextureLoader()
const texture = textureLoader.load('/textures/door/color.jpg')
texture.colorSpace = THREE.SRGBColorSpace
```

내부적으로, Three.js는 이전에 했던 것처럼 이미지를 로드하고 준비가 되면 텍스처를 업데이트할 것입니다.

원하는 만큼의 텍스처를 하나의 TextureLoader 인스턴스로 로드할 수 있습니다.

경로 뒤에 3개의 함수를 보낼 수 있습니다. 이 함수들은 다음 이벤트에 대해 호출됩니다:

* 이미지가 성공적으로 로드됐을 때 load
* 로딩이 진행 중일 때 progress
* 무언가 잘못됐을 때 error

```javascript
const textureLoader = new THREE.TextureLoader()
const texture = textureLoader.load(
    '/textures/door/color.jpg',
    () =>
    {
        console.log('loading finished')
    },
    () =>
    {
        console.log('loading progressing')
    },
    () =>
    {
        console.log('loading error')
    }
)
texture.colorSpace = THREE.SRGBColorSpace
```

텍스처가 작동하지 않는다면, 이 콜백 함수들을 추가하여 무슨 일이 일어나고 있는지 보고 오류를 찾는 것이 유용할 수 있습니다.

#### LoadingManager 사용하기

마지막으로, 여러 이미지를 로드하고 모든 이미지가 로드됐을 때 알림과 같은 이벤트를 공유하고 싶다면 LoadingManager를 사용할 수 있습니다.

LoadingManager 클래스의 인스턴스를 생성하고 TextureLoader에 전달하세요:

```javascript
const loadingManager = new THREE.LoadingManager()
const textureLoader = new THREE.TextureLoader(loadingManager)
```

onStart, onLoad, onProgress, onError 같은 다양한 이벤트를 자신의 함수로 대체하여 듣을 수 있습니다:

```javascript
const loadingManager = new THREE.LoadingManager()
loadingManager.onStart = () =>
{
    console.log('loading started')
}
loadingManager.onLoad = () =>
{
    console.log('loading finished')
}
loadingManager.onProgress = () =>
{
    console.log('loading progressing')
}
loadingManager.onError = () =>
{
    console.log('loading error')
}

const textureLoader = new THREE.TextureLoader(loadingManager)
```

이제 필요한 모든 이미지를 로드하기 시작할 수 있습니다:

```javascript
// ...

const colorTexture = textureLoader.load('/textures/door/color.jpg')
colorTexture.colorSpace = THREE.SRGBColorSpace
// 다른 텍스처 로드
```

여기서 보듯이, 우리는 텍스처 변수를 colorTexture로 이름을 바꿨으니 재질에서도 이를 변경하는 것을 잊지 마세요:

```javascript
const material = new THREE.MeshBasicMaterial({ map: colorTexture })
```

모든 에셋이 로드될 때까지 로더를 보여주고 싶다면 LoadingManager가 매우 유용합니다. 미래의 수업에서 볼 수 있듯이, 다른 유형의 로더와 함께 사용할 수도 있습니다.

## UV unwrapping

큐브에 텍스처를 배치하는 것이 상당히 논리적인 반면, 다른 기하학적 형태에 대해서는 조금 더 까다로울 수 있습니다.

BoxGeometry 대신 다른 기하학적 형태로 교체해 보세요:

```javascript
const geometry = new THREE.BoxGeometry(1, 1, 1)

// 또는
const geometry = new THREE.SphereGeometry(1, 32, 32)

// 또는
const geometry = new THREE.ConeGeometry(1, 1, 32)

// 또는
const geometry = new THREE.TorusGeometry(1, 0.35, 32, 100)
```

보시다시피, 텍스처가 기하학적 형태를 덮기 위해 다양한 방식으로 늘어나거나 압축되고 있습니다.

이것을 UV 언랩핑이라고 합니다. 오리가미나 캔디 포장을 펼쳐 평평하게 만드는 것처럼 상상할 수 있습니다. 각 정점은 평면(보통 정사각형) 위에 2D 좌표를 가지게 됩니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/009.png" alt="" width="375"><figcaption></figcaption></figure>

실제로 `geometry.attributes.uv` 속성에서 그 UV 2D 좌표를 볼 수 있습니다:

```javascript
console.log(geometry.attributes.uv)
```

이러한 UV 좌표는 프리미티브를 사용할 때 Three.js에 의해 생성됩니다. 만약 자신만의 기하학적 형태를 만들고 그 위에 텍스처를 적용하고 싶다면, UV 좌표를 명시해야 합니다.

3D 소프트웨어를 사용하여 기하학적 형태를 만들고 있다면, UV 언랩핑도 해야 합니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/010.png" alt="" width="375"><figcaption></figcaption></figure>

걱정하지 마세요; 대부분의 3D 소프트웨어에는 자동 언랩핑 기능이 있어 필요한 작업을 수행할 수 있습니다.

## 텍스처 변형하기

우리의 큐브와 하나의 텍스처로 돌아가서 우리가 그 텍스처에 적용할 수 있는 변형의 종류를 살펴봅시다.

### Repeat

repeat 속성을 사용하여 텍스처를 반복할 수 있습니다. 이는 Vector2이므로 x와 y 속성을 가집니다.

이 속성들을 변경해 보세요:

```javascript
const colorTexture = textureLoader.load('/textures/door/color.jpg')
colorTexture.colorSpace = THREE.SRGBColorSpace
colorTexture.repeat.x = 2
colorTexture.repeat.y = 3
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/011.png" alt="" width="375"><figcaption></figcaption></figure>

보시다시피, 텍스처가 반복되는 것이 아니라 더 작아지고, 마지막 픽셀이 늘어나 보입니다.

이는 기본적으로 텍스처가 자체적으로 반복되도록 설정되어 있지 않기 때문입니다. 이를 변경하려면, wrapS와 wrapT 속성을 THREE.RepeatWrapping 상수를 사용하여 업데이트해야 합니다.

* wrapS는 x축을 위한 것입니다.
* wrapT는 y축을 위한 것입니다.

```javascript
colorTexture.wrapS = THREE.RepeatWrapping
colorTexture.wrapT = THREE.RepeatWrapping
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/012.png" alt="" width="375"><figcaption></figcaption></figure>

THREE.MirroredRepeatWrapping을 사용하여 방향을 번갈아 가며 반복할 수도 있습니다:

```javascript
colorTexture.wrapS = THREE.MirroredRepeatWrapping
colorTexture.wrapT = THREE.MirroredRepeatWrapping
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/013.png" alt="" width="375"><figcaption></figcaption></figure>

### Offset

offset 속성을 사용하여 텍스처를 오프셋할 수도 있습니다. 이것 역시 Vector2로서 x와 y 속성을 가집니다. 이들을 변경하는 것은 단순히 UV 좌표를 오프셋합니다:

```javascript
colorTexture.offset.x = 0.5
colorTexture.offset.y = 0.5
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/014.png" alt="" width="375"><figcaption></figcaption></figure>

### Rotation

rotation 속성을 사용하여 텍스처를 회전시킬 수 있습니다. 이는 라디안 단위의 각도에 해당하는 단순한 숫자입니다:

```javascript
colorTexture.rotation = Math.PI * 0.25
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/015.png" alt="" width="375"><figcaption></figcaption></figure>

offset과 repeat 속성을 제거하면, 회전이 큐브 면의 왼쪽 하단 모서리를 중심으로 발생하는 것을 볼 수 있습니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/016.png" alt="" width="375"><figcaption></figcaption></figure>

사실, 그것은 0, 0 UV 좌표입니다. 회전의 중심점을 변경하고 싶다면, center 속성을 사용하여 변경할 수 있습니다. 이것도 Vector2입니다:

```javascript
colorTexture.rotation = Math.PI * 0.25
colorTexture.center.x = 0.5
colorTexture.center.y = 0.5
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/017.png" alt="" width="375"><figcaption></figcaption></figure>

### Filtering and Mipmapping&#x20;

큐브의 윗면을 바라볼 때, 이 면이 거의 숨겨져 있을 때, 매우 흐릿한 텍스처를 볼 수 있습니다.

<figure><img src="https://threejs-journey.com/assets/lessons/11/018.png" alt="" width="375"><figcaption></figcaption></figure>

이는 필터링과 밉맵핑(mipmapping) 때문입니다.

밉맵핑은 텍스처의 반으로 줄인 작은 버전을 계속 만드는 기술로, 1x1 텍스처를 얻을 때까지 반복합니다. 이러한 모든 텍스처 변형들은 GPU에 전송되며, GPU는 가장 적절한 버전의 텍스처를 선택합니다.

Three.js와 GPU는 이미 이 모든 것을 처리하고 있으며, 사용할 필터 알고리즘을 설정하기만 하면 됩니다. 필터 알고리즘에는 두 가지 유형이 있습니다: 축소 필터(minification filter)와 확대 필터(magnification filter).

#### Minification filter

축소 필터는 텍스처의 픽셀이 렌더링의 픽셀보다 작을 때 발생합니다. 즉, 텍스처가 커버하는 표면에 비해 너무 큰 경우입니다.

minFilter 속성을 사용하여 텍스처의 축소 필터를 변경할 수 있습니다.

6가지 가능한 값이 있습니다:

* THREE.NearestFilter
* THREE.LinearFilter
* THREE.NearestMipmapNearestFilter
* THREE.NearestMipmapLinearFilter
* THREE.LinearMipmapNearestFilter
* THREE.LinearMipmapLinearFilter

기본값은 THREE.LinearMipmapLinearFilter입니다. 텍스처의 모습에 만족하지 않는다면, 다른 필터를 시도해야 합니다.

모든 필터를 살펴보진 않겠지만, 매우 다른 결과를 나타내는 THREE.NearestFilter를 테스트할 것입니다:

```javascript
colorTexture.minFilter = THREE.NearestFilter
```

<figure><img src="../.gitbook/assets/ezgif.com-video-to-gif-converter (4).gif" alt=""><figcaption></figcaption></figure>

픽셀 비율이 1보다 큰 장치를 사용하는 경우 큰 차이를 보지 못할 수 있습니다. 그렇지 않다면, 카메라를 이 면이 거의 숨겨진 위치에 두면 더 많은 디테일과 이상한 아티팩트(artifacts)를 볼 수 있어야 합니다.

/static/textures/ 폴더에 위치한 checkerboard-1024x1024.png 텍스처로 테스트하면, 이러한 아티팩트를 더 명확하게 볼 수 있습니다:

```javascript
const colorTexture = textureLoader.load('/textures/checkerboard-1024x1024.png')
```

<figure><img src="../.gitbook/assets/ezgif.com-video-to-gif-converter (5).gif" alt=""><figcaption></figcaption></figure>

보이는 아티팩트는 모아레 패턴이라고 하며, 보통 이를 피하려고 합니다.

#### Magification filter

확대 필터는 축소 필터와 마찬가지로 작동하지만, 텍스처의 픽셀이 렌더링의 픽셀보다 클 때 적용됩니다. 즉, 텍스처가 커버하는 표면에 비해 너무 작은 경우입니다.

static/textures/ 폴더에도 위치한 checkerboard-8x8.png 텍스처를 사용하여 결과를 볼 수 있습니다:

```javascript
const colorTexture = textureLoader.load('/textures/checkerboard-8x8.png')
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/021.png" alt="" width="375"><figcaption></figcaption></figure>

텍스처가 매우 작은 텍스처를 매우 큰 표면에 사용하기 때문에 전부 흐려집니다.

이것이 끔찍해 보일 수도 있지만, 아마도 최선일 것입니다. 효과가 너무 과장되지 않는다면, 사용자는 아마도 이를 전혀 알아채지 못할 것입니다.

magFilter 속성을 사용하여 텍스처의 확대 필터를 변경할 수 있습니다.

가능한 값은 두 가지뿐입니다:

* THREE.NearestFilter
* THREE.LinearFilter

기본값은 THREE.LinearFilter입니다.

THREE.NearestFilter를 테스트해 보면, 기본 이미지가 보존되고 픽셀화된 텍스처를 얻게 됩니다:

```javascript
colorTexture.magFilter = THREE.NearestFilter
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/022.png" alt="" width="375"><figcaption></figcaption></figure>

픽셀화된 텍스처를 사용하는 마인크래프트 스타일을 목표로 한다면 이는 유리할 수 있습니다.

static/textures/ 폴더에 위치한 minecraft.png 텍스처를 사용하여 결과를 볼 수 있습니다:

```javascript
const colorTexture = textureLoader.load('/textures/minecraft.png')
```

<figure><img src="https://threejs-journey.com/assets/lessons/11/023.png" alt="" width="375"><figcaption></figcaption></figure>

모든 필터에 대한 마지막 말은 THREE.NearestFilter가 다른 것들보다 더 간단(?)하며, 이를 사용할 때 더 나은 성능을 얻을 수 있다는 것입니다.

minFilter 속성에만 미핑맵을 사용하세요. THREE.NearestFilter를 사용한다면 미핑맵이 필요 없으며, `colorTexture.generateMipmaps = false`로 미핑맵 생성을 비활성화할 수 있습니다:

```javascript
colorTexture.generateMipmaps = false
colorTexture.minFilter = THREE.NearestFilter
```

이렇게 하면 GPU의 부하를 약간 줄일 수 있습니다.

텍스처 형식과 최적화 텍스처를 준비할 때, 3가지 중요한 요소를 염두에 두어야 합니다:

* 무게
* 크기(또는 해상도)
* 데이터

#### &#x20;The weight

웹사이트를 방문하는 사용자들이 이러한 텍스처를 다운로드해야 한다는 것을 잊지 마세요. .jpg(손실 압축이지만 보통 가벼움) 또는 .png(무손실 압축이지만 보통 무거움)와 같은 웹에서 사용하는 대부분의 이미지 유형을 사용할 수 있습니다.

가능한 한 가벼우면서도 수용 가능한 이미지를 얻기 위해 일반적인 방법을 적용하세요. TinyPNG(또한 jpg와 함께 작동)와 같은 압축 웹사이트나 소프트웨어를 사용할 수 있습니다.

#### The size&#x20;

사용하는 텍스처의 각 픽셀은 이미지의 무게와 관계없이 GPU에 저장되어야 합니다. 그리고 하드 드라이브처럼 GPU에도 저장 공간에 제한이 있습니다. 자동 생성된 미핑맵은 저장해야 할 픽셀의 수를 증가시키기 때문에 문제가 더욱 심각합니다.

이미지 크기를 가능한 한 줄이려고 노력하세요.

미핑맵에 대해 언급했듯이, Three.js는 1x1 텍스처가 될 때까지 텍스처의 반으로 크기를 줄일 것입니다. 그렇기 때문에 텍스처의 너비와 높이는 2의 거듭제곱이어야 합니다. 이는 Three.js가 텍스처 크기를 2로 나눌 수 있도록 하기 위해 필수적입니다.

예: 512x512, 1024x1024 또는 512x2048

512, 1024, 2048은 1에 도달할 때까지 2로 나눌 수 있습니다.

2의 거듭제곱이 아닌 너비나 높이를 가진 텍스처를 사용하는 경우, Three.js는 가장 가까운 2의 거듭제곱 숫자로 늘리려고 시도할 것이며, 이는 시각적으로 좋지 않은 결과를 초래할 수 있고 콘솔에 경고가 표시될 것입니다.

#### The data&#x20;

아직 테스트하지 않았지만, 텍스처는 투명도를 지원합니다. 아시다시피, jpg 파일은 알파 채널이 없으므로 png를 사용하는 것이 좋을 수 있습니다.

또는 미래의 수업에서 볼 알파 맵을 사용할 수 있습니다.

노멀 텍스처(보라색인 것)를 사용하는 경우, 각 픽셀의 빨강, 초록, 파랑 채널에 대한 정확한 값을 원할 것이므로, 무손실 압축이 값을 보존하는 png를 사용해야 합니다.

## 텍스처 찾기

안타깝게도, 완벽한 텍스처를 찾는 것은 항상 어렵습니다. 많은 웹사이트가 있지만, 텍스처가 항상 적합하지 않거나 지불해야 할 수도 있습니다.

웹에서 검색하는 것부터 시작하는 것이 좋습니다. 여기 제가 자주 방문하는 몇몇 웹사이트가 있습니다.

* [poliigon.com](http://poliigon.com/)
* [3dtextures.me](http://3dtextures.me/)
* [arroway-textures.ch](http://arroway-textures.ch/')

개인적인 용도가 아닌 경우 텍스처를 사용할 권리가 있는지 항상 확인하세요.

또한 Photoshop이나 Substance Designer와 같은 소프트웨어를 사용해 사진과 2D 소프트웨어로 직접 만들거나 절차적 텍스처를 생성할 수도 있습니다.
