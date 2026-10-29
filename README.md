[Uploading index.html…]()
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>山东省青少年创客大赛 · 考场查询</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", "Microsoft YaHei", sans-serif;
    background: #f5f7fa;
    color: #1f2d3d;
    min-height: 100vh;
    padding: 24px 16px 48px;
  }
  .container { max-width: 560px; margin: 0 auto; }
  h1 {
    font-size: 22px;
    text-align: center;
    margin-bottom: 6px;
    color: #1a3a6b;
  }
  .subtitle {
    text-align: center;
    font-size: 13px;
    color: #7a8ba6;
    margin-bottom: 24px;
  }
  .search-box {
    background: #fff;
    border-radius: 14px;
    padding: 18px 16px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.06);
    margin-bottom: 20px;
  }
  .search-box label {
    display: block;
    font-size: 14px;
    color: #4a5a70;
    margin-bottom: 8px;
    font-weight: 600;
  }
  .search-box input {
    width: 100%;
    height: 48px;
    border: 2px solid #d9e2ef;
    border-radius: 10px;
    padding: 0 14px;
    font-size: 17px;
    outline: none;
    transition: border-color .2s;
  }
  .search-box input:focus { border-color: #3b7ddd; }
  .hint {
    font-size: 12px;
    color: #9aa8bc;
    margin-top: 8px;
  }
  .result-card {
    background: #fff;
    border-radius: 14px;
    padding: 18px 16px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.06);
    margin-bottom: 14px;
    border-left: 5px solid #3b7ddd;
  }
  .result-card .name-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 14px;
    padding-bottom: 12px;
    border-bottom: 1px dashed #e3eaf3;
  }
  .result-card .name {
    font-size: 20px;
    font-weight: 700;
    color: #1a3a6b;
  }
  .result-card .group-tag {
    font-size: 12px;
    background: #eaf1fb;
    color: #3b7ddd;
    padding: 4px 10px;
    border-radius: 20px;
  }
  .info-row {
    display: flex;
    margin-bottom: 10px;
    font-size: 15px;
    line-height: 1.5;
  }
  .info-row:last-child { margin-bottom: 0; }
  .info-label {
    flex: 0 0 78px;
    color: #7a8ba6;
    font-size: 14px;
  }
  .info-value {
    flex: 1;
    color: #1f2d3d;
    font-weight: 500;
    word-break: break-all;
  }
  .info-value.highlight {
    color: #d4380d;
    font-weight: 700;
  }
  .empty, .no-result {
    text-align: center;
    padding: 32px 16px;
    color: #9aa8bc;
    font-size: 15px;
    background: #fff;
    border-radius: 14px;
  }
  .no-result .big { font-size: 40px; margin-bottom: 10px; }
  .count-tip {
    font-size: 13px;
    color: #7a8ba6;
    margin-bottom: 12px;
    text-align: center;
  }
</style>
</head>
<body>
<div class="container">
  <h1>山东省青少年创客大赛</h1>
  <p class="subtitle">输入学生姓名，查询考场信息</p>

  <div class="search-box">
    <label for="nameInput">学生姓名</label>
    <input id="nameInput" type="text" placeholder="请输入姓名" autocomplete="off">
    <p class="hint">支持输入完整姓名，输入后自动查询</p>
  </div>

  <div id="resultArea">
    <div class="empty">请输入姓名开始查询</div>
  </div>
</div>

<script>
const VENUE_BASE = "济南市中区未成年人素质教育基地（济南市市中区望岳路5677号）";
const DATE_PREFIX = "11月1号 ";

const DATA = [
  {赛项:"无人机编程场地赛（实操飞行）",姓名:"赵俊芃",参赛编号:"B-WRJBC-105",比赛场馆:"3号场馆4楼无人机编程场地赛1",时间:"13:00-18:30",组别:"初中组",学校:"济南市历城第五中学",年级:"7年级"},
  {赛项:"无人机编程场地赛（实操飞行）",姓名:"董翰誉",参赛编号:"B-WRJBC-211",比赛场馆:"3号场馆4楼无人机编程场地赛2",时间:"13:00-18:30",组别:"小学组全年级",学校:"济南市历下区历山学校",年级:"4年级"},
  {赛项:"无人机编程场地赛（实操飞行）",姓名:"张天睿",参赛编号:"B-WRJBC-212",比赛场馆:"3号场馆4楼无人机编程场地赛2",时间:"13:00-18:30",组别:"小学组全年级",学校:"济南市历城区幼安小学",年级:"3年级"},
  {赛项:"无人机编程场地赛（实操飞行）",姓名:"胥俊泽",参赛编号:"B-WRJBC-213",比赛场馆:"3号场馆4楼无人机编程场地赛2",时间:"13:00-18:30",组别:"小学组全年级",学校:"济南市历城区第六中学",年级:"3年级"},
  {赛项:"无人机编程场地赛（实操飞行）",姓名:"刘金睿",参赛编号:"B-WRJBC-214",比赛场馆:"3号场馆4楼无人机编程场地赛2",时间:"13:00-18:30",组别:"小学组全年级",学校:"济南市历下区熙华小学",年级:"6年级"},
  {赛项:"算法编程（scratch）",姓名:"李彦宏",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"13:00-14:00",组别:"小学组全年级",学校:"济南市历城区工业北路第二小学",年级:"3年级"},
  {赛项:"算法编程（scratch）",姓名:"赵英皓",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"13:00-14:00",组别:"小学组全年级",学校:"济南市甸柳第一小学",年级:"3年级"},
  {赛项:"算法编程（scratch）",姓名:"李家辉",参赛编号:"",比赛场馆:"3号场馆2楼编程考场5",时间:"13:00-14:00",组别:"小学组全年级",学校:"济南市历下区实验小学",年级:"3年级"},
  {赛项:"算法编程（scratch）",姓名:"李旻泰",参赛编号:"",比赛场馆:"3号场馆2楼编程考场5",时间:"13:00-14:00",组别:"小学组全年级",学校:"济南历下区历山双语学校",年级:"3年级"},
  {赛项:"算法编程（scratch）",姓名:"傅圣杰",参赛编号:"",比赛场馆:"3号场馆2楼编程考场5",时间:"13:00-14:00",组别:"小学组全年级",学校:"济南市历下区东方双语实验小学",年级:"4年级"},
  {赛项:"算法编程（scratch）",姓名:"孙远淳",参赛编号:"",比赛场馆:"3号场馆2楼编程考场5",时间:"13:00-14:00",组别:"小学组全年级",学校:"历城六中教育集团清河校区",年级:"3年级"},
  {赛项:"算法编程（Python）",姓名:"伊泽诺",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市解放路第二小学",年级:"4年级"},
  {赛项:"算法编程（Python）",姓名:"张雨桐",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"13:00-14:30",组别:"小学组全年级",学校:"历城双语实验学校",年级:"4年级"},
  {赛项:"算法编程（Python）",姓名:"朱靖桓",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市天桥区云世界实验小学",年级:"3年级"},
  {赛项:"算法编程（Python）",姓名:"姜沐阳",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市历城区实验小学",年级:"5年级"},
  {赛项:"算法编程（Python）",姓名:"李旻橡",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市历山双语学校",年级:"4年级"},
  {赛项:"算法编程（Python）",姓名:"郎一宸",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市历下区历山双语学校",年级:"3年级"},
  {赛项:"算法编程（Python）",姓名:"张梓熙",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市历下区弘毅小学",年级:"6年级"},
  {赛项:"算法编程（Python）",姓名:"冯天奇",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市历城双语实验学校",年级:"4年级"},
  {赛项:"算法编程（Python）",姓名:"高屹轩",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市历城第六中学",年级:"5年级"},
  {赛项:"算法编程（Python）",姓名:"王谦毅",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"13:00-14:30",组别:"小学组全年级",学校:"济南市解放路第二小学",年级:"4年级"},
  {赛项:"算法编程（C++）",姓名:"程欣臻",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市历城第六中学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"马茗宇",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南天山实验学校",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"张恒瑜",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"15:00-17:00",组别:"小学组全年级",学校:"历城区洪家楼第三小学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"臧敏灿",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"15:00-17:00",组别:"小学组全年级",学校:"洪家楼第二小学",年级:"5年级"},
  {赛项:"算法编程（C++）",姓名:"赵家和",参赛编号:"",比赛场馆:"1号场馆2楼编程考场3",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市历城实验小学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"杨韵涵",参赛编号:"",比赛场馆:"1号场馆2楼编程考场2",时间:"15:00-17:00",组别:"小学组全年级",学校:"解放路第二小学",年级:"5年级"},
  {赛项:"算法编程（C++）",姓名:"张茗睿",参赛编号:"",比赛场馆:"1号场馆2楼编程考场2",时间:"15:00-17:00",组别:"小学组全年级",学校:"历下区历山双语学校",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"郝开源",参赛编号:"",比赛场馆:"1号场馆2楼编程考场2",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市历城区实验小学",年级:"5年级"},
  {赛项:"算法编程（C++）",姓名:"常宸睿",参赛编号:"",比赛场馆:"1号场馆2楼编程考场2",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市历城区工业北路第二小学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"路箫菀",参赛编号:"",比赛场馆:"1号场馆2楼编程考场2",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市历下实验小学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"赵翊翀",参赛编号:"",比赛场馆:"1号场馆2楼编程考场2",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市棋盘街小学",年级:"5年级"},
  {赛项:"算法编程（C++）",姓名:"何秋霖",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市历城区北全福小学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"张伦语",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"15:00-17:00",组别:"小学组全年级",学校:"南全福小学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"刘恩硕",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"15:00-17:00",组别:"小学组全年级",学校:"燕山小学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"张峻齐",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"15:00-17:00",组别:"小学组全年级",学校:"历城区华山第二小学",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"高雅",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市洪家楼小学",年级:"5年级"},
  {赛项:"算法编程（C++）",姓名:"乔振宇",参赛编号:"",比赛场馆:"1号场馆2楼编程考场1",时间:"15:00-17:00",组别:"小学组全年级",学校:"济南市历城区华山小学南校区",年级:"6年级"},
  {赛项:"算法编程（C++）",姓名:"张瀚文",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"15:00-17:00",组别:"初中组",学校:"济南市历城区双语实验学校",年级:"7年级"},
  {赛项:"算法编程（C++）",姓名:"葛一平",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"15:00-17:00",组别:"初中组",学校:"章丘双语学校",年级:"8年级"},
  {赛项:"算法编程（C++）",姓名:"曹舒博",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"15:00-17:00",组别:"初中组",学校:"济南市历城区第三中学",年级:"7年级"},
  {赛项:"算法编程（C++）",姓名:"孙若涵",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"15:00-17:00",组别:"初中组",学校:"济南市燕山中学",年级:"7年级"},
  {赛项:"算法编程（C++）",姓名:"张龙轩",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"15:00-17:00",组别:"初中组",学校:"济南市天桥区云锦中学",年级:"7年级"},
  {赛项:"算法编程（C++）",姓名:"蒋宇勃锐",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"15:00-17:00",组别:"初中组",学校:"济南市历下区燕山中学",年级:"8年级"},
  {赛项:"算法编程（C++）",姓名:"何梓林",参赛编号:"",比赛场馆:"3号场馆2楼编程考场5",时间:"15:00-17:00",组别:"初中组",学校:"济南市历下区历山双语学校",年级:"7年级"},
  {赛项:"算法编程（C++）",姓名:"张沐辰",参赛编号:"",比赛场馆:"3号场馆2楼编程考场5",时间:"15:00-17:00",组别:"初中组",学校:"山东济南历城区华山第二中学",年级:"7年级"},
  {赛项:"算法编程（C++）",姓名:"刘先航",参赛编号:"",比赛场馆:"3号场馆2楼编程考场5",时间:"15:00-17:00",组别:"初中组",学校:"山东省济南第五中学",年级:"9年级"},
  {赛项:"算法编程（GOC）",姓名:"王睿涵",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"08:30-10:00",组别:"小学组全年级",学校:"济南市历城区南全福小学",年级:"5年级"},
  {赛项:"算法编程（GOC）",姓名:"崔凌赫",参赛编号:"",比赛场馆:"3号场馆2楼编程考场4",时间:"08:30-10:00",组别:"小学组全年级",学校:"济南市历下实验小学",年级:"5年级"}
];

const input = document.getElementById('nameInput');
const resultArea = document.getElementById('resultArea');

function esc(s) {
  return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}

function render(query) {
  const q = query.trim();
  if (!q) {
    resultArea.innerHTML = '<div class="empty">请输入姓名开始查询</div>';
    return;
  }
  const hits = DATA.filter(item => item.姓名 === q);
  if (hits.length === 0) {
    resultArea.innerHTML = '<div class="no-result"><div class="big">🔍</div>未找到该姓名的考生<br>请检查姓名是否输入正确</div>';
    return;
  }
  let html = '';
  if (hits.length > 1) {
    html += `<div class="count-tip">找到 ${hits.length} 条记录，请根据学校和年级确认</div>`;
  }
  hits.forEach(item => {
    const fullTime = DATE_PREFIX + item.时间;
    html += `
      <div class="result-card">
        <div class="name-row">
          <span class="name">${esc(item.姓名)}</span>
          <span class="group-tag">${esc(item.组别)}</span>
        </div>
        <div class="info-row"><span class="info-label">赛项</span><span class="info-value">${esc(item.赛项)}</span></div>
        <div class="info-row"><span class="info-label">比赛基地</span><span class="info-value highlight">${esc(VENUE_BASE)}</span></div>
        <div class="info-row"><span class="info-label">具体场馆</span><span class="info-value highlight">${esc(item.比赛场馆)}</span></div>
        <div class="info-row"><span class="info-label">时间</span><span class="info-value highlight">${esc(fullTime)}</span></div>
        <div class="info-row"><span class="info-label">学校</span><span class="info-value">${esc(item.学校)}</span></div>
        <div class="info-row"><span class="info-label">年级</span><span class="info-value">${esc(item.年级)}</span></div>
        <div class="info-row"><span class="info-label">参赛编号</span><span class="info-value">${item.参赛编号 ? esc(item.参赛编号) : '—'}</span></div>
      </div>
    `;
  });
  resultArea.innerHTML = html;
}

input.addEventListener('input', e => render(e.target.value));
</script>
</body>
</html>
