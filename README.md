<!DOCTYPE html>
<html lang="zh-TW">

<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>《日治時期森林計畫事業區分調查史料》大武調查區概覽</title>

<!-- MapLibre GL -->
<link href="https://unpkg.com/maplibre-gl@3.6.2/dist/maplibre-gl.css" rel="stylesheet" />
<script src="https://unpkg.com/maplibre-gl@3.6.2/dist/maplibre-gl.js"></script>

<!-- Scrollama -->
<script src="https://unpkg.com/scrollama"></script>

<style>
/* =========================================================
   基本設定
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
    line-height: 2;
    background: #fcfaf7;
    overflow-x: hidden;
}

/* =========================================================
   左文右圖版面
========================================================= */
#container {
    display: flex;
    flex-direction: row;
    min-height: 100vh;
    position: relative;
}

#story {
    width: 45vw;
    padding: 6vh 4vw;
    z-index: 2;
}

#map-container {
    width: 55vw;
    height: 100vh;
    position: sticky;
    top: 0;
    background: #e5e3df;
}

#map {
    width: 100%;
    height: 100%;
}

/* =========================================================
   SVG 全螢幕連線畫布
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
   標題與排版
========================================================= */
h1.main-title {
    font-size: 1.8rem;
    line-height: 1.5;
    font-weight: bold;
    margin-bottom: 8px;
    color: #1a2a1d;
    border-bottom: 2px solid #1a2a1d;
    padding-bottom: 12px;
}

.subtitle {
    font-size: 1rem;
    color: #666;
    margin-bottom: 30px;
    font-weight: 500;
}

.paper-body {
    width: 100%;
}

p {
    font-size: 1.02rem;
    text-align: justify;
    margin-bottom: 1.5em;
    text-indent: 2em;
}

h2.section-title {
    font-size: 1.3rem;
    color: #1a2a1d;
    margin: 1.8em 0 0.8em 0;
    border-left: 4px solid #2b580c;
    padding-left: 10px;
}

/* =========================================================
   Scrollama Step 樣式
========================================================= */
.step-section {
    margin-bottom: 75vh;
    padding: 28px;
    background: rgba(255, 255, 255, 0.96);
    border-radius: 8px;
    box-shadow: 0 4px 18px rgba(0,0,0,0.05);
    border-left: 5px solid #d1c7bd;
    transition: border-color 0.3s ease;
}

.step-section.is-active {
    border-left-color: #2b580c;
}

/* =========================================================
   地名互動連結標籤 (Hover 特效)
========================================================= */
.location-link {
    color: #b83227;
    font-weight: bold;
    border-bottom: 2px dashed #b83227;
    background: rgba(184, 50, 39, 0.08);
    padding: 1px 6px;
    margin: 0 2px;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.2s ease;
    display: inline-block;
    text-indent: 0;
}

.location-link:hover, .location-link.active-link {
    background: #b83227;
    color: #ffffff;
    border-bottom-color: transparent;
    box-shadow: 0 2px 8px rgba(184, 50, 39, 0.35);
    transform: translateY(-1px);
}

/* =========================================================
   右上角底圖控制項
========================================================= */
.basemap-control {
    position: absolute;
    top: 16px;
    right: 16px;
    z-index: 10;
    font-size: 14px;
}

.basemap-btn {
    display: flex;
    align-items: center;
    gap: 6px;
    background: rgba(255, 255, 255, 0.95);
    color: #2b2b2b;
    border: 1px solid rgba(0, 0, 0, 0.18);
    padding: 8px 14px;
    border-radius: 6px;
    cursor: pointer;
    font-weight: 600;
    box-shadow: 0 2px 8px rgba(0,0,0,0.12);
    transition: all 0.2s ease;
}

.basemap-btn:hover {
    background: #ffffff;
    box-shadow: 0 4px 12px rgba(0,0,0,0.18);
}

.basemap-menu {
    position: absolute;
    top: 42px;
    right: 0;
    width: 240px;
    background: rgba(255, 255, 255, 0.96);
    backdrop-filter: blur(4px);
    border: 1px solid rgba(0, 0, 0, 0.15);
    border-radius: 8px;
    padding: 12px 14px;
    box-shadow: 0 4px 16px rgba(0,0,0,0.15);
}

.basemap-menu.hidden {
    display: none;
}

.basemap-title {
    font-weight: bold;
    color: #1a2a1d;
    margin-bottom: 8px;
    font-size: 13px;
    border-bottom: 1px solid #e0e0e0;
    padding-bottom: 4px;
}

.basemap-option {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 8px;
    cursor: pointer;
    font-size: 13px;
}

.opacity-box {
    margin-top: 10px;
    padding-top: 8px;
    border-top: 1px dashed #cccccc;
    display: flex;
    flex-direction: column;
    gap: 4px;
}

/* Responsive */
@media (max-width: 768px) {
    #container { flex-direction: column-reverse; }
    #story { width: 100vw; padding: 20px; }
    #map-container { width: 100vw; height: 45vh; position: sticky; top: 0; }
}
</style>
</head>

<body>

<!-- SVG 全螢幕動態連線畫布 -->
<svg id="svg-overlay">
    <defs>
        <filter id="glow" x="-20%" y="-20%" width="140%" height="140%">
            <feGaussianBlur stdDeviation="3" result="blur" />
            <feComposite in="SourceGraphic" in2="blur" operator="over" />
        </filter>
    </defs>
    <path id="connection-line" fill="none" stroke="#d9381e" stroke-width="3" stroke-dasharray="6,4" filter="url(#glow)" style="display:none;" />
    <circle id="start-dot" r="5" fill="#d9381e" style="display:none;" />
    <circle id="end-dot" r="7" fill="#d9381e" stroke="#ffffff" stroke-width="2" style="display:none;" />
</svg>

<div id="container">

<!-- 左側故事文案 -->
<div id="story">
    <h1 class="main-title">《日治時期森林計畫事業區分調查史料》解讀──大武調查區概覽</h1>
    <div class="subtitle">區域範圍：臺東廳大武支廳（83,612公頃）</div>

    <div class="paper-body">

        <!-- [Step 1] 區域概覽與邊界 -->
        <div class="step-section" data-step="1">
            <p>
                本調查區包含臺東廳大武支廳內的國有林野，面積為 83,612 公頃。此區域原為第二次森林計畫地區的預定地，但已趁森林治水調查之便，實施區分調查。依此調查，規劃了蕃人生活用的區域，且此調查具備保護林野的取締效果；另一方面，亦釐清了第二次森林計畫裡的營林用要存置林野面積。由此可見此次調查的必要性。
            </p>
            <p>
                關於調查區的邊界：西方及南方通過<span class="location-link" data-name="大武山">大武山</span>（海拔10,665日尺；3,232公尺），以脊梁山脈的分水嶺—<span class="location-link" data-name="牡丹溪山">牡丹溪山</span>（海拔1,606日尺；487公尺）為界，與高雄州相鄰；北方依<span class="location-link" data-name="知本溪">知本溪</span>，與臺東支廳相接；南方則至海岸。
            </p>
        </div>

        <!-- [Step 2] 地勢與水系 -->
        <div class="step-section" data-step="2">
            <h2 class="section-title">壹、土地的狀況</h2>
            <p>
                關於林野的狀態、地勢，西方有高峰相連的脊梁山脈分水嶺。山脈的地勢向東漸低，山勢陡峭，其中有<span class="location-link" data-name="大麻溪">大麻溪</span>、<span class="location-link" data-name="蚶子崙溪">蚶子崙溪</span>、<span class="location-link" data-name="大竹高溪">大竹高溪</span>、<span class="location-link" data-name="大武溪">大武溪</span>等諸溪流過，溪邊的山坡地遭溪流嚴重侵蝕；綜觀調查區各地的地勢，調查區南方從北邊起為緩坡，各山峰的山勢由北至南越緩。地質以黏板岩及砂岩為基岩，土壤質地為砂質壤土，地力一般。
            </p>
            <p>
                林況方面，原生林約佔全林地面積 7 成，其餘大部分的土地為蕃人開墾地，其中一半以上為叢林。9 成的原生林為闊葉樹的雜木林，但到了<span class="location-link" data-name="大武山">大武山</span>附近的高地，有由自然生長的臺灣鐵杉、紅檜而成的針闊葉混合林。隨著遠離海岸，樹木生長情形逐漸變好。
            </p>
        </div>

        <!-- [Step 3] 聚落與蕃社 -->
        <div class="step-section" data-step="3">
            <p>
                關於調查區內居民的狀況，此區域原為「パイワン」（Paiwan）族太麻里蕃的根據地，現居此地的蕃人，在行政區有 17 個蕃社、人口有 3,527 人；蕃地上有 43 社、人口有 5,059 人，合計 8,586 人。此外，尚有平地蕃「アミ」（Ami）族 261 人與 Paiwan 族混居。
            </p>
            <p>
                <strong>行政區蕃社（17社）：</strong><br>
                <span class="location-link" data-name="太麻里">太麻里</span>、<span class="location-link" data-name="羅打結">羅打結</span>、<span class="location-link" data-name="鴨子蘭">鴨子蘭</span>、<span class="location-link" data-name="文里格">文里格</span>、<span class="location-link" data-name="猴子蘭">猴子蘭</span>、<span class="location-link" data-name="蚶子崙">蚶子崙</span>、<span class="location-link" data-name="大武窟">大武窟</span>、<span class="location-link" data-name="察暗密">察暗密</span>、<span class="location-link" data-name="打暗打蘭">打暗打蘭</span>、<span class="location-link" data-name="大武">大武</span>、<span class="location-link" data-name="鴿子籠">鴿子籠</span>、<span class="location-link" data-name="大烏萬">大烏萬</span>、<span class="location-link" data-name="拔子洞">拔子洞</span>、<span class="location-link" data-name="獅子獅">獅子獅</span>、<span class="location-link" data-name="大竹高">大竹高</span>、<span class="location-link" data-name="甘壁壁">甘壁壁</span>、<span class="location-link" data-name="大得吉">大得吉</span>。
            </p>
            <p>
                <strong>蕃地蕃社（43社，範例重點社）：</strong><br>
                <span class="location-link" data-name="近黃社">近黃社</span>、<span class="location-link" data-name="ルラクシ">ルラクシ社</span>、<span class="location-link" data-name="ビララウ">ビララウ社</span>、<span class="location-link" data-name="パウモリ">パウモリ社</span>、<span class="location-link" data-name="斗里斗里">斗里斗里社</span>、<span class="location-link" data-name="チヨコゾル">チヨコゾル社</span>、<span class="location-link" data-name="カラタラン">カラタラン社</span>、<span class="location-link" data-name="姑子崙">姑子崙社</span>、<span class="location-link" data-name="出水坡">出水坡社</span>、<span class="location-link" data-name="阿聖衛">阿聖衛社</span>、<span class="location-link" data-name="大板鹿">大板鹿社</span> 等 43 社。
            </p>
        </div>

        <!-- [Step 4] 調查成果與區分劃設 -->
        <div class="step-section" data-step="4">
            <h2 class="section-title">貳、調查概要與區分成果</h2>
            <p>
                本調查區的區分調查班長為伊藤太右衛門技師，調查時間從昭和 5 年 9 月 8 日至 11 月 4 日，調查日數為 58 天。進行調查時，不只注重在治水國土保安上無窒礙難行之處，亦留意避免造成現居蕃人生活上的威脅，在選定蕃人用保留地上，期望能不留遺遺憾。
            </p>
            <p>
                針對蕃人用保留地（準要存置林野），高山蕃 8,586 人，加上平地蕃 261 人，選定土地面積 2,575 公頃，人均所得土地面積約 2 至 3 公頃。另外，關於準要存置林野面積 25,100 公頃裡的<span class="location-link" data-name="蚶子崙">蕃地蚶子崙 25 公頃</span>，因屬溫泉所在地，認定有公共保存與使用之必要，特別劃歸為準要存置林野。
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
            <div class="basemap-title">底圖選擇</div>
            <label class="basemap-option">
                <input type="radio" name="basemap" value="default" checked>
                <span>預設底圖 (Positron)</span>
            </label>
            <label class="basemap-option">
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
    zoom: 9.8,
    pitch: 0,
    attributionControl: true
});

/* GitHub Raw 資料庫位址與備援路徑 */
const GITHUB_RAW_BASE = "https://raw.githubusercontent.com/willson0505/Dawu/main/";

const DATA_SOURCES = {
    boundary: ["大武調查區界.geojson", "調查區界.geojson"],
    yao: ["大武要存置林野.geojson"],
    zhun: ["大武準要存置林野.geojson"],
    buyao: ["大武不要存置林野.geojson"],
    change: ["大武變動區.geojson"]
};

// 全域儲存所有載入的 GeoJSON 數據以供 Hover 檢索中心點
const loadedGeoData = {};

async function fetchGeoJSON(filenames) {
    for (const fn of filenames) {
        try {
            // 優先嘗試 GitHub Raw，失敗則讀取相對路徑
            let res = await fetch(GITHUB_RAW_BASE + encodeURIComponent(fn));
            if (!res.ok) res = await fetch("./" + encodeURIComponent(fn));
            if (res.ok) {
                console.log("成功載入 GeoJSON:", fn);
                return await res.json();
            }
        } catch (e) {
            console.warn("無法取得檔案:", fn);
        }
    }
    return null;
}

/* =========================================================
   2. 地圖資源載入
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

    // 1916 蕃地地形圖 WMTS
    map.addSource("jm50k-1916-src", {
        type: "raster",
        tiles: ["https://gis.sinica.edu.tw/tileserver/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=JM50K_1916&STYLE=_null&TILEMATRIXSET=GoogleMapsCompatible&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&FORMAT=image/jpeg"],
        tileSize: 256,
        maxzoom: 16
    });

    // 載入 GeoJSON 數據
    loadedGeoData.boundary = await fetchGeoJSON(DATA_SOURCES.boundary);
    loadedGeoData.yao = await fetchGeoJSON(DATA_SOURCES.yao);
    loadedGeoData.zhun = await fetchGeoJSON(DATA_SOURCES.zhun);
    loadedGeoData.buyao = await fetchGeoJSON(DATA_SOURCES.buyao);

    // 1. 調查區界
    if (loadedGeoData.boundary) {
        map.addSource("boundary-src", { type: "geojson", data: loadedGeoData.boundary });
        map.addLayer({
            id: "boundary-fill",
            type: "fill",
            source: "boundary-src",
            paint: { "fill-color": "#5a6b7c", "fill-opacity": 0.08 }
        });
        map.addLayer({
            id: "boundary-line",
            type: "line",
            source: "boundary-src",
            paint: { "line-color": "#2c3e50", "line-width": 1.5 }
        });
    }

    // 2. 1916 地形圖層 (預設隱藏)
    map.addLayer({
        id: "jm50k-1916-layer",
        type: "raster",
        source: "jm50k-1916-src",
        layout: { visibility: "none" },
        paint: { "raster-opacity": 0.85 }
    });

    // 3. 要存置林野 (綠色)
    if (loadedGeoData.yao) {
        map.addSource("yao-src", { type: "geojson", data: loadedGeoData.yao });
        map.addLayer({
            id: "yao-layer",
            type: "fill",
            source: "yao-src",
            paint: { "fill-color": "#2e7d32", "fill-opacity": 0.65, "fill-outline-color": "#1b5e20" }
        });
    }

    // 4. 準要存置林野 (橘色)
    if (loadedGeoData.zhun) {
        map.addSource("zhun-src", { type: "geojson", data: loadedGeoData.zhun });
        map.addLayer({
            id: "zhun-layer",
            type: "fill",
            source: "zhun-src",
            paint: { "fill-color": "#ef6c00", "fill-opacity": 0.75, "fill-outline-color": "#e65100" }
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

    // 5. 不要存置林野 (紅色)
    if (loadedGeoData.buyao) {
        map.addSource("buyao-src", { type: "geojson", data: loadedGeoData.buyao });
        map.addLayer({
            id: "buyao-layer",
            type: "fill",
            source: "buyao-src",
            paint: { "fill-color": "#c62828", "fill-opacity": 0.7, "fill-outline-color": "#8e0000" }
        });
    }

    // 6. Hover 動態高亮外框圖層
    map.addSource("highlight-src", {
        type: "geojson",
        data: { type: "FeatureCollection", features: [] }
    });
    map.addLayer({
        id: "highlight-layer",
        type: "line",
        source: "highlight-src",
        paint: { "line-color": "#d9381e", "line-width": 4, "line-blur": 1 }
    });
    map.addLayer({
        id: "highlight-fill-layer",
        type: "fill",
        source: "highlight-src",
        paint: { "fill-color": "#ffeb3b", "fill-opacity": 0.45 }
    });

    // 綁定左側地名 Hover 事件
    initLocationHoverEvents();
});

/* =========================================================
   3. 多邊形中心點計算 (Centroid Calculation)
========================================================= */
function getFeatureCentroid(feature) {
    if (!feature || !feature.geometry) return null;
    let coords = [];
    const geom = feature.geometry;

    if (geom.type === 'Point') return geom.coordinates;
    if (geom.type === 'Polygon') coords = geom.coordinates[0];
    else if (geom.type === 'MultiPolygon') coords = geom.coordinates.flatMap(p => p[0]);
    else if (geom.type === 'LineString') coords = geom.coordinates;

    if (!coords || coords.length === 0) return null;
    let sumLng = 0, sumLat = 0;
    coords.forEach(c => { sumLng += c[0]; sumLat += c[1]; });
    return [sumLng / coords.length, sumLat / coords.length];
}

/* 檢索與地名匹配的 GeoJSON Feature */
function findFeatureByName(name) {
    for (const key of ['zhun', 'yao', 'buyao', 'boundary']) {
        const geojson = loadedGeoData[key];
        if (!geojson || !geojson.features) continue;

        for (const feat of geojson.features) {
            const props = feat.properties || {};
            for (const val of Object.values(props)) {
                if (typeof val === 'string' && (val.includes(name) || name.includes(val))) {
                    return feat;
                }
            }
        }
    }
    return null;
}

/* =========================================================
   4. 動態拉線 (SVG Linking Line) 邏輯
========================================================= */
let activeHoverState = null;

function updateConnectionLine() {
    if (!activeHoverState) return;

    const { element, feature, centroid } = activeHoverState;
    const mapContainer = document.getElementById("map-container");
    const mapRect = mapContainer.getBoundingClientRect();

    // 轉換地圖中心座標為螢幕像素 [x, y]
    const mapPixel = map.project(centroid);
    const targetX = mapRect.left + mapPixel.x;
    const targetY = mapRect.top + mapPixel.y;

    // 計算左側 HTML 標籤右邊界位置
    const elemRect = element.getBoundingClientRect();
    const sourceX = elemRect.right;
    const sourceY = elemRect.top + elemRect.height / 2;

    // 繪製貝茲曲線 (Bezier Curve)
    const controlX = (sourceX + targetX) / 2;
    const pathD = `M ${sourceX} ${sourceY} C ${controlX} ${sourceY}, ${controlX} ${targetY}, ${targetX} ${targetY}`;

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
        link.addEventListener("mouseenter", (e) => {
            const name = link.getAttribute("data-name");
            const feat = findFeatureByName(name);

            if (feat) {
                const centroid = getFeatureCentroid(feat);
                if (centroid) {
                    activeHoverState = { element: link, feature: feat, centroid: centroid };
                    link.classList.add("active-link");

                    // 高亮地圖上的 Polygon
                    map.getSource("highlight-src").setData({
                        type: "FeatureCollection",
                        features: [feat]
                    });

                    updateConnectionLine();
                }
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

    // 當地圖移動、縮放時即時更新連線位置
    map.on("move", updateConnectionLine);
    map.on("render", updateConnectionLine);
    window.addEventListener("scroll", updateConnectionLine);
}

/* =========================================================
   5. Scrollama 視角切換
========================================================= */
const stepActions = {
    "1": () => {
        map.easeTo({ center: [120.89, 22.35], zoom: 9.8, pitch: 0, duration: 1800 });
    },
    "2": () => {
        map.easeTo({ center: [120.91, 22.42], zoom: 11.2, pitch: 35, duration: 1800 });
    },
    "3": () => {
        map.easeTo({ center: [120.85, 22.38], zoom: 11.8, pitch: 45, duration: 2000 });
    },
    "4": () => {
        map.easeTo({ center: [120.89, 22.35], zoom: 10.5, pitch: 20, duration: 1800 });
    }
};

const scroller = scrollama();
scroller.setup({ step: ".step-section", offset: 0.5 }).onStepEnter((res) => {
    document.querySelectorAll(".step-section").forEach(e => e.classList.remove("is-active"));
    res.element.classList.add("is-active");
    const step = res.element.getAttribute("data-step");
    if (stepActions[step]) stepActions[step]();
});

/* =========================================================
   6. UI 底圖切換與拉桿控制
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
