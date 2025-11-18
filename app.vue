<template>
<!-- データがロードされ、かつ detailsが存在する場合のみメインコンテンツをレンダリング -->
<main v-if="response?.details">

    <!-- ヘッダー: v-once を追加し、データが確定するまで描画をブロックし、初期のクラッシュを防ぐ -->
    <header 
        class="header" 
        :style="{ 
            backgroundImage: `url(${response.details.ext_1?.url || ''})` 
        }"
        v-once
    >
        <div class="header__text">
            <h1>{{ response.details.ext_2 }}</h1>
            <p>{{ response.details.ext_3 }}</p>
        </div>
    </header>

    <!-- WORKS セクション -->
    <section v-if="response.details.ext_4?.length">
        <h2>WORKS</h2>
        <ul>
            <!-- リスト要素の画像アクセスにもオプションチェイニングを使用 -->
            <li v-for="n in response.details.ext_4" :key="n.slag" class="works__item">
                <img :src="n.ext_4?.url" alt="Work Image" />
                <div class="works__item__text">
                    <h3>{{ n.ext_5 }}</h3>
                    <p>{{ n.ext_6 }}</p>
                </div>
            </li>
        </ul>
    </section>

    <!-- ABOUT セクション -->
    <section class="about">
        <h2>ABOUT</h2>
        <!-- v-html はデータが存在することを確認してから実行 -->
        <p v-html="response.details.ext_7"></p>
    </section>
</main>
<!-- ロード中の代替表示 (データがない場合にクラッシュしないよう、メインの外で表示) -->
<div v-else class="text-center p-8 text-xl text-gray-500">
    データを読み込み中、またはデータがありません...
</div>


</template>

<script setup>
// Nuxtのランタイム設定を取得
const config = useRuntimeConfig();

// データの非同期取得
// dataオブジェクト全体を response として受け取る
const { data: response } = await useFetch(
${config.public.apiBase}/rcms-api/3/service/3,
{
credentials: 'include',
}
);
</script>

<style>
/* ------------------------------------ /
/ ユーザー提供のCSSスタイル (変更なし) /
/ ------------------------------------ */

body {
margin: 0;
font-size: 1em;
line-height: 1.5;
}

ul {
margin: 0;
padding: 0;
list-style: none;
}

img {
max-width: 100%;
}

section {
max-width: 1200px;
margin: 3em auto;
padding: 0 20px;
}

h1 {
margin: 0.5em 0;
font-size: 1.8em;
}

h2 {
margin: 2em auto;
font-size: 1.5em;
text-align: center;
}

h3 {
margin: 1em auto;
font-size: 1.2em;
}

p {
margin: 1em 0;
font-size: 0.75em;
}

.header {
position: relative;
width: 100%;
height: 300px;
background-position: center center;
background-size: cover;
background-color: #f0f0f0;
}

.header__text {
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
height: 100%;
padding: 0 20px;
}

.works__item:not(:first-child) {
margin-top: 3em;
}

@media screen and (max-width: 767px) {
.header::before {
width: 100%;
height: 300px;
position: absolute;
top: 0;
left: 0;
background-color: rgba(0, 0, 0, 0.5);
z-index: 1;
content: '';
}

.header__text {
position: absolute;
top: 0;
left: 0;
color: #fff;
z-index: 10;
}
}

@media screen and (min-width: 768px) {
body {
font-size: 2em;
}

.header {
height: 600px;
}

.header__text {
width: 1200px;
align-items: flex-end;
margin: auto;
}

.header__text p {
width: 600px;
}

.works__item {
display: flex;
}

.works__item img {
width: 400px;
margin-right: 2em;
}

.about {
text-align: center;
}
}
</style>