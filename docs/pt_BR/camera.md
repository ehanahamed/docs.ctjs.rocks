# camera

::: tip Hey,
Essa página descreve os métodos e parâmetros do objeto `camera` em forma de referência. Você pode aprender mais sobre técnicas e uso de forma menos formal na [página "Trabalhando com o Viewport"](/viewport-management.html).
:::

## Geometria da Câmera

### `camera.x`, `camera.y`

As coordenadas x e y reais da câmera. Nela não está incluso o efeito de tremer a tela, assim como, pode haver divergência entre `targetX` e `targetY` se a câmera estiver em transição.

### `camera.targetX` e `camera.targetY`

As coordenadas x e y do local de destino. Movendo dessa forma em vez de apenas usar o parâmetros `x`/`y` acionará o efeito de desvio.

### `camera.computedX`, `camera.computedY`

A posição resultante da câmera em coordenadas de jogo. Nela está incluso o efeito de tremer a tela, `camera.shiftX` e `camera.shiftY`.

### `camera.width`, `camera.height`

A largura e altura da câmera sem redimensionamento da região mostrada. Use o `camera.scale` para obter a versão com redimensionamento. Para alterar esses valores, veja as propriedades `ct.width` e `ct.height`.

### `camera.rotation`

O valor de rotação da câmera em graus.

### `camera.scale.x`, `camera.scale.y`

O valor de redimensionamento do retângulo de captura. Se comparado com as ferramentas de visualização de imagens, `1` e `1` significa sem redimensionamento, enquanto que `0.5` aumentará o zoom para uma visualização de 200%, já o valor `3` diminuirá o zoom e produzirá um redimensionamento para 33%.

### `camera.left`, `camera.top`, `camera.right` e `camera.bottom`

Eles representam a localização resultante de um lado específico da câmera em unidades de jogo. Os mesmos não podem ser alterados manualmente.

### `camera.moveTo(x, y)` e `camera.teleportTo(x, y)`

Ambos movem a camera para uma nova posição. `camera.moveTo` é útil para os cortes de cena e transições suaves entre objetos, porque ele funciona com o `camera.drift`. `camera.teleportTo` não faz transições e reseta os efeitos de tremer a tela. Ele é útil para alterar a posição da câmera de forma precisa e imediata, como por exemplo, quando precisamos mover a posição da camera para uma local distante.

### `camera.uiToGameCoord(x, y)` e `camera.gameToUiCoord(x, y)`

Converte um ponto de um espaço de coordenadas para outro. Retorna um objeto (`PIXI.Point`) com duas propriedades: as coordenadas`x` e `y`.

Também existe o `ct.u.uiToGameCoord` e `ct.u.gameToUiCoord`, que chamam esses métodos do objeto `camera` atual.

### `camera.getTopLeftCorner()`, `camera.getTopRightCorner()`, `camera.getBottomLeftCorner()`, `camera.getBottomRightCorner()`

 Retorna um objeto (`PIXI.Point`) com duas propriedades: as coordenadas`x` e `y`. Essas são as coordendas de jogo e está incluso na conta a rotação e o redimensionamento.

### `camera.getBoundingBox()`

Retorna uma caixa retangular representando os limites da câmera, em coordenadas de jogo. Para essas prorpiedades veja [PIXI.Rectangle](https://pixijs.download/release/docs/PIXI.Rectangle.html).

## Seguindo uma Copy

### `camera.follow`

Se definido, a câmera seguirá a copy passada.

### `camera.followX`, `camera.followY`

Funciona apenas se `follow` foi definido para uma copy. Definindo um desses para `false` irá automaticamente desabilitar a câmera para a direção determinada.

### `camera.borderX`, `camera.borderY`

Funciona apenas se `follow` foi definido para uma copy. Define o limite no qual a copy será mantida, em coordenadas de UI. Pode ser `null` para que a copy seja definida no centro da tela.

## Oscilação e Tremor de Tela

### `camera.shake`

A potência atual do efeito de tremer a tela, em relação ao lado máximo da tela (100 é 100% do tremor de tela). Se o valor 0 ou menor for definido, então o efeito será desabilitado.

### `camera.shakePhase`

A fase atual da oscilação do tremor de tela.

### `camera.shakeDecay`

A quantidade de unidades de `shake` (tremores) subtraídos em um segundo. O padrão é 5.

### `camera.shakeFrequency`

A frequência base do efeito de tremer a tela. O padrão é 50.

### `camera.shakeX`, `camera.shakeY`

Um multiplicador aplicado ao efeito de tremer a tela. O padrão é 1.

### `camera.shakeMax`

O valor máximo possível para a propriedade `shake` com o objetivo de proteger o jogador de perder seu monitor, em unidades de `shake`. O padrão é 10.

### `camera.minX`, `camera.maxX`, `camera.minY`, e `camera.maxY`

Isso faz com que a câmera seja movida dentro do retângulo determinado. Por padrão, a câmera pode se mover sem limites, a menos que isso seja definido de alguma outra forma dentro do editor de room.

Você pode definir todas essas propriedades ou apenas algumas delas.

Para redefinir algum desses valores, use por exemplo, `delete camera.minX;` ou escreva `camera.minX = undefined;`.

## Outros

### `camera.drift`

Se definido um valor entre 0 e 1, ele fará o movimento da camera ser mais suave.

### `camera.shiftX`, `camera.shiftY`

Eles deslocam a câmera em unidades de UI, mas não alteram o `camera.x` ou `camera.y`.

### `camera.realign(room)`

Realinha todas as copies em uma room de acordo com as novas dimensões da câmera. Isso é útil para um posicionamento rápido dos elementos de UI em diferentes telas. A nova posição é o resultado da interpolação tendo como base os parâmetros `xstart` e `ystart` das copies, portanto, não funcionará com elementos em movimento. Você pode pular o realinhamento para algumas copies se você definir o parâmetro `skipRealign` para `true`.

Normalmente o método é aplicável apenas aos modos "Expand" e "Scaling without letterboxing" de fittoscreen (ajustar a tela).

Sem `camera.realign` | Com `camera.realign`
-|-
![Os elementos de UI são redimensionados, mas aparecem deslocados se as proporções de tela mudarem](../images/ctCameraAlign_notIncluded.gif) | ![Os elementos de UI são redimensionados e distribuídos uniformemente pela tela](../images/ctCameraAlign_included.gif)

#### Exemplo: Realinhando os elementos de UI em uma room

Código na aba `Draw` da sua room:

```js
camera.realign(this);
```

Sim, é isso!

### `camera.manageStage()`

Isso permite realinhar todas as camadas que não seja de UI no jogo de acordo com a transformação da câmera. Esse é um processor feito de forma automática pelo ct.js, então você dificilmente vai precisar usar.

## Criando uma nova instância da câmera

Você pode criar um novo objeto câmera para os seus propositos apenas chamando o construtor `Camera`:

```js
const oldCamera = camera;
camera = new Camera(0, 0, 1024, 768);
```
