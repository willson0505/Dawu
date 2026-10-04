<!DOCTYPE html>
<html lang="zh-TW">

<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>《日治時期森林計畫事業區分調查史料》解讀──大武調查區概覽</title>

<!-- MapLibre GL -->
<link href="https://unpkg.com/maplibre-gl@3.6.2/dist/maplibre-gl.css" rel="stylesheet" />
<script src="https://unpkg.com/maplibre-gl@3.6.2/dist/maplibre-gl.js"></script>

<!-- Scrollama -->
<script src="https://unpkg.com/scrollama"></script>

<style>
/* =========================================================
   基本與版面設定
========================================================= */
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

html, body {
    width: 100%;
    min-height: 100%;
}

body {
    font-family: "PingFang TC", "Microsoft JhengHei", "Noto Sans TC", sans-serif;
    color: #2b2b2b;
    line-height: 2.1;
    background: #fdfbf7;
    overflow-x: hidden;
}

#container {
    display: flex;
    flex-direction: row;
    min-height: 100vh;
    position: relative;
}

/* 左側論文區：擴寬至 58vw */
#story {
    width: 58vw;
    padding: 50px 60px 120px 60px;
    z-index: 2;
    background: #fdfbf7;
}

/* 右側地圖區：42vw 固定欄位 */
#map-container {
    width: 42vw;
    height: 100vh;
    position: sticky;
    top: 0;
    background: #e5e3df;
    box-shadow: -4px 0 20px rgba(0,0,0,0.08);
}

#map {
    width: 100%;
    height: 100%;
}

/* =========================================================
   SVG 動態連線畫布
========================================================= */
#svg-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    pointer-events: none;
    z-index: 9999;
}

/* =========================================================
   論文完整內文樣式
========================================================= */
.paper-container {
    width: 100%;
    max-width: 900px;
    margin: 0 auto;
}

h1.main-title {
    font-size: 2rem;
    line-height: 1.4;
    font-weight: 700;
    margin-bottom: 10px;
    color: #1a2a1d;
    border-bottom: 3px solid #1a2a1d;
    padding-bottom: 14px;
}

.subtitle {
    font-size: 1.1rem;
    color: #666;
    margin-bottom: 40px;
    font-weight: 500;
}

.paper-section {
    background: #ffffff;
    padding: 38px 42px;
    border-radius: 12px;
    margin-bottom: 60px;
    box-shadow: 0 4px 24px rgba(0,0,0,0.05);
    border-left: 6px solid #2b580c;
    transition: all 0.3s ease;
}

.paper-section.is-active {
    border-left-color: #d9381e;
    box-shadow: 0 6px 30px rgba(217, 56, 30, 0.1);
}

p {
    font-size: 1.08rem;
    text-align: justify;
    margin-bottom: 1.6em;
    text-indent: 2.2em;
    color: #333333;
}

h2.section-title {
    font-size: 1.4rem;
    color: #1a2a1d;
    margin: 1.2em 0 0.8em 0;
    padding-bottom: 6px;
    border-bottom: 1.5px solid #e0dcd3;
}

.tribe-list-box {
    background: #fdfaf5;
    border: 1px solid #eae5d9;
    padding: 20px 24px;
    border-radius: 8px;
    margin: 1.5em 0;
    text-indent: 0;
}

/* =========================================================
   地名互動連結標籤 (Hover 特效)
========================================================= */
.location-link {
    color: #a82810;
    font-weight: 600;
    border-bottom: 2px dashed #a82810;
    background: rgba(168, 40, 16, 0.08);
    padding: 2px 6px;
    margin: 0 2px;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.2s ease;
    display: inline-block;
    text-indent: 0;
}

.location-link:hover, .location-link.active-link {
    background: #a82810;
    color: #ffffff;
    border-bottom-color: transparent;
    box-shadow: 0 3px 10px rgba(168, 40, 16, 0.3);
    transform: translateY(-1px);
}

/* =========================================================
   右側圖層控制項
========================================================= */
.basemap-control {
    position: absolute;
    top: 18px;
    right: 18px;
    z-index: 10;
}

.basemap-btn {
    display: flex;
    align-items: center;
    gap: 8px;
    background: rgba(255, 255, 255, 0.95);
    color: #2b2b2b;
    border: 1px solid rgba(0, 0, 0, 0.15);
    padding: 9px 16px;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
    font-size: 14px;
    box-shadow: 0 3px 10px rgba(0,0,0,0.12);
}

.basemap-menu {
    position: absolute;
    top: 48px;
    right: 0;
    width: 250px;
    background: rgba(255, 255, 255, 0.96);
    border: 1px solid rgba(0, 0, 0, 0.15);
    border-radius: 8px;
    padding: 14px 16px;
    box-shadow: 0 6px 20px rgba(0,0,0,0.15);
}

.basemap-menu.hidden { display: none; }

.opacity-box {
    margin-top: 10px;
    padding-top: 8px;
    border-top: 1px dashed #cccccc;
    display: flex;
    flex-direction: column;
    gap: 4px;
}

@media (max-width: 900px) {
    #container { flex-direction: column-reverse; }
    #story { width: 100vw; padding: 24px; }
    #map-container { width: 100vw; height: 50vh; position: sticky; top: 0; }
}
</style>
</head>

<body>

<!-- SVG 動態連線畫布 -->
<svg id="svg-overlay">
    <defs>
        <filter id="glow" x="-20%" y="-20%" width="140%" height="140%">
            <feGaussianBlur stdDeviation="3" result="blur" />
            <feComposite in="SourceGraphic" in2="blur" operator="over" />
        </filter>
    </defs>
    <path id="connection-line" fill="none" stroke="#a82810" stroke-width="3" stroke-dasharray="6,4" filter="url(#glow)" style="display:none;" />
    <circle id="start-dot" r="5" fill="#a82810" style="display:none;" />
    <circle id="end-dot" r="7" fill="#a82810" stroke="#ffffff" stroke-width="2" style="display:none;" />
</svg>

<div id="container">

<!-- 左側論文完整內容區域 -->
<div id="story">
    <div class="paper-container">
        
        <h1 class="main-title">大武調查區概覽</h1>
        <div class="subtitle">《日治時期森林計畫事業區分調查史料》完整全文</div>

        <!-- Section 1 -->
        <div class="paper-section step-section" data-step="1">
            <p>
                本調查區包含臺東廳大武支廳內的國有林野，面積為 83,612 公頃。此區域原為第二次森林計畫地區的預定地，但已趁森林治水調查之便，實施區分調查。依此調查，規劃了蕃人生活用的區域，且此調查具備保護林野的取締效果；另一方面，亦釐清了第二次森林計畫裡的營林用要存置林野面積。由此可見此次調查的必要性。
            </p>
            <p>
                關於調查區的邊界：西方及南方通過大武山（海拔 10,665 日尺；3,232 公尺），以脊梁山脈的分水嶺—牡丹溪山（海拔 1,606 日尺；487 公尺）為界，與高雄州相鄰；北方依知本溪，與臺東支廳相接；南方則至海岸。
            </p>
        </div>

        <!-- Section 2: 壹、土地的狀況 -->
        <div class="paper-section step-section" data-step="2">
            <h2 class="section-title">壹、土地的狀況</h2>
            <p>
                關於林野的狀態、地勢，西方有高峰相連的脊梁山脈分水嶺。山脈的地勢向東漸低，山勢陡峭，其中有大麻溪、蚶子崙溪、大竹高溪、大武溪等諸溪流過，溪邊的山坡地遭溪流嚴重侵蝕：綜觀調查區各地的地勢，調查區南方從北邊起為緩坡，各山峰的山勢由北至南越緩。地質以黏板岩及砂岩為基岩，土壤質地為砂質壤土，地力一般。
            </p>
            <p>
                林況方面，原生林約佔全林地面積 7 成，其餘大部分的土地為蕃人開墾地，其中一半以上為叢林。9 成的原生林為闊葉樹的雜木林，但到了大武山附近的高地，有由自然生長的臺灣鐵杉、紅檜而成的針闊葉混合林。接著，就樹木生長情形來看，雖然海岸附近的樹木生長情形不佳，但隨著遠離海岸，情形逐漸變好。
            </p>
        </div>

        <!-- Section 3: 聚落與名冊 -->
        <div class="paper-section step-section" data-step="3">
            <p>
                關於調查區內居民的狀況，此區域原為「パイワン」（Paiwan）族太麻里蕃的根據地，現居此地的蕃人，在行政區有 17 個蕃社、人口有 3,527 人：蕃地上有 43 社、人口有 5,059 人，合計 8,586 人。此外，除少數內地人、本島人外，尚有平地蕃「アミ」（Ami）族 261 人與「パイワン」(Paiwan)族居住於此。行政區及蕃地上的現存蕃社名和人口如下：
            </p>

            <div class="tribe-list-box">
                <p style="text-indent: 0; font-weight: bold; margin-bottom: 8px;">行政區現存蕃社（17社，人口3,527人）：</p>
                <p style="text-indent: 0; margin-bottom: 0;">
                    <span class="location-link" data-name="太麻里">太麻里</span>、
                    <span class="location-link" data-name="羅打結">羅打結</span>、
                    <span class="location-link" data-name="鴨子蘭">鴨子蘭</span>、
                    <span class="location-link" data-name="文里格">文里格</span>、
                    <span class="location-link" data-name="猴子蘭">猴子蘭</span>、
                    <span class="location-link" data-name="蚶子崙">蚶子崙</span>、
                    <span class="location-link" data-name="大武窟">大武窟</span>、
                    <span class="location-link" data-name="察暗密">察暗密</span>、
                    <span class="location-link" data-name="打暗打蘭">打暗打蘭</span>、
                    <span class="location-link" data-name="大武">大武</span>、
                    <span class="location-link" data-name="鴿子籠">鴿子籠</span>、
                    <span class="location-link" data-name="大烏萬">大烏萬</span>、
                    <span class="location-link" data-name="拔子洞">拔子洞</span>、
                    <span class="location-link" data-name="獅子獅">獅子獅</span>、
                    <span class="location-link" data-name="大竹高">大竹高</span>、
                    <span class="location-link" data-name="甘壁壁">甘壁壁</span>、
                    <span class="location-link" data-name="大得吉">大得吉</span>，以上17社，人口3,527人。
                </p>
            </div>

            <div class="tribe-list-box">
                <p style="text-indent: 0; font-weight: bold; margin-bottom: 8px;">蕃地現存蕃社（43社，人口5,059人）：</p>
                <p style="text-indent: 0; margin-bottom: 0; line-height: 2.3;">
                    <span class="location-link" data-name="ルラクシ">「ルラクシ」（Rurakushi）社</span>、
                    <span class="location-link" data-name="ビララウ">「ビララウ」（Birarau）社</span>、
                    <span class="location-link" data-name="パウモリ">「パウモリ」（Paumori）社</span>、
                    <span class="location-link" data-name="斗里斗里">斗里斗里社</span>、
                    <span class="location-link" data-name="チヨコゾル">「チヨコゾル」（Chiyokozoru）社</span>、
                    <span class="location-link" data-name="近黃">近黃社</span>、
                    <span class="location-link" data-name="ツダカス">「ツダカス」（Tsudakasu）社</span>、
                    <span class="location-link" data-name="トビロウ">「トビロウ」（Tobirou）社</span>、
                    <span class="location-link" data-name="那保那保">那保那保社</span>、
                    <span class="location-link" data-name="カラタラン">「カラタラン」（Karataran）社</span>、
                    <span class="location-link" data-name="マリドツプ">「マリドツプ」（Maridotsupu）社</span>、
                    <span class="location-link" data-name="ポケツ">「ポケツ」（Poketsu）社</span>、
                    <span class="location-link" data-name="カアロワン">「カアロワン」（Kaarowan）社</span>、
                    <span class="location-link" data-name="トロコワン">「トロコワン」（Torokowan）社</span>、
                    <span class="location-link" data-name="パシヨロ">「パシヨロ」（Pashiyoro）社</span>、
                    <span class="location-link" data-name="マリプル">「マリプル」（Maripuru）社</span>、
                    <span class="location-link" data-name="シヨモル">「シヨモル」（Shiyomoru）社</span>、
                    <span class="location-link" data-name="讀古梧">讀古梧社</span>、
                    <span class="location-link" data-name="トコブルイピリ">「トコブルイピリ」（Tokoburuipiri）社</span>、
                    <span class="location-link" data-name="シヤコプ">「シヤコプ」（Shiyakopu）社</span>、
                    <span class="location-link" data-name="トコホル">「トコホル」（10Tokohoru）社</span>、
                    <span class="location-link" data-name="姑子崙">姑子崙社</span>、
                    <span class="location-link" data-name="チヨカクライ">「チヨカクライ」（Chiyakakurai）社</span>、
                    <span class="location-link" data-name="東テババオ">東「テババオ」（Tebabao）社</span>、
                    <span class="location-link" data-name="西テババオ">西「テババオ」（Tebabao）社</span>、
                    <span class="location-link" data-name="テノコ">「テノコ」（Tenoko）社</span>、
                    <span class="location-link" data-name="タバカス">「タバカス」（Tabakasu）社</span>、
                    <span class="location-link" data-name="カクブラン">「カクブラン」（Kakuburan）社</span>、
                    <span class="location-link" data-name="ポリガツト">「ポリガツト」（Porigatsuto）社</span>、
                    <span class="location-link" data-name="タリリク">「タリリク」（Taririku）社</span>、
                    <span class="location-link" data-name="キナバリヤン">「キナバリヤン」(Kinabariyan)社</span>、
                    <span class="location-link" data-name="クダカス">「クダカス」（Kudakasu）社</span>、
                    <span class="location-link" data-name="トアバル">「トアバル」（Toabaru）社</span>、
                    <span class="location-link" data-name="トアカウ">「トアカウ」（Toakau）社</span>、
                    <span class="location-link" data-name="ハイブカイ">「ハイブカイ」（Haibukai）社</span>、
                    <span class="location-link" data-name="ラリバ">「ラリバ」（Rariba）社</span>、
                    <span class="location-link" data-name="トコトコワン">「トコトコワン」（Tokotokowan）社</span>、
                    <span class="location-link" data-name="カケラブチャン">「カケラブチャン」（Kakerabuchiyan）社</span>、
                    <span class="location-link" data-name="大板鹿">大板鹿社</span>、
                    <span class="location-link" data-name="チヨコプリ">「チヨコプリ」（Chokoburi）社</span>、
                    <span class="location-link" data-name="チヤチヤガトワン">「チヤチヤガトワン」（Chiyachiyagatowan）社</span>、
                    <span class="location-link" data-name="阿聖衛">阿聖衛社</span>、
                    <span class="location-link" data-name="出水坡">出水坡社</span>，以上43社，人口5,059人。
                </p>
            </div>
        </div>

        <!-- Section 4: 貳、調查概要與區分成果 -->
        <div class="paper-section step-section" data-step="4">
            <h2 class="section-title">貳、調查概要與區分成果</h2>
            <p>
                本調查區的區分調查班長為伊藤太右衛門技師，調查時間從昭和 5 年 9 月 8 日至 11 月 4 日，調查日數為 58 天，如同前述所記，進行治水調查時，兼施行了區分調查。進行調查時，不只注重在治水國土保安上無窒礙難行之處，亦留意避免造成現居蕃人生活上的威脅，在選定蕃人用保留地上，期望能不留遺憾，且須考量如大武附近的農業適地。區分結果如下表所示。
            </p>
            <p>
                針對蕃人用保留地（準要存置林野），高山蕃 8,586 人，加上平地蕃 261 人，合計有 8,847 人，選定土地面積 2,575 公頃。考量土地的好壞，人均所得土地面積為 2 公頃至 3 公頃。一般而言，雖不以平地蕃「アミ」(Ami)族為對象，制定蕃人用保留地的方針，但長久以來此地的「アミ」(Ami)族與「パイワン」(Paiwan)族混居在一起，與牠們度過同樣的生活，加上平地蕃完全沒有私有地，遂依照臺東廳的期望，讓平地蕃比照高山蕃，先選定保留地。
            </p>
            <p>
                另外，關於準要存置林野面積 25,100 公頃裡的<span class="location-link" data-name="蚶子崙">蕃地蚶子崙 25 公頃</span>，由於溫泉所在地，故認定有必要對之進行公共上的保存與使用，而區分為準要存置林野。
            </p>
        </div>

    </div>
</div>

<!-- 右側地圖容器 -->
<div id="map-container">
    <div id="basemap-control" class="basemap-control">
        <button id="basemap-toggle-btn" class="basemap-btn">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                <polygon points="12 2 2 7 12 12 22 7 12 2"></polygon>
                <polyline points="2 17 12 22 22 17"></polyline>
                <polyline points="2 12 12 17 22 12"></polyline>
            </svg>
            <span>圖層切換</span>
        </button>

        <div id="basemap-menu" class="basemap-menu hidden">
            <div style="font-weight:bold; margin-bottom:8px; font-size:13px; border-bottom:1px solid #eee; padding-bottom:4px;">底圖選擇</div>
            <label style="display:flex; align-items:center; gap:8px; margin-bottom:8px; cursor:pointer; font-size:13px;">
                <input type="radio" name="basemap" value="default" checked>
                <span>預設底圖 (Positron)</span>
            </label>
            <label style="display:flex; align-items:center; gap:8px; margin-bottom:8px; cursor:pointer; font-size:13px;">
                <input type="radio" name="basemap" value="jm50k_1916">
                <span>1916-日治蕃地地形圖</span>
            </label>

            <div id="opacity-control" class="opacity-box" style="display: none;">
                <span style="font-size: 12px; color: #666;">歷史地圖透明度</span>
                <input type="range" id="opacity-slider" min="0.1" max="1" step="0.05" value="0.85">
            </div>
        </div>
    </div>

    <div id="map"></div>
</div>

</div>

<script>
/* =========================================================
   1. 地圖初始化
========================================================= */
const map = new maplibregl.Map({
    container: "map",
    style: "https://basemaps.cartocdn.com/gl/positron-gl-style/style.json",
    center: [120.89, 22.35],
    zoom: 10,
    pitch: 0,
    attributionControl: true
});

const GITHUB_RAW_BASE = "https://raw.githubusercontent.com/willson0505/Dawu/main/";

const DATA_SOURCES = {
    boundary: ["大武調查區界.geojson", "調查區界.geojson"],
    yao: ["大武要存置林野.geojson"],
    zhun: ["大武準要存置林野.geojson"],
    buyao: ["大武不要存置林野.geojson"],
    change: ["大武變動區.geojson"]
};

const loadedGeoData = {};

async function fetchGeoJSON(filenames) {
    for (const fn of filenames) {
        try {
            let res = await fetch(GITHUB_RAW_BASE + encodeURIComponent(fn));
            if (!res.ok) res = await fetch("./" + encodeURIComponent(fn));
            if (res.ok) {
                const data = await res.json();
                console.log("成功載入 GeoJSON:", fn, "物件數:", data.features ? data.features.length : 0);
                return data;
            }
        } catch (e) {
            console.warn("無法讀取檔案:", fn);
        }
    }
    return null;
}

/* =========================================================
   2. 地圖資源與圖層載入
========================================================= */
map.on("load", async () => {

    // 3D DEM
    map.addSource("taiwan-dem", {
        type: "raster-dem",
        tiles: ["https://s3.amazonaws.com/elevation-tiles-prod/terrarium/{z}/{x}/{y}.png"],
        tileSize: 256,
        encoding: "terrarium",
        maxzoom: 15
    });
    map.setTerrain({ source: "taiwan-dem", exaggeration: 1.2 });

    // 1916 地形圖
    map.addSource("jm50k-1916-src", {
        type: "raster",
        tiles: ["https://gis.sinica.edu.tw/tileserver/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=JM50K_1916&STYLE=_null&TILEMATRIXSET=GoogleMapsCompatible&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&FORMAT=image/jpeg"],
        tileSize: 256
    });

    // 讀取 GeoJSON 數據
    loadedGeoData.boundary = await fetchGeoJSON(DATA_SOURCES.boundary);
    loadedGeoData.yao = await fetchGeoJSON(DATA_SOURCES.yao);
    loadedGeoData.zhun = await fetchGeoJSON(DATA_SOURCES.zhun);
    loadedGeoData.buyao = await fetchGeoJSON(DATA_SOURCES.buyao);
    loadedGeoData.change = await fetchGeoJSON(DATA_SOURCES.change);

    // 區界
    if (loadedGeoData.boundary) {
        map.addSource("boundary-src", { type: "geojson", data: loadedGeoData.boundary });
        map.addLayer({
            id: "boundary-fill",
            type: "fill",
            source: "boundary-src",
            paint: { "fill-color": "#5a6b7c", "fill-opacity": 0.05 }
        });
        map.addLayer({
            id: "boundary-line",
            type: "line",
            source: "boundary-src",
            paint: { "line-color": "#2c3e50", "line-width": 2, "line-dasharray": [4, 2] }
        });
    }

    // 1916 歷史底圖
    map.addLayer({
        id: "jm50k-1916-layer",
        type: "raster",
        source: "jm50k-1916-src",
        layout: { visibility: "none" },
        paint: { "raster-opacity": 0.85 }
    });

    // 要存置林野 (綠)
    if (loadedGeoData.yao) {
        map.addSource("yao-src", { type: "geojson", data: loadedGeoData.yao });
        map.addLayer({
            id: "yao-layer",
            type: "fill",
            source: "yao-src",
            paint: { "fill-color": "#2e7d32", "fill-opacity": 0.6, "fill-outline-color": "#1b5e20" }
        });
    }

    // 準要存置林野 (橘)
    if (loadedGeoData.zhun) {
        map.addSource("zhun-src", { type: "geojson", data: loadedGeoData.zhun });
        map.addLayer({
            id: "zhun-layer",
            type: "fill",
            source: "zhun-src",
            paint: { "fill-color": "#ef6c00", "fill-opacity": 0.7, "fill-outline-color": "#e65100" }
        });
        map.addLayer({
            id: "zhun-label",
            type: "symbol",
            source: "zhun-src",
            layout: {
                "text-field": ["coalesce", ["get", "地名"], ["get", "番號"], ["get", "Name"], ""],
                "text-size": 12,
                "text-allow-overlap": false
            },
            paint: { "text-color": "#ffffff", "text-halo-color": "#000000", "text-halo-width": 2 }
        });
    }

    // 不要存置林野 (紅)
    if (loadedGeoData.buyao) {
        map.addSource("buyao-src", { type: "geojson", data: loadedGeoData.buyao });
        map.addLayer({
            id: "buyao-layer",
            type: "fill",
            source: "buyao-src",
            paint: { "fill-color": "#c62828", "fill-opacity": 0.65, "fill-outline-color": "#8e0000" }
        });
    }

    // Hover 高亮發光圖層
    map.addSource("highlight-src", {
        type: "geojson",
        data: { type: "FeatureCollection", features: [] }
    });
    map.addLayer({
        id: "highlight-fill-layer",
        type: "fill",
        source: "highlight-src",
        paint: { "fill-color": "#ffea00", "fill-opacity": 0.75 }
    });
    map.addLayer({
        id: "highlight-line-layer",
        type: "line",
        source: "highlight-src",
        paint: { "line-color": "#d9381e", "line-width": 4 }
    });

    // 啟用懸停連線事件
    initLocationHoverEvents();
});

/* =========================================================
   3. 地名檢索與多邊形中心點計算
========================================================= */
function getFeatureCentroid(feature) {
    if (!feature || !feature.geometry) return null;
    const geom = feature.geometry;
    let coords = [];

    if (geom.type === 'Point') return { center: geom.coordinates };
    if (geom.type === 'Polygon') coords = geom.coordinates[0];
    else if (geom.type === 'MultiPolygon') coords = geom.coordinates.flatMap(p => p[0]);
    else if (geom.type === 'LineString') coords = geom.coordinates;

    if (!coords || coords.length === 0) return null;

    let minX = Infinity, minY = Infinity, maxX = -Infinity, maxY = -Infinity;
    let sumX = 0, sumY = 0;
    
    coords.forEach(c => {
        sumX += c[0];
        sumY += c[1];
        if (c[0] < minX) minX = c[0];
        if (c[1] < minY) minY = c[1];
        if (c[0] > maxX) maxX = c[0];
        if (c[1] > maxY) maxY = c[1];
    });

    return {
        center: [sumX / coords.length, sumY / coords.length],
        bounds: [[minX, minY], [maxX, maxY]]
    };
}

/* 優先抓取 JSON 中的 {地名} 屬性欄位 */
function findFeatureByName(rawName) {
    if (!rawName) return null;
    
    // 清理文字雜質（如社、括號、引號）
    const cleanSearchName = rawName
        .replace(/[「」『』"]/g, '')
        .replace(/（[^）]+）/g, '')
        .replace(/\([^\)]+\)/g, '')
        .replace(/社$/g, '')
        .trim();

    for (const key of ['zhun', 'yao', 'buyao', 'boundary', 'change']) {
        const geojson = loadedGeoData[key];
        if (!geojson || !geojson.features) continue;

        for (const feat of geojson.features) {
            const props = feat.properties || {};
            
            // 優先比對 {地名} 欄位
            const diming = props["地名"] || props["diming"] || props["DIMING"] || props["Name"] || props["NAME"] || props["番號"];
            
            if (diming) {
                const targetStr = String(diming)
                    .replace(/[「」『』"]/g, '')
                    .replace(/社$/g, '')
                    .trim();
                
                if (
                    targetStr === cleanSearchName ||
                    targetStr.includes(cleanSearchName) ||
                    cleanSearchName.includes(targetStr)
                ) {
                    return feat;
                }
            }

            // 次要備援：比對所有屬性值
            for (const val of Object.values(props)) {
                if (val && typeof val === 'string') {
                    const strVal = val.replace(/[「」『』"]/g, '').replace(/社$/g, '').trim();
                    if (strVal && (strVal === cleanSearchName || strVal.includes(cleanSearchName) || cleanSearchName.includes(strVal))) {
                        return feat;
                    }
                }
            }
        }
    }
    return null;
}

/* =========================================================
   4. 動態 SVG 拉線與高亮
========================================================= */
let activeHoverState = null;

function updateConnectionLine() {
    if (!activeHoverState) return;

    const { element, centroidInfo } = activeHoverState;
    if (!centroidInfo || !centroidInfo.center) return;

    const mapContainer = document.getElementById("map-container");
    const mapRect = mapContainer.getBoundingClientRect();

    // MapLibre project 計算地圖座標點於螢幕之像素位置
    const mapPixel = map.project(centroidInfo.center);
    const targetX = mapRect.left + mapPixel.x;
    const targetY = mapRect.top + mapPixel.y;

    // 左側文字標籤右邊界
    const elemRect = element.getBoundingClientRect();
    const sourceX = elemRect.right;
    const sourceY = elemRect.top + elemRect.height / 2;

    // 計算貝茲曲線
    const dx = targetX - sourceX;
    const controlX1 = sourceX + Math.max(dx * 0.4, 30);
    const controlX2 = targetX - Math.max(dx * 0.4, 30);
    
    const pathD = `M ${sourceX} ${sourceY} C ${controlX1} ${sourceY}, ${controlX2} ${targetY}, ${targetX} ${targetY}`;

    const line = document.getElementById("connection-line");
    const startDot = document.getElementById("start-dot");
    const endDot = document.getElementById("end-dot");

    line.setAttribute("d", pathD);
    line.style.display = "block";

    startDot.setAttribute("cx", sourceX);
    startDot.setAttribute("cy", sourceY);
    startDot.style.display = "block";

    endDot.setAttribute("cx", targetX);
    endDot.setAttribute("cy", targetY);
    endDot.style.display = "block";
}

function initLocationHoverEvents() {
    const links = document.querySelectorAll(".location-link");

    links.forEach(link => {
        link.addEventListener("mouseenter", () => {
            const name = link.getAttribute("data-name");
            const feat = findFeatureByName(name);

            if (feat) {
                const centroidInfo = getFeatureCentroid(feat);
                if (centroidInfo) {
                    activeHoverState = { element: link, feature: feat, centroidInfo: centroidInfo };
                    link.classList.add("active-link");

                    // 1. 在地圖高亮該多邊形
                    map.getSource("highlight-src").setData({
                        type: "FeatureCollection",
                        features: [feat]
                    });

                    // 2. 地圖平移聚焦
                    map.easeTo({
                        center: centroidInfo.center,
                        zoom: Math.max(map.getZoom(), 11.5),
                        duration: 800
                    });

                    // 3. 畫線上點
                    updateConnectionLine();
                }
            } else {
                console.warn("未能在 GeoJSON 中匹配到地名:", name);
            }
        });

        link.addEventListener("mouseleave", () => {
            link.classList.remove("active-link");
            activeHoverState = null;

            // 隱藏線條與高亮
            document.getElementById("connection-line").style.display = "none";
            document.getElementById("start-dot").style.display = "none";
            document.getElementById("end-dot").style.display = "none";

            map.getSource("highlight-src").setData({
                type: "FeatureCollection",
                features: []
            });
        });
    });

    map.on("move", updateConnectionLine);
    map.on("render", updateConnectionLine);
    window.addEventListener("scroll", updateConnectionLine);
}

/* =========================================================
   5. Scrollama 捲動觸發視角
========================================================= */
const stepActions = {
    "1": () => { map.easeTo({ center: [120.89, 22.35], zoom: 10, pitch: 0, duration: 1500 }); },
    "2": () => { map.easeTo({ center: [120.91, 22.42], zoom: 10.8, pitch: 30, duration: 1500 }); },
    "3": () => { map.easeTo({ center: [120.86, 22.37], zoom: 11.2, pitch: 35, duration: 1500 }); },
    "4": () => { map.easeTo({ center: [120.89, 22.35], zoom: 10.2, pitch: 15, duration: 1500 }); }
};

const scroller = scrollama();
scroller.setup({ step: ".step-section", offset: 0.5 }).onStepEnter((res) => {
    document.querySelectorAll(".step-section").forEach(e => e.classList.remove("is-active"));
    res.element.classList.add("is-active");
    const step = res.element.getAttribute("data-step");
    if (stepActions[step]) stepActions[step]();
});

/* =========================================================
   6. 底圖控制項選單
========================================================= */
const basemapBtn = document.getElementById("basemap-toggle-btn");
const basemapMenu = document.getElementById("basemap-menu");
const opacityControl = document.getElementById("opacity-control");
const opacitySlider = document.getElementById("opacity-slider");

basemapBtn.addEventListener("click", (e) => {
    e.stopPropagation();
    basemapMenu.classList.toggle("hidden");
});

document.addEventListener("click", (e) => {
    if (!document.getElementById("basemap-control").contains(e.target)) {
        basemapMenu.classList.add("hidden");
    }
});

document.querySelectorAll("input[name='basemap']").forEach(radio => {
    radio.addEventListener("change", (e) => {
        const isHist = e.target.value === "jm50k_1916";
        map.setLayoutProperty("jm50k-1916-layer", "visibility", isHist ? "visible" : "none");
        opacityControl.style.display = isHist ? "flex" : "none";
    });
});

opacitySlider.addEventListener("input", (e) => {
    map.setPaintProperty("jm50k-1916-layer", "raster-opacity", parseFloat(e.target.value));
});

window.addEventListener("resize", () => {
    scroller.resize();
    map.resize();
});
</script>

</body>
</html>
