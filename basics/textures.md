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
