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
