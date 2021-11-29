# Position

문서 상에 요소들의 위치를 결정하는 속성
<br>
<br>

## Static<br>

모든 요소들의 기본 position 상태로, 요소를 일반적인 문서 흐름에 따라 배치한다.<br>
static의 경우 top, right, left, bottom, z-index 등의 속성들에 아무런 영향도 받지 않는다.

<br>

## relative<br>

static과 마찬가지로 요소들이 문서의 일반적인 흐름에 따라 배치되지만, 요소 자신의 static 위치에서 top, right, left, bottom 등의 속성들에 의해 상대적인(relative) 위치에 배치된다.

<br>

## absolute<br>

문서의 일반적인 흐름에 따르지 않고 가장 가까운 부모 요소 중 position:static이 아닌 요소를 기준으로 상대적인 위치에 배치된다.<br>
만약 부모 요소 중 position이 relative, absolute, fixed인 태그가 없다면 가장 상위의 태그(body)가 기준이 된다.

<br>

## fixed<br>

absolute와 마찬가지로 문서의 일반적인 흐름에 따르지 않으며 스크린의 뷰포트를 기준으로 특정 위치에 고정되어 배치된다.<br>
즉, 스크롤이 이동해도 움직이지 않고 해당 위치에 고정되어 있다.

<br>

## sticky<br>

fixed와 같이 지정된 위치에 고정되지만 뷰포트 기준이 아닌 부모 요소를 기준으로 배치된다.<br>
또한 문서의 일반적인 흐름에 따라 배치되어 다른 요소들에 영향을 주지 않는다.<br>

sticky속성이 정상적으로 작동하기 위해서는 여러 조건이 필요하다.

- 해당 요소에 top, right, bottom, left 중 하나를 필수적으로 설정해야 한다.
- 부모 또는 조상 요소에 overflow 속성을 해주어야 한다. 부모 요소가 없을 경우엔 body가 기준이 된다.
- 부모 요소에 반드시 height 속성이 설정되어 있어야 한다. 만약 height 속성이 설정되어 있지 않으면 해당 요소는 static 속성처럼 동작한다.
