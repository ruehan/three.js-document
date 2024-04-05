# Transform objects

## 시작에 앞서

이제 모든 것이 준비되었으니, Three.js의 기능들을 탐색할 수 있습니다.

scene을 애니메이션하기 전에, scene 내의 객체들을 변환하는 방법을 알아야 합니다. 이전 파트에서 **`camera.position.z = 3`**을 사용하여 카메라를 뒤로 이동시킴으로써 이 작업을 수행했습니다.

scene 내의 객체들을 변환하기 위한 속성은 4가지가 있습니다:

1. position (객체를 이동)
2. scale (객체의 크기를 조절)
3. rotation (객체를 회전)
4. quaternion (객체를 회전)

## 객체 이동하기

**`position`**은 **`x, y, z`**라는 3가지 필수 속성을 가지고 있습니다. 이것들은 3D 공간 안에서 무언가를 위치시키기 위한 필수적인 3축입니다.

각 축의 방향은 전적으로 임의적일 수 있으며, 환경에 따라 달라질 수 있습니다. Three.js에서는 보통 y축이 위로, z축이 뒤로, x축이 오른쪽으로 가는 것으로 간주합니다.

1단위의 의미는 자기 자신에게 달려 있으며, 1은 1cm, 1m, 1km 일 수도 있습니다. 자신이 만들고자 하는 것에 단위를 적용시키는 것을 추천합니다. 예를 들어 집을 만들고자 한다면, 1단위가 1m가 될 것 입니다.

**`render(...)`** 메서드를 호출하기 전에 이 작업을 수행해야 하몀, 그렇지 않으면 이동하기 전에 mesh를 렌더링하게 됩니다.

```javascript
mesh.position.x = 0.7
mesh.position.y = -0.6
mesh.position.z = 1
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

**`position`** 속성은 단순한 객체가 아닙니다. **Vector3** 클래스의 인스턴스입니다. 이 클래스에는 **`x, y, z`** 속성 뿐 아니라 많은 메서드들이 있습니다.

벡터의 길이를 얻을 수 있습니다:

```javascript
console.log(mesh.position.length())
```

다른 **Vertor3**로 부터의 거리를 구할 수 있습니다. ( 이 코드를 카메라 생성 이후에 사용하세요 ):

```javascript
console.log(mesh.position.distanceTo(camera.position))
```

값을 정규화할 수 있습니다 ( 벡터의 길이를 1단위로 줄이되 방향은 유지하는 것을 의미합니다 ):

```javascript
console.log(mesh.position.normalize())
```

**`x, y, z`** 값을 따로 변경하는 대신에, **`set(...)`** 메서드를 사용하여 값을 변경할 수도 있습니다:

```javascript
mesh.position.set(0.7, -0.6, 1)
```

## Axes Helper

3D 공간안에 물건들을 배치하는 것은 어려운 도전이 될 수 있습니다. 각 축이 어떤 방향으로 정렬되어 있는지 알아내는 것은, 특히 카메라를 움직이기 시작할 때 복잡해집니다.

이를 해결하기 위한 방법 중 하나는 Three.js의 **AxesHelper**를 사용하는 것입니다.

**AxesHelper**는 **`x, y, z`** 축에 해당하는 3개의 선을 표시할 것이며, 각각은 scene의 중심에서 시작하여 해당 방향으로 이동합니다.

**AxesHelper**를 생성하기 위해, 장면을 인스터스화한 직후에 이를 인스턴스화하고 scene에 추가하면 됩니다. 선의 길이를 파라미터로 지정할 수 있습니다.

```javascript
const axesHelper = new THREE.AxesHelper(2)
scene.add(axesHelper)
```

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

녹색선은 y축 빨간색선은 x축에 해당하며 z축에 해당하는 파란색선도 있지만, 카메라와 완벽하게 정렬되어 있어서 볼 수 없습니다.

## 객체 크기 조절하기

**`scale`** 역시 **Vector3** 입니다. 기본적으로, **`x, y, z`**는 1과 같아, 즉 객체에는 어떤 크기 조절도 적용되지 않았다는 것을 의미합니다. 만약 값을 0.5로 설정하면, 해당 축에서 객체의 크기가 절반으로 줄어들고, 값을 2로 설정하면 해당 축에서 객체가원래 크기의 2배가 됩니다.

**`position`** 값을 주석처리하고 **`scale`**을 추가해봅시다.

```
mesh.scale.x = 2
mesh.scale.y = 0.25
mesh.scale.z = 0.5
```

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

아쉽지만, **`mesh`**가 카메라를 향하고 있기 때문에 z축의 변화를 확인 할 수 없습니다.

음수 값을 사용할 수는 있지만, 축이 논리적인 방향으로 정렬되지 않기 때문에 문제가 생길 수 있습니다.

**Vertor3**이기 때문에, **`position`**에서 사용한 모든 메서드를 사용할 수 있습니다.

## 객체 회전하기

회전은 위치와 크기 조절보다 조금 더 까다롭습니다. 회전을 처리하는 방법에는 두 가지가 있습니다.

명확한 **`rotation`** 속성을 사용할 수 있지만, 덜 명백한 **`quaternion`** 속성을 사용할 수도 있습니다. Three.js는 둘 다들 지원하며, 하나를 업데이트하면 다른 하나도 자동으로 업데이트 됩니다. 어느 것을 선호하는냐에 달려있습니다.

### Rotation

**`rotation`** 속성 역시 **`x, y, z`**속성을 가지고 있지만, **Vector3** 대신에 **Euler** 입니다. **Euler**의 **`x, y, z`** 속성을 변경할 때, 축의 방향으로 객체 중심을 관통하는 막대를 꽂고 그 객체를 막대에 따라 회전시키는 것을 상상할 수 있습니다.

* y축에서 회전하면, 마치 회전목마처럼 상상할 수 있습니다.
* x축에서 회전하면, 마치 자동차의 바퀴를 회전시키는 것처럼 상상할 수 있습니다.
* z축에서 회전하면, 마치 비행기 앞의 프로펠러를 회전시키는 것처럼 상상할 수 있습니다.

이 축의 값은 radians(라디안) 으로 표현됩니다. 반 회전을 하고 싶다면, 3.14159...와 같은 것을 작성해야 합니다. 자바스크립트에서는 **`Math.PI`**를 사용하여 근사치를 얻을 수 있습니다.

크기 조절 부분을 주석 처리하고 x와 y축 모두에서 완전 회전의 1/8을 추가해봅시다.

```javascript
mesh.rotation.x = Math.PI * 0.25
mesh.rotation.y = Math.PI * 0.25
```

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

쉽나요? 하지만 이러한 회전을 조합하면 이상한 결과가 나올 수 있습니다. 왜냐하면 x축을 회전시키면서 다른 축들의 방향도 변경하기 때문입니다. 회전은 다음 순서로 적용됩니다: x, y 그리고 z. 이것은 기존 축들 때문에 한 축이 더 이상 효과가 없게 되는 것처럼 이상한 동작들을 초래할 수 있는데 이를 gimbal lock(짐벌 락)이라고 합니다.

이 순서를 **`object.rotation.reorder('YXZ')`**메서드를 사용하여 변경할 수 있습니다.

**`Euler`**는 이해하기 쉽지만, 이 순서 문제는 더 많은 문제를 일으킬 수 있습니다. 그리고 이것이 대부분의 엔진과 3D 소프트웨어가 **`Quaternion`**이라고 불리는 다른 해결책을 사용하는 이유입니다.

### Quaternion

**`quaternion`** 역시 회전을 표현하지만, 순서 문제를 해결하는 더 수학적인 방식으로 표현합니다.

여기에서는 **`quaternion`**이 어떻게 작동하는지 다루지 않지만, 회전을 변경할 때 **`quaternion`**이 업데이트된다는 점을 기억하세요. 이는 두 가지 중 어느 것이든 마음대로 사용할 수 있다는 것을 의미합니다.

### Look at this!

Object3D 인스턴스에는 객체가 무언가를 바라보도록 요청할 수 있는 훌륭한 메서드인 **`lookAt(...)`**이 있습니다. 객체는 자동으로 **`-z`**축을 제공한 대상 쪽으로 회전합니다. 복잡한 수학 계산이 필요 없습니다.

이것을 사용하여 카메라를 객체 쪽으로 회전시키거나, 대포를 적을 향하게 하거나, 캐릭터의 눈을 객체 쪽으로 움직일 수 있습니다.

파라미터는 대상이며 **Vector3**이어야 합니다.

```javascript
camera.lookAt(new THREE.Vector3(0, -1, 0))
```

Vector3를 사용할 수도 있지만, mesh가 scene의 중심에 있기 때문에 기본 카메라 위치를 결과로 얻게 됩니다.

```javascript
camera.lookAt(mesh.position)
```

## 변환 결합하기

위치, 회전, 크기 조절을 어떤 순서로든 결합할 수 있습니다. 결과는 동일할 것입니다. 이는 객체의 상태와 동등합니다.

이전에 진행했던 모든 변환을 결합해 봅시다.

```javascript
mesh.position.x = 0.7
mesh.position.y = -0.6
mesh.position.z = 1
mesh.scale.x = 2
mesh.scale.y = 0.25
mesh.scale.z = 0.5
mesh.rotation.x = Math.PI * 0.25
mesh.rotation.y = Math.PI * 0.25
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

## Scene Graph

어느 순간, 물건들을 그룹화하고 싶을 수 있습니다. 예를 들어, 벽, 문, 창문, 지붕, 관목 등이 있는 집을 짓고 있다고 가정해 봅시다.

집이 너무 작다는 것을 깨닫고 각 객체의 크기를 다시 조정하고 그 위치를 업데이트해야 한다고 생각했을 때, 모든 객체들을 컨테이너에 그룹화하고 그 컨테이너의 크기를 조절하는 것이 좋은 대안일 수 있습니다.

이것을 Group 클래스로 할 수 있습니다.

Group을 인스턴스화하고 scene에 추가하세요. 이제 새 객체를 만들 때, 직접 장면에 추가하는 대신 방금 만든 Group에 add(...) 메서드를 사용하여 추가할 수 있습니다.

Group 클래스가 Object3D 클래스로부터 상속받기 때문에, position, scale, rotation, quaternion, lookAt과 같은 이전에 언급된 속성과 메서드에 접근할 수 있습니다.

lookAt(...) 호출을 주석 처리하고, 이전에 생성한 큐브 대신 3개의 큐브를 만들어 Group에 추가하세요. 그런 다음 그룹에 변환을 적용하세요.

```javascript
/**
 * 객체
 */
const group = new THREE.Group()
group.scale.y = 2
group.rotation.y = 0.2
scene.add(group)

const cube1 = new THREE.Mesh(
    new THREE.BoxGeometry(1, 1, 1),
    new THREE.MeshBasicMaterial({ color: 0xff0000 })
)
cube1.position.x = - 1.5
group.add(cube1)

const cube2 = new THREE.Mesh(
    new THREE.BoxGeometry(1, 1, 1),
    new THREE.MeshBasicMaterial({ color: 0xff0000 })
)
cube2.position.x = 0
group.add(cube2)

const cube3 = new THREE.Mesh(
    new THREE.BoxGeometry(1, 1, 1),
    new THREE.MeshBasicMaterial({ color: 0xff0000 })
)
cube3.position.x = 1.5
group.add(cube3)
```

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

객체를 변환하는 방법을 알게 되었으니, 이제 애니메이션을 만들 시간입니다.
