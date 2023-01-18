# タイトル

応用プログラミングレポート @G184582021 對馬　由敬

## 作品の概要

下の図のような自分が操作するxwingと自動に異なる速度で動くxwingが銃弾を撃ち合い、得点を増やしていきライフが0になるとリセットされる、スペースインベーダーに似たゲームを作った。
外側にある枠は第5回で作ったピンボールのものを使った。
自分で操作するxwingは自分のマウスカーソルにくっついて動くようになっている。銃弾はクリックをすると発射される仕組みになっている。銃弾の当たり判定は3機ともに翼の根本にあり、自分の銃弾が敵機のそこに当たると500点加算され、敵機の銃弾が自分のそこに当たるとライフが1つ減るようになっている。

## 代表的な変数の説明

* `xwing1,xwing2,xwing3` : xwingを1つずつ格納している変数。1つにまとめると別々の動きをすることができないので3つに分けている。モデルは gltf 型式だったので，読み込みには `GLTFLoarder` を使用した。

* `center` : 銃弾の当たり判定を測定するためのpaddleを格納している変数。3つのxwingにそれぞれ別々のものを作ることでバラバラに動くxwingに対応できるようにしている。

*　`ball,ball2,ball3` : これらはxwingから放たれるビームを格納している変数。わかりやすいように自分で操作しているxwingと敵機とで色を分けている。銃弾の速度もxwingと同様に異なる速度で進むようにしマンネリ化しないようにしている。

## 工夫(苦心)したところ

*　マウスカーソルに合わせてxwingを動かすために第5回のときはx軸での設定だけだったところをｚ軸のものも追加することで自由に動くようにした。そのうえで外枠からはみ出すことがないようにすることで枠からマウスカーソルがはみ出しても枠内にxwingがとどまるようにした。

```javascript
      mouse.x = (event.clientX / window.innerWidth) * 2 - 1;
      mouse.y = (event.clientY / window.innerHeight) * 2 - 1;
      raycaster.setFromCamera(mouse, camera);
      raycaster.ray.intersectPlane(plane, intersects);
      const offset1 = hFrameW / 2 - vFrameW - 1.5 / 2 - 0.5;
      const offset2 = vFrameD / 2 - hFrameD - 3 / 2 - (-0.2);
      if (intersects.x < - offset1) {
        intersects.x = - offset1;
      }
      else if (intersects.x > offset1) {
        intersects.x = offset1;
      }
      if (intersects.z < - offset2) {
        intersects.z = - offset2;
      }
      else if (intersects.z > offset2) {
        intersects.z = offset2;
      }
      xwing1.position.x = intersects.x;
      xwing1.position.z = -intersects.z;
      xwing1.rotation.y = 9.43;
```

*　敵機のxwingは一定のコースを動き続けてほしいので横に動くように制御点を設定した。

```javascript
      const controlPoints = [
    [5, 0, -9],
    [-5, 0, -9],
  ]
  // コースの補間
  const p0 = new THREE.Vector3();
  const p1 = new THREE.Vector3();
  const course = new THREE.CatmullRomCurve3(
    controlPoints.map((p, i) => {
      p0.set(...p);
      p1.set(...controlPoints[(i + 1) % controlPoints.length]);
      return [
        (new THREE.Vector3()).copy(p0),
      ];
    }).flat(), true
  )
```

*　

*

## 感想
