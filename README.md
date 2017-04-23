# Vue.js からはじめる JS フレームワーク

この資料は 2017/04/24 開催の勉強会「Vue.js からはじめる JS フレームワーク」用の資料です。


## イベント詳細

[【勉強会】Vue.js からはじめる JS フレームワーク（ハンズオン形式） - サポーターズCoLab](https://supporterzcolab.com/event/8/)


## 進め方

handson.html というファイルがあるので、このファイルをダウンロードし下記お題に沿って追記していってください。

## ハンズオン

### Hello World

`<script>` タグ内で Vue インスタンスを生成します。

慣習として変数名に `vm` （ViewModel の略） を使います。

```html
<script type="text/javascript">
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello World'
    }
});
</script>
```

HTML 側で `{{ }}` を用いることで変数を展開できます。

```html
<div id="app" class="container">
    {{ message }}
</div>
```

ブラウザで handson.html を開き、 'Hello World' と表示されている事を確認してください。

なお、コンソールから Vue インスタンスのプロパティを書き換えると、表示内容も更新されます。

```js
vm.message = 'Hello Vue'
```

### 配列の要素の描画

v-for を使うことで配列の要素を描画することができます。

```html
<script type="text/javascript">
var vm = new Vue({
    el: '#app',
    data: {
        users: [
            {name: 'Alice'},
            {name: 'Bob'},
            {name: 'Carol'}
        ]
    }
});
</script>
```

```html
<div id="app" class="container">
    <table class="table table-striped">
        <thead>
            <tr>
                <th>Name</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for='user in users'>
                <td>{{ user.name }}</td>
            </tr>
        <tbody>
    </table>
</div>
```
