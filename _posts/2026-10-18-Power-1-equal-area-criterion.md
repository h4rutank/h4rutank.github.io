---
title: "電力 (1) 過渡安定度と等面積法"
date: 2026-10-18
categories: Electronics
math: true
---
## Introduction
送電線への落雷などで、電力系統では地絡・短絡事故が起きる。<br>
事故が起きると、保護リレーが検出して遮断器で事故区間を切り離す。この間、発電機は負荷が減るのに、タービンからの機械入力はそのままなので加速する。<br>
切り離しが遅れると、発電機が加速しすぎて脱調する。つまり、系統と同期を保てなくなる。<br><br>

そこで、「事故を何秒以内に除去すれば、発電機は同期を保てるか」が重要であり、その指標が安定度である。<br><br>

安定度の計算は電験でも出題されており、電験一種の勉強のために、安定度計算の基本となる**等面積法**についてこの記事でまとめる。<br><br>

電力系統の安定度は、擾乱の大きさと制御の扱いで、次の3つに分けられる。<br>

| 種類 | 擾乱 | 概要 |
|-|-|-|
| 定態安定度 | 非常に小さい | 負荷や出力がゆっくり変化しても同期を保てるか |
| 過渡安定度 | 大きい（短絡・地絡、線路の開放など） | 故障などの大きな擾乱の後も同期を保てるか |
| 動態安定度 | 小さい | AVR・調速機を含めた動揺を減衰して収束できるか |

<br>
事故のような大きな擾乱を扱うのは**過渡安定度**で、事故直後の最初の揺れで脱調するかどうかの判定に等面積法が使える。<br><br>

---
## 1. 等面積法
### 1.1. 動揺方程式
発電機の電気出力 $$P_e$$ は、相差角 $$\delta$$ で決まる。<br>

$$
P_e = \frac{EV}{X}\sin\delta = P_{\max}\sin\delta
$$

$$E$$ は発電機の内部誘起電圧、$$V$$ は無限大母線の電圧、$$X$$ はその間のリアクタンス。<br>
**相差角** $$\delta$$ は、$$E$$ と $$V$$ の位相差。発電機の回転子が、系統の基準からどれだけ先に進んで回っているかを表す角度と考えればよい。<br>
$$\delta$$ が大きいほど多くの電力を送れるが、90° を超えると逆に減っていく。回転子が系統より速く回ると $$\delta$$ が増え、遅く回ると減る。<br>

回転子の動きは、動揺方程式で表される。<br>

$$
\frac{2H}{\omega_s}\frac{d^2\delta}{dt^2} = P_m - P_e
$$

* $$H$$: 単位慣性定数 [s]、$$\omega_s = 2\pi f$$: 同期角速度 [rad/s]
* $$P_m$$: 機械入力
* $$P_e$$: 電気出力

$$P_m > P_e$$ なら加速して $$\delta$$ が増え、$$P_m < P_e$$ なら減速する。<br><br>

### 1.2. 加速エネルギーと減速エネルギー
故障が起きると $$X$$ が変わるので、$$P_{\max}$$ も変わる。<br>
故障前・故障中・除去後の電気出力を、$$P_1\sin\delta$$、$$P_2\sin\delta$$、$$P_3\sin\delta$$ とする。<br>

- 平常時は $$P_m = P_1\sin\delta_0$$ を満たす $$\delta_0$$ で運転している
- 故障中は $$P_e$$ が下がるので加速する。溜まるエネルギーが**加速面積 $$A_1$$**（赤色）
- $$\delta_c$$ で故障を除去すると $$P_e > P_m$$ になり減速する。吐き出せるエネルギーが**減速面積 $$A_2$$**（緑色）

減速できるのは、除去後の曲線が $$P_m$$ を下回る $$\delta_{\max}$$ まで。<br>

$$
\delta_{\max} = \pi - \sin^{-1}\frac{P_m}{P_3}
$$

$$A_2 \geq A_1$$ なら安定、$$A_2 < A_1$$ なら脱調。$$A_1 = A_2$$ になる $$\delta_c$$ が**臨界故障除去角**。<br><br>

<style>
.eac { border: 1px solid #e8e8e8; border-radius: 4px; padding: 12px; margin: 0 0 15px; }
.eac svg { width: 100%; height: auto; display: block; }
.eac-readout { font-size: 15px; line-height: 1.7; margin: 8px 0; }
.eac-verdict-ok { color: #1e8c45; font-weight: bold; }
.eac-verdict-ng { color: #c62828; font-weight: bold; }
.eac-controls { display: grid; grid-template-columns: repeat(auto-fit, minmax(min(100%, 240px), 1fr)); gap: 6px 16px; font-size: 14px; }
.eac-controls label { display: grid; grid-template-columns: 6.5em minmax(0, 1fr) 3.8em; align-items: center; gap: 6px; }
.eac-controls input { width: 100%; min-width: 0; margin: 0; }
.eac-controls output { text-align: right; font-family: Menlo, Consolas, monospace; }
</style>
<div class="eac">
  <svg id="eac-svg" viewBox="0 0 640 400" role="img" aria-label="P-δ曲線と加速面積・減速面積"></svg>
  <div class="eac-readout" id="eac-readout"></div>
  <div class="eac-controls">
    <label><span>故障中 P<sub>2</sub></span> <input type="range" id="eac-p2" min="0" max="1" step="0.01" value="0.3"><output id="eac-p2-out"></output></label>
    <label><span>除去後 P<sub>3</sub></span> <input type="range" id="eac-p3" min="0.1" max="1" step="0.01" value="0.9"><output id="eac-p3-out"></output></label>
    <label><span>機械入力 P<sub>m</sub></span> <input type="range" id="eac-pm" min="0.05" max="0.95" step="0.01" value="0.5"><output id="eac-pm-out"></output></label>
    <label><span>除去角 δ<sub>c</sub></span> <input type="range" id="eac-dc" min="0" max="180" step="0.1" value="70"><output id="eac-dc-out"></output></label>
  </div>
</div>
<script>
(function () {
  var NS = "http://www.w3.org/2000/svg";
  var svg = document.getElementById("eac-svg");
  var readout = document.getElementById("eac-readout");
  var keys = ["p2", "p3", "pm", "dc"];
  var P1 = 1;  // 故障前の曲線の最大値は 1 pu に固定
  var inp = {};
  keys.forEach(function (k) { inp[k] = document.getElementById("eac-" + k); });
  var W = 640, H = 400, L = 48, R = 16, T = 16, B = 40, pw = W - L - R, ph = H - T - B;
  var YMAX = 1.2;
  var dragging = false;
  var rad = function (d) { return d * Math.PI / 180; };
  var deg = function (r) { return r * 180 / Math.PI; };

  function el(name, attrs, text) {
    var e = document.createElementNS(NS, name);
    Object.keys(attrs).forEach(function (k) { e.setAttribute(k, attrs[k]); });
    if (text !== undefined) e.textContent = text;
    svg.appendChild(e);
    return e;
  }

  // 等面積法の計算。角度は rad
  function calc(P1, P2, P3, Pm, dc) {
    var r = { ok0: Pm < P1, okm: Pm < P3 };
    if (!r.ok0) return r;
    r.d0 = Math.asin(Pm / P1);
    r.dmax = r.okm ? Math.PI - Math.asin(Pm / P3) : null;
    var A1 = function (x) { return Pm * (x - r.d0) + P2 * (Math.cos(x) - Math.cos(r.d0)); };
    var A2 = function (x) {
      return (r.okm && x < r.dmax) ? P3 * (Math.cos(x) - Math.cos(r.dmax)) - Pm * (r.dmax - x) : 0;
    };
    r.A1 = A1(dc);
    r.A2 = A2(dc);
    r.stable = r.okm && dc < r.dmax && r.A2 >= r.A1;
    if (!r.okm || r.dmax <= r.d0) {
      r.crit = "none";
    } else {
      var g = function (x) { return A2(x) - A1(x); };
      if (g(r.dmax) >= 0) {
        r.crit = "always";
      } else {
        var a = r.d0, b = r.dmax;
        for (var i = 0; i < 60; i++) {
          var m = (a + b) / 2;
          if (g(m) >= 0) a = m; else b = m;
        }
        r.crit = a;
      }
    }
    return r;
  }

  function draw() {
    var P3 = +inp.p3.value, Pm = +inp.pm.value;
    // 故障中の曲線は除去後より低い
    if (+inp.p2.value > P3) inp.p2.value = P3;
    var P2 = +inp.p2.value;
    var r0 = calc(P1, P2, P3, Pm, 0);
    var minDc = r0.ok0 ? Math.ceil(deg(r0.d0) * 10) / 10 : 0;
    inp.dc.min = minDc;
    if (+inp.dc.value < minDc) inp.dc.value = minDc;
    var dcDeg = +inp.dc.value, dc = rad(dcDeg);
    var r = calc(P1, P2, P3, Pm, dc);

    ["p2", "p3", "pm"].forEach(function (k) {
      document.getElementById("eac-" + k + "-out").textContent = (+inp[k].value).toFixed(2);
    });
    document.getElementById("eac-dc-out").textContent = dcDeg.toFixed(1) + "°";

    svg.textContent = "";
    var ymax = YMAX;
    var X = function (d) { return L + pw * d / 180; };
    var Y = function (p) { return T + ph * (1 - p / ymax); };

    for (var d = 0; d <= 180; d += 30) {
      el("line", { x1: X(d), x2: X(d), y1: T, y2: T + ph, stroke: "#eee" });
      el("text", { x: X(d), y: T + ph + 16, "font-size": 12, "text-anchor": "middle", fill: "#666" }, d + "°");
    }
    for (var p = 0; p <= ymax + 1e-9; p += 0.2) {
      el("line", { x1: L, x2: L + pw, y1: Y(p), y2: Y(p), stroke: "#eee" });
      el("text", { x: L - 6, y: Y(p) + 4, "font-size": 12, "text-anchor": "end", fill: "#666" }, p.toFixed(1));
    }
    el("text", { x: L + pw, y: T + ph + 34, "font-size": 12, "text-anchor": "end", fill: "#666" }, "相差角 δ");
    el("text", { x: L, y: T - 4, "font-size": 12, fill: "#666" }, "P [pu]");

    function pts(from, to, f) {
      var a = [];
      for (var x = from; x < to; x += 0.5) a.push(X(x) + "," + Y(f(x)));
      a.push(X(to) + "," + Y(f(to)));
      return a;
    }
    var sinP = function (P) { return function (x) { return P * Math.sin(rad(x)); }; };
    var flat = function () { return Pm; };

    if (r.ok0) {
      var d0 = deg(r.d0);
      el("polygon", {
        points: pts(d0, dcDeg, flat).concat(pts(d0, dcDeg, sinP(P2)).reverse()).join(" "),
        fill: "rgba(220, 50, 47, 0.35)"
      });
      if (r.okm && dc < r.dmax) {
        var dm = deg(r.dmax);
        el("polygon", {
          points: pts(dcDeg, dm, sinP(P3)).concat(pts(dcDeg, dm, flat).reverse()).join(" "),
          fill: "rgba(40, 160, 80, 0.35)"
        });
      }
    }

    el("polyline", { points: pts(0, 180, sinP(P1)).join(" "), fill: "none", stroke: "#999", "stroke-width": 1.5, "stroke-dasharray": "5 4" });
    el("polyline", { points: pts(0, 180, sinP(P2)).join(" "), fill: "none", stroke: "#e67e22", "stroke-width": 2 });
    el("polyline", { points: pts(0, 180, sinP(P3)).join(" "), fill: "none", stroke: "#2a7ae2", "stroke-width": 2 });
    el("line", { x1: L, x2: L + pw, y1: Y(Pm), y2: Y(Pm), stroke: "#333", "stroke-width": 1.5 });
    // 機械入力の横線はドラッグで上下に動かせる。つかみやすいよう透明な太線と丸いつまみを重ねる
    var hit = el("line", { x1: L, x2: L + pw, y1: Y(Pm), y2: Y(Pm), stroke: "transparent", "stroke-width": 18, style: "cursor: ns-resize" });
    var knob = el("circle", { cx: L + 14, cy: Y(Pm), r: 6, fill: "#333", style: "cursor: ns-resize" });
    el("text", { x: L + 24, y: Y(Pm) - 6, "font-size": 12, fill: "#333" }, "Pm ↕");
    [hit, knob].forEach(function (h) {
      h.addEventListener("pointerdown", startDrag);
      h.addEventListener("touchstart", function (e) { e.preventDefault(); }, { passive: false });
    });

    function marker(dDeg, label, color, row) {
      el("line", { x1: X(dDeg), x2: X(dDeg), y1: T, y2: T + ph, stroke: color, "stroke-dasharray": "3 3" });
      el("text", { x: X(dDeg) + 3, y: T + ph - 6 - 16 * (row || 0), "font-size": 12, fill: color }, label);
    }
    if (r.ok0) marker(deg(r.d0), "δ0", "#555");
    if (r.ok0 && typeof r.crit === "number") marker(deg(r.crit), "臨界", "#7b1fa2", 1);
    marker(dcDeg, "δc", "#c62828");
    if (r.ok0 && r.okm) marker(deg(r.dmax), "δmax", "#555");

    var legend = [["#999", "故障前 sinδ（1 pu）"], ["#e67e22", "故障中 P2 sinδ"], ["#2a7ae2", "除去後 P3 sinδ"], ["#333", "機械入力 Pm"]];
    legend.forEach(function (item, i) {
      var y = T + 12 + i * 17;
      el("line", { x1: L + pw - 150, x2: L + pw - 128, y1: y - 4, y2: y - 4, stroke: item[0], "stroke-width": 2 });
      el("text", { x: L + pw - 122, y: y, "font-size": 12, fill: "#333" }, item[1]);
    });

    var html;
    if (!r.ok0) {
      html = "P<sub>m</sub> ≥ P<sub>1</sub> なので、故障前の運転点がない";
    } else {
      html = "δ<sub>0</sub> = " + deg(r.d0).toFixed(1) + "°　" +
        "δ<sub>max</sub> = " + (r.okm ? deg(r.dmax).toFixed(1) + "°" : "なし") + "　" +
        "δ<sub>c</sub> = " + dcDeg.toFixed(1) + "°<br>" +
        "A<sub>1</sub> = " + r.A1.toFixed(3) + "　A<sub>2</sub> = " + r.A2.toFixed(3) + "　[pu·rad]<br>" +
        "判定：" + (r.stable
          ? '<span class="eac-verdict-ok">安定（A<sub>2</sub> ≥ A<sub>1</sub>）</span>'
          : '<span class="eac-verdict-ng">脱調（A<sub>2</sub> &lt; A<sub>1</sub>）</span>');
      html += "<br>臨界故障除去角：" + (r.crit === "none" ? "除去後の運転点がないので、必ず脱調する"
        : r.crit === "always" ? "δ<sub>max</sub> までのどこで除去しても安定"
        : '<span style="color:#7b1fa2">' + deg(r.crit).toFixed(1) + "°</span>");
    }
    readout.innerHTML = html;
  }

  function startDrag(e) {
    dragging = true;
    svg.setPointerCapture(e.pointerId);
    e.preventDefault();
  }
  svg.addEventListener("pointermove", function (e) {
    if (!dragging) return;
    var pt = svg.createSVGPoint();
    pt.x = e.clientX;
    pt.y = e.clientY;
    var v = pt.matrixTransform(svg.getScreenCTM().inverse());
    var p = (1 - (v.y - T) / ph) * YMAX;
    p = Math.min(+inp.pm.max, Math.max(+inp.pm.min, p));
    inp.pm.value = p.toFixed(2);
    draw();
  });
  ["pointerup", "pointercancel"].forEach(function (t) {
    svg.addEventListener(t, function () { dragging = false; });
  });

  keys.forEach(function (k) { inp[k].addEventListener("input", draw); });
  draw();
})();
</script>

<br><br>

スライダーで条件を変えると、$$A_1$$（赤）と $$A_2$$（緑）が変わる。紫の縦線が臨界故障除去角。<br>
故障前の曲線の最大値は 1 pu に固定してあるので、ほかの値は「故障前の $$P_{\max}$$ の何倍か」と考えてほしい。<br>
機械入力 $$P_m$$ の大きさは、グラフ上でつかんで上下に動かすこともできる。機械入力は故障前後で一定とする。<br>




<br>

### 1.3. 計算

次に、加速エネルギーと減速エネルギーを計算で求める。<br>
動揺方程式の両辺に $$d\delta/dt$$ を掛けて時間で積分すると、次の式になる。<br>

$$
\frac{H}{\omega_s}\left(\frac{d\delta}{dt}\right)^2 = \int_{\delta_0}^{\delta}(P_m - P_e)\,d\delta
$$

左辺は回転子の余分な運動エネルギー。回転子が折り返す点では $$d\delta/dt = 0$$ なので、右辺の積分がゼロになる。<br>
つまり、加速した分（$$A_1$$）と減速した分（$$A_2$$）が等しくなったところで折り返す。<br><br>


$$A_1$$ と $$A_2$$ は、それぞれ次のように計算できる。<br>

$$
\begin{aligned}
A_1 &= \int_{\delta_0}^{\delta_c}(P_m - P_2\sin\delta)\,d\delta = P_m(\delta_c - \delta_0) + P_2(\cos\delta_c - \cos\delta_0) \\
A_2 &= \int_{\delta_c}^{\delta_{\max}}(P_3\sin\delta - P_m)\,d\delta = P_3(\cos\delta_c - \cos\delta_{\max}) - P_m(\delta_{\max} - \delta_c)
\end{aligned}
$$

$$A_1 = A_2$$ を $$\cos\delta_c$$ について解くと、臨界故障除去角の一般式になる。<br>

$$
\cos\delta_c = \frac{P_m(\delta_{\max} - \delta_0) + P_3\cos\delta_{\max} - P_2\cos\delta_0}{P_3 - P_2}
$$

故障中 $$P_2 = 0$$、除去後が故障前と同じ $$P_3 = P_1 = P_{\max}$$ なら、次のように簡単になる。<br>

$$
\cos\delta_c = \frac{P_m}{P_{\max}}(\pi - 2\delta_0) - \cos\delta_0
$$
<br><br>

---
## 2. 例題
$$P_{\max} = 2.0$$ pu、$$P_m = 1.0$$ puの発電機端で三相短絡（故障中 $$P_e = 0$$）が起き、除去後は故障前と同じ系統に戻る。ただし、単位慣性係数$$H = 5$$ s、$$f = 50$$ Hzとする。<br><br>

### 2.1. 臨界故障除去角
* 初期相差角$$\delta_0 = \sin^{-1}(1.0/2.0) = 30^\circ$$
* $$\delta_{\max} = 180^\circ - 30^\circ = 150^\circ$$
* 加速エネルギー $$A_1=\delta_c - \frac{\pi}{6}$$
* 減速エネルギー $$A_2=2(\cos\delta_c - \cos 150^\circ) - (\frac{5\pi}{6} - \delta_c)$$

等面積法より、加速エネルギーと減速エネルギーは等しくなるので、

$$
\cos\delta_c = \frac{1}{2} \times \frac{2\pi}{3} - \frac{\sqrt{3}}{2} = 0.181
\quad\Rightarrow\quad \delta_c \approx 79.6^\circ
$$

### 2.2. 臨界故障除去時間
故障中は $$P_e = 0$$ なので、加速度は一定になる。<br>

$$
\frac{d^2\delta}{dt^2} = \frac{\omega_s}{2H}P_m = \frac{2\pi \times 50}{2 \times 5} \times 1.0 \approx 31.4 \ \text{rad/s}^2
$$

$$\Delta\delta = 79.6^\circ - 30^\circ = 0.865$$ rad 進むまでの時間は、次のとおり。<br>

$$
t = \sqrt{\frac{2\Delta\delta}{d^2\delta/dt^2}} = \sqrt{\frac{2 \times 0.865}{31.4}} \approx 0.235 \ \text{s}
$$

50 Hz で約11.7サイクル。<br><br>

---
## 3. 安定度向上対策
等面積法の図のどこに効くかで整理すると覚えやすい。<br>

| 狙い | 図での効果 | 対策 |
|-|-|-|
| リアクタンス X を下げる | 曲線が高くなり、A<sub>2</sub> が増える | 並行回線の増設、直列コンデンサ、多導体化、低リアクタンス変圧器、短絡比の大きい発電機 |
| 電圧を保つ | 曲線が低くならない | 速応励磁（高速AVR）、SVC |
| 故障を速く除く | δ<sub>c</sub> が小さくなり、A<sub>1</sub> が減る | 高速遮断、高速度再閉路 |
| 加速エネルギーを減らす | A<sub>1</sub> が減る | 高速バルブ制御、電源制限、制動抵抗 |
| 振動を抑える | 第2波以降の動揺が減衰する | PSS |
| 脱調の影響を広げない | （被害を局所化する） | 系統分離 |

<br>

---
## Summary
等面積法は、事故中の加速エネルギー（$$A_1$$）と除去後に減速エネルギー（$$A_2$$）を面積で比べて過渡安定度を計算する手法です。<br>
臨界故障除去角が分かれば、事故を何秒以内に除去すればよいか（臨界故障除去時間）が決まる。<br>
安定度向上対策も、$$A_1$$ を減らすか $$A_2$$ を増やすかで整理できる。<br><br>

大学の電力工学の講義での「電力系統は人間が生み出した最大のシステムです。」という教授の言葉が印象に残っている。現代の最大のシステムは通信ネットワークだろうけど。<br>
ただ、電力系統という必要不可欠な大規模システムが高校数学で理解できる原理で動いているというのがとても面白い。電力系統の魅力は、そのシンプルさだと思います。<br><br>