# 차트 렌더링하기 (Render a chart)

보여주는 각 `.mmd` 파일을 npm 패키지 `pele@1.9.0`으로 렌더링하고 사용자에게 SVG를 전달하세요. Pele은 저장소 외부에 한 번만 설치합니다. 각 SVG는 해당 `.mmd` 파일 옆에 동일한 이름으로 작성하세요. API 문서는 https://pele.run/api 에 있습니다. `render`의 옵션으로 `{ limit: 1_000_000, outputLimit: 8_000_000, maxEdges: 10_000 }`을 전달하세요. Pele의 기본값은 500개 상태 규모의 차트가 가질 수 있는 크기보다 작습니다. Pele이 `PeleError`를 던지면 사용자에게 Mermaid 텍스트와 에러를 보여주세요.

## 색상 (Colors)

Pele은 Mermaid의 `theme`과 `themeVariables`를 무시합니다. `.pele`에 `--pele-*` CSS 변수를 설정하세요. 목록은 https://pele.run/theming 에 있습니다. 명령어는 각 상태에 `class sN teal`처럼 그룹의 색상 클래스를 부여합니다. 또한 규칙을 위반하는 각 상태에는 `class sN breaks` 또는 `class sN mayBreak`를, 버그로 가는 최단 경로 위의 각 상태에는 `class sN bugPath`를, PROPERTY 반례가 무한히 반복하는 각 상태에는 `class sN repeats`를 표시합니다. Pele은 이러한 클래스들을 상태 노드에 추가합니다. CSS로 이 클래스들에 색상을 입히세요.

어떤 Mermaid 뷰어에서든 버그 표시가 보이도록 `.mmd` 파일에는 `breaks`와 `mayBreak`에 대한 `classDef` 줄이 포함되어 있습니다. 그룹 색상이나 `bugPath`, `repeats`에 대한 `classDef`는 없습니다. Pele은 각 `classDef` 색상을 노드의 `style` 속성으로 기록하며, `style` 속성은 일반 CSS 규칙보다 우선합니다. 따라서 아래 표시(mark) 규칙들에는 `!important`를 사용합니다. 이것이 없으면 다크 테마에서도 라이트 테마의 표시 색상이 그대로 유지됩니다. 버그 표시는 채우기(fill)와 텍스트 색상을 설정하고, `bugPath`와 `repeats`는 테두리(border)를 설정합니다. 표시가 그룹 색상 위에 나타나도록 그룹 색상 규칙 뒤에 표시 규칙을 배치하세요.

아래 색상들은 권장 예시입니다. 이전 스킬에서 사용하던 색상들이며, 각 SVG 시작 부분의 `<style>` 요소 안에 넣으세요. 그러면 각 SVG가 뷰어의 라이트 또는 다크 모드 설정을 따르게 됩니다.

```css
.pele {
  --pele-bg: #ffffff;
  --pele-fg: #1c1c1c;
  --pele-muted: #656565;
  --pele-line: #808080;
  --pele-surface: #fcfcfc;
  --pele-surface-alt: #f1f1f1;
  --pele-border: #c9c9c9;
}
.pele .blue rect { fill: #c3e0ff; }
.pele .orange rect { fill: #f0d3bf; }
.pele .purple rect { fill: #e3d5ff; }
.pele .yellow rect { fill: #f0dcbf; }
.pele .teal rect { fill: #bfe3df; }
.pele .tan rect { fill: #e5d9ce; }
.pele .pink rect { fill: #ffcbe6; }
.pele .breaks rect {
  fill: #ffecee !important;
  stroke: #e72e3b !important;
  stroke-width: 2.5px !important;
}
.pele .breaks text {
  fill: #cd2834 !important;
}
.pele .mayBreak rect {
  fill: #fff3e6 !important;
  stroke: #e8730c !important;
  stroke-width: 2.5px !important;
}
.pele .mayBreak text {
  fill: #b85a06 !important;
}
.pele .bugPath rect {
  stroke: #1680f3 !important;
  stroke-width: 2.5px !important;
}
.pele .repeats rect {
  stroke: #9b35d6 !important;
  stroke-width: 2.5px !important;
  stroke-dasharray: 6 3;
}
@media (prefers-color-scheme: dark) {
  .pele {
    --pele-bg: #1f1f1f;
    --pele-fg: #f5f5f5;
    --pele-muted: #acacac;
    --pele-line: #939393;
    --pele-surface: #121212;
    --pele-surface-alt: #2c2c2c;
    --pele-border: #4e4e4e;
  }
  .pele .blue rect { fill: #1b3857; }
  .pele .orange rect { fill: #573117; }
  .pele .purple rect { fill: #3c2e57; }
  .pele .yellow rect { fill: #573d17; }
  .pele .teal rect { fill: #174641; }
  .pele .tan rect { fill: #3d3127; }
  .pele .pink rect { fill: #57233e; }
  .pele .breaks rect {
    fill: #2e2021 !important;
    stroke: #cf2b35 !important;
  }
  .pele .breaks text {
    fill: #ff6463 !important;
  }
  .pele .mayBreak rect {
    fill: #2e2619 !important;
    stroke: #d0700f !important;
  }
  .pele .mayBreak text {
    fill: #ffa257 !important;
  }
  .pele .bugPath rect {
    stroke: #116dd2 !important;
  }
  .pele .repeats rect {
    stroke: #9a3fcf !important;
  }
}
```
