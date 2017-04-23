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

`v-for` ディレクティブを使うことで配列の要素を描画することができます。

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
            <tr v-for="user in users">
                <td>{{ user.name }}</td>
            </tr>
        <tbody>
    </table>
</div>
```

### 画面上で要素の追加入力

配列の内容を変更すると連動して表の内容も再描画されます。

上記の状態でブラウザのコンソールから配列に要素を追加してみましょう。

```js
vm.users.push({name: 'Dave'});
```

これを利用して入力フォームから要素を追加入力できるようにします。

#### v-model ディレクティブ

Vue インスタンスのプロパティと `<input>` 要素の入力内容を結びつけるには `v-model` ディレクティブを使用します。

まず、`data` プロパティに下記を追加します。

```html
<script type="text/javascript">
var vm = new Vue({
    el: '#app',
    data: {
        users: [
            // 略
        ],
        // 下記を追加
        newUserName: ''
    }
});
</script>
```

そして、HTML 側に `<input>` 要素を追加します。

```html
<div id="app" class="container">
    <table class="table table-striped">
        <!-- 略 -->
    </table>
    <!-- 下記を追加 -->
    <input v-model="newUserName">
</div>
```

この状態で、試しに何か入力した状態でコンソールから `newUserName` プロパティの値を確認してみてください。

```js
vm.newUserName
```

また、`newUserName` プロパティを別の値で上書きすると `<input>` 要素の内容も連動することを確認してみてください。

```js
vm.newUserName = 'Dave'
```

#### methods プロパティ

次に、入力内容を配列に追加するメソッドを作成します。

Vue インスタンスの `methods` プロパティに関数を記述することで、メソッドを作成できます。

このとき、Vue インスタンス自身には `this` でアクセスすることができます。

```html
<script type="text/javascript">
var vm = new Vue({
    // 略
    methods: {
        append: function () {
            this.users.push({name: this.newUserName});
            this.newUserName = '';
        }
    }
});
</script>
```

#### v-on ディレクティブ

最後に、Enter キーの keyup イベントをハンドリングして、 作成した `append` メソッドが呼び出されるようにします。

イベントハンドリングを行うには `v-on` ディレクティブを使用します。

`<input>` 要素を次のように書き換えてください。

```html
    <input v-model="newUserName" v-on:keyup.enter="append">
```

これで、入力後に Enter キーを押すことで要素が追加されるようになります。

なお `v-on:` は省略記法として `@` で代用することができます。

```html
    <input v-model="newUserName" @keyup.enter="append">
```

### [応用] 要素の削除

ここまでの内容を応用して、クリックすると要素を削除するボタンを作成してみましょう。

HTML 部分を次のように変えると、各行に「×」マークが表示されます。

```html
<div id="app" class="container">
    <table class="table table-striped">
        <thead>
            <tr>
                <th>Name</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(user, index) in users">
                <td>{{ user.name }}</td>
                <td><span @click="remove(index)" class="glyphicon glyphicon-remove"></span></td>
            </tr>
        <tbody>
    </table>
    <input v-model="newUserName" @keyup.enter="append">
</div>
```

「×」マークをクリックした際に該当行が削除されるようにしてみましょう。


Hint 1: `v-for="(user, index) in users'` とすることで、ループ中の要素番号（0開始）に `index` でアクセスすることができます

Hint 2: 例えば 3 番目の要素を削除するとき、コンソール上では次のようにします

```js
vm.users.splice(2, 1);
```

Hint 3: `v-on` ディレクティブで呼び出すメソッドには `v-on:click="remove(index)"` のようにして引数を渡すことができます

### コンポーネント

Vue.js では「コンポーネント」を利用することでコードをカプセル化し、再利用しやすい形にすることができます。

コンポーネント指向とは Vue.js 固有ではなく一般的な概念です。

詳細な解説は割愛しますが（私もたいして理解していない）、Vue.js においてはとりあえず下記を認識しておいてください。

* コンポーネントは親子関係を持たせられる
* 子から親のデータを直接参照できない
* 親はプロパティを介して子にデータを伝達できる
* 子はイベントを発火することで親に情報を知らせる

Vue.js におけるコンポーネントの詳細については公式ドキュメントを参考にしてください。

[コンポーネント - Vue.js # コンポーネントの構成](https://jp.vuejs.org/v2/guide/components.html#コンポーネントの構成)

例として `user` をコンポーネント化してみましょう。

コンポーネントを作成するには次のように記述します。

```js
Vue.component('user', {
    props: ['name'],
    template:
    '<tr>\
        <td>{{ name }}</td>\
        <td><span @click="remove" class="glyphicon glyphicon-remove"></span></td>\
    </tr>',
    methods: {
        remove: function (index) {
            this.$emit('remove');
        }
    }
});
```

`props` は親コンポーネントから渡されるプロパティです。

親コンポーネントからは直接値を指定する方法と、 `v-bind` ディレクティブを利用してデータを動的に結びつける方法があります。

今回のハンズオンでは後者のみを利用します（後ほど説明）。

`template` は文字通りコンポーネントのテンプレートです。

テンプレート内ではプロパティの値を参照できます。

また、コンポーネントも `methods` を持つことができます。

ただし、`remove` メソッドは要素の削除を行っていないことに留意してください。

`$emit` を用いると任意の名前のイベントを発火することができます。

ここでは remove という名前のイベントを発火しているだけに留まり、親コンポーネントでそのイベントをハンドリングします。


上記だけではわかりにくいので全体のコードを見てましょう。

```html
<script type="text/javascript">

Vue.component('user', {
    props: ['name'],
    template:
    '<tr>\
        <td>{{ name }}</td>\
        <td><span @click="remove" class="glyphicon glyphicon-remove"></span></td>\
    </tr>',
    methods: {
        remove: function (index) {
            this.$emit('remove');
        }
    }
});

var vm = new Vue({
    el: '#app',
    data: {
        users: [
            {name: 'Alice'},
            {name: 'Bob'},
            {name: 'Carol'}
        ],
        newUserName: '',
    },
    methods: {
        append: function () {
            this.users.push({name: this.newUserName});
            this.newUserName = '';
        },
        remove: function (index) {
            this.users.splice(index, 1);
        }
    }
});

</script>
```

親コンポーネントとなる Vue インスタンスには変更はありません。

```html
<div id="app" class="container">
    <table class="table table-striped">
        <thead>
            <tr>
                <th>Name</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            <tr
                is="user"
                v-for="(user, index) in users"
                v-bind:name="user.name"
                @remove='remove(index)'
            ></tr>
        <tbody>
    </table>
    <input v-model="newUserName" @keyup.enter="append">
</div>
```

`<tbody>` の中身がコンポーネントによってカプセル化されています。

`is` 属性によってコンポーネントを指定しています。

また、`v-bind` ディレクティブを用いて `user.name` を子コンポーネントのプロパティ `name` に結びつけています。

なお `v-bind:` は省略記法として `:` で代用することができます。

`:name="user.name"`

更に、`@`（`v-on:` の省略記法）を使って `remove` イベントをハンドリングしています。

子コンポーネントから `remove` イベントが送出されると、該当する要素を削除（自身の `remove` メソッドを呼び出し）するようになっています。

以上でこれまでに作ってきたものをコンポーネント化することができました。

コンポーネント化により親と子が疎結合になり、再利用しやすくなっていることがわかると思います。
