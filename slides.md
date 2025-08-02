---
theme: seriph
title: Proxmoxでつくる快適お家リージョンのすゝめ
class: text-left
drawings:
  persist: false
transition: fade
mdc: true
seoMeta:
  ogImage: auto
fonts:
  sans: "Zen Old Mincho"
---

# Proxmoxでつくる快適
# お家リージョンのすゝめ
## MCC部長 upiscium

<style>
h1 {
    background-color: #2B90B6;
    background-image: linear-gradient(45deg, #FF7300 1%, #BB3000 90%);
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
</style>

---
transition: fade
layout: image-right
image: ./images/logo.png
---

# 自己紹介

<div v-click>

## upiscium

- 東京農工大学 学部3年
- MCC 部長

- 趣味
  - プログラミング
  - サーバー構築
  - ゲーム(Minecraft)
  - 音楽鑑賞(VOCALOID, クラシック)
- Github: @upiscium
- Twitter: @upiscium

</div>

<style>
h1 {
    background-color: #FF7300;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
    transition: opacity 500ms ease;
}
</style>

---
transition: fade
layout: center
---

# 突然ですが

<div v-click>

# <p1>Proxmox</p1>を知っていますか？

</div>

<style>
h1 {
    background-color: #FFFFFF;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p1 {
    background-color: #FF7300;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
    transition: opacity 500ms ease;
}
</style>

---
transition: fade
layout: image-right
image: ./images/proxmox-top.png
---

# <p1>Proxmoxとは</p1>

<div v-click>

### オープンソースの
### 仮想化プラットフォーム

</div>

<br>

<div v-click>

### <p2>特徴</p2>
- 複数サーバをnodeとして統合管理できる
- VMをGUIで簡単に管理できる
- KVMとLXCが使える

</div>

<br>
<br>

<div v-click>

# <p3>とにかく楽しい！！！</p3>
# <p3>ぜひ触ってほしい！</p3>

</div>

<style>
p1 {
    background-color: #FF7300;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p2 {
    background-color: #0D8;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p3 {
    background-color: #0D8;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
    transition: opacity 500ms ease;
}
</style>

---
transition: fade
layout: center
---

# 本題の<p1>お家リージョン</p1>紹介

<style>
h1 {
    background-color: #FFFFFF;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p1 {
    background-color: #FF7300;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
</style>

---
transition: fade
---

# 物理サーバ構成

- 物理サーバを4台運用中

<div v-click>

それぞれのスペックは以下の通り

| <p1>サーバ名</p1> | <p1>CPU</p1> | <p1>RAM</p1> | <p1>ストレージ</p1> | <p1>GPU</p1> |
| --- | --- | --- | --- | --- |
| Gabriel | Ryzen 7 8700G (16コア) | 128GB | 1TB | NVIDIA GeForce RTX 3060 x2 |
| Metatron | Intel Core i3-7100 (4コア) | 40GB | 128GB | - |
| Raphael | Ryzen 7 5700G (8コア) | 32GB | 512GB | - |
| Zadkiel | Intel Core i9-13900KF (32コア) | 128GB | 1TB | NVIDIA GeForce RTX 4080 |
| Total | 68コア | 328GB | 2.64TB | 3GPU |

</div>

<style>
h1 {
    background-color: #FF7300;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p1 {
    background-color: #0D8;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
    transition: opacity 500ms ease;
}
</style>

---
transition: fade
---

# 全体構成

<div v-click>

```mermaid
flowchart TB

subgraph PS1[ホスト1]
    VS1[仮想スイッチ]
    VM1[GPUサーバ1]
    VM2[GPUサーバ2]
    VM3[Minecraftサーバ]
    VM4[試験用サーバ]

    VS1 <--> VM1
    VS1 <--> VM2
    VS1 <--> VM3
    VS1 <--> VM4
end

subgraph PS2[ホスト2]
    VS2[仮想スイッチ]
    LXC1[Nextcloudサーバ]
    LXC2[Apacheサーバ]
    VM5[コンテナサーバ]

    VS2 <--> LXC1
    VS2 <--> LXC2
    VS2 <--> VM5
end

subgraph PS3[ホスト3]
    VS3[仮想スイッチ]
    VM6[WireGuardサーバ]
    VM7[DNSサーバ]
    VM8[Nginxサーバ]

    VS3 <--> VM6
    VS3 <--> VM7
    VS3 <--> VM8
end

subgraph PS4[ホスト4]
    VS4[仮想スイッチ]
    VM9[Windowsクライアント]

    VS4 <--> VM9
end

Router[ルーター]

Router <--> VS1
Router <--> VS2
Router <--> VS3
Router <--> VS4

Internet[インターネット]
Internet <--> Router
```

→ 仮想マシン: 9台 / LXC: 2台 を稼働中
</div>

<br>

<div v-click>

<div class="flex">

<p1>

### 構成のポイント

- WireGuardを使った自前VPNホスティング
- DNSサーバによるローカル名前解決
- Nginxを使ったリバースプロキシ

</p1>

<img class="w-40" src="./images/servers.jpg" alt="Proxmox server">
<img class="w-40" src="./images/servers2.jpg" alt="Proxmox server">

</div>
</div>

<style>
h1 {
    background-color: #FF7300;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
h3 {
    background-color: #0D8;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
    transition: opacity 500ms ease;
}
.flex>img{
    padding: 10px;
}
</style>

---
transition: fade
layout: center
---

# ここからは実際に運用している
# <p1>主要サービス</p1>を紹介します

<style>
h1 {
    background-color: #FFFFFF;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p1 {
  background-color: #FF7300;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
  transition: opacity 500ms ease;
}
</style>

---
transition: fade
---

# 主要サービス紹介(1)

<div v-click>
<div class="flex">

<p1>

### WireGuardサーバ
- 外からでも安全にアクセスできる(TailScaleで良いとか言わない)
- KDE Connectがどんなネットワークでも使える

</p1>

<img class="h-13" src="https://upload.wikimedia.org/wikipedia/commons/9/98/Logo_of_WireGuard.svg" alt="WireGuard Logo">

</div>
</div>

<br>

<div v-click>
<div class="flex">

<p1>

### DNSサーバ
- いちいちIPアドレスを覚えなくて良い

</p1>

<img class="h-16" src="https://upload.wikimedia.org/wikipedia/commons/2/2c/Dnsmasq_icon.svg" alt="BIND Logo">

</div>

- ホスト名でサービスにアクセスできる(ホスト名がサービス名で無いのはご愛嬌)

</div>

<br>

<div v-click>
<div class="flex">

<p1>

### Nginxサーバ
- 煩雑なポート番号を気にしなくて良い!
- あのサービスのポート何だっけ? からの解放

</p1>

<img class="h-13" src="https://upload.wikimedia.org/wikipedia/commons/c/c5/Nginx_logo.svg" alt="Nginx Logo">

</div>
</div>

<br>

<div v-click>

※ 実は全部同一ホストで稼働中 → このホストが落ちるとインフラが逝ってしまう素晴らしい仕様.
<br>
　こんなに大事なホストなのに中古PCとかいう意味不明な構成

</div>

<style>
h1 {
  background-color: #FF7300;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
h3 {
  background-color: #0D8;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
  transition: opacity 500ms ease;
}
</style>

---
transition: fade
---

# 主要サービス紹介(2)

<div v-click>
<div class="flex">

<p1>

### Nextcloudサーバ
- 自前のクラウドストレージで何でも共有できる
- 画像や動画, ドキュメントの共有も簡単
- 4TBストレージで何でも入れられるからとてもハッピー
- これが無いと何もできない

</p1>

<img class="h-30" src="./images/nextcloud.png" alt="Nextcloud Logo">
<img class="h-30" src="./images/nextcloud2.jpg" alt="Nextcloud Logo">

</div>
</div>

<br>

<div v-click>
<div class="flex">

<p1>

### GPUサーバ
- RTX3060を1枚ずつ別VMに割り当て
- OllamaでローカルLLMを稼働中
- Stable Diffusionで画像生成もする予定

※ RTX4080はWindowsでゲームする用

</p1>

<img class="h-50" src="./images/gpu.jpg" alt="NVIDIA Logo" gap="20">
<img class="h-50" src="./images/gpu2.jpg" alt="NVIDIA Logo" gap="20">

</div>
</div>

<style>
h1 {
  background-color: #FF7300;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
h3 {
  background-color: #0D8;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
  transition: opacity 500ms ease;
}
.flex>img{
  padding: 10px;
}
</style>

---
transition: fade
layout: center
---

# <p1>お家リージョン</p1>のメリット

<style>
h1 {
    background-color: #FFFFFF;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p1 {
  background-color: #FF7300;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
  transition: opacity 500ms ease;
}
</style>

---
transition: fade
layout: image-right
image: ./images/proxmox-top.png
---

## 好き放題できる
- 自分のサーバだから何でもできる
- メモリやストレージを気にせず使える
- ハードからソフトまで全部好きにできる

<br>

<div v-click>

## 勉強になる
- サーバ構築やネットワークの勉強になる
- Linuxの知識が身につく
- セキュリティや運用の知識も得られる

</div>

<br>

<div v-click>

## コストが安い(要出典)
- 初期投資が必要になる <br>
→ 買ってしまえば維持費だけ
- クラウドサービスの料金を気にしなくて良い
- 自宅運用だから維持費は電気代のみ

</div>

<style>
h2 {
  background-color: #FF7300;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
  transition: opacity 500ms ease;
}
</style>

---
transition: fade
layout: center
---

# ここで実際に運用しているサービスを<p1>実演</p1>します！

<style>
h1 {
    background-color: #FFFFFF;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p1 {
  background-color: #FF7300;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
  transition: opacity 500ms ease;
}
</style>

---
transition: fade
layout: center
---

# 何よりも一番のメリットは<p1>楽しい</p1>こと!
# <p1>Proxmox</p1>を使って<p1>お家リージョン</p1>を作ろう！

<style>
h1 {
    background-color: #FFFFFF;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
p1 {
  background-color: #FF7300;
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
.slidev-vclick-target {
  transition: opacity 500ms ease;
}
</style>

---
transition: fade
layout: center
---

# ご清聴ありがとうございました！

<style>
h1 {
    background-color: #999;
    background-size: 100%;
    -webkit-background-clip: text;
    -moz-background-clip: text;
    -webkit-text-fill-color: transparent;
    -moz-text-fill-color: transparent;
}
</style>

