---
title: 選擇題測驗卷網站講義（學生版）.md

---

---
title: 選擇題測驗卷網站講義（學生版）

---

---
title: 選擇題測驗卷網站講義（學生版）
tags: [114程式設計與實習_上學期]

---

# 選擇題測驗卷網站講義（學生版）

學號：４１５７３００８３　　姓名：陳沁儀

> **填寫方式**
> 1. 每個學習都要放：**執行截圖**、**三次問 AI 的提示詞**、**最後採用的程式碼**。
> 2. 問 AI 的提示詞請**逐字貼上**自己實際輸入的內容（不要寫摘要），第一次、第二次、第三次依序記錄。
> 3. 程式碼貼在「點開貼上」的收合區塊裡，貼上**你最後真正採用、而且能執行**的版本。

---

## 學習1：產生一個選擇題測驗卷網站

https://cfchen58.synology.me/115/week4/stage1/

**這個階段的目標：** 用 p5.js 做出一個一次顯示一題、四個選項、答完會顯示對錯與總分的測驗網站（題目先寫在程式裡）。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

![動畫](https://hackmd.io/_uploads/BJEYqh4ifl.gif)


![學習1截圖](請貼上截圖)

### 第一次問 AI

```tex!
使用ｐ５撰寫一個選擇題網頁系統，我已經產生一個ｐ５．ｊｓ專案，請把程式碼寫到ｓｋｅｔｃｈ．ｊｓ檔案內，每條指令都需要加上中文註解，測驗系統題目設定為五題，測驗題目的內容為程式設計ｐ５．ｊｓ簡易指令練習測驗，系統採用全螢幕畫布，使用者答錯時，系統會在正確答案選項上，加上ffc2d1背景顏色，該選項要上下跳動，答錯的選項採用fb6f92背景顏色，選項左右移動，選擇題選項共有四個選項，當五題結束後，需要顯示答對的題數，每顯示一個題目，需要有下一題的按鈕
```

### 第二次問 AI

```tex!
題目的顯示到整個視窗畫布的右邊，造成無法全部正確顯示，題目請顯示在整個視窗的中間，並加上方框，方框的背景顏色為f4acb7
```

### 第三次問 AI

```tex!
哪一個指令可以繪製圓形的字要在方框中間
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習1的程式碼
```javascript=
// 儲存五題測驗資料
let questions = [
  {
    // 設定第一題題目
    question: "哪一個指令可以繪製圓形？",

    // 設定第一題的四個選項
    options: ["rect()", "ellipse()", "line()", "triangle()"],

    // 設定正確答案的索引值
    answer: 1
  },
  {
    // 設定第二題題目
    question: "哪一個指令可以設定背景顏色？",

    // 設定第二題的四個選項
    options: ["background()", "fill()", "stroke()", "noLoop()"],

    // 設定正確答案的索引值
    answer: 0
  },
  {
    // 設定第三題題目
    question: "哪一個指令可以繪製直線？",

    // 設定第三題的四個選項
    options: ["line()", "point()", "arc()", "quad()"],

    // 設定正確答案的索引值
    answer: 0
  },
  {
    // 設定第四題題目
    question: "哪一個指令可以繪製矩形？",

    // 設定第四題的四個選項
    options: ["rect()", "square()", "box()", "circle()"],

    // 設定正確答案的索引值
    answer: 0
  },
  {
    // 設定第五題題目
    question: "哪一個指令可以繪製文字？",

    // 設定第五題的四個選項
    options: ["text()", "print()", "println()", "loadFont()"],

    // 設定正確答案的索引值
    answer: 0
  }
];

// 設定目前題目的索引
let currentQuestion = 0;

// 設定使用者選擇的選項索引
let selectedOption = -1;

// 設定答對的題數
let score = 0;

// 設定測驗狀態
let quizState = "answering";

// 設定動畫開始時間
let animationStartTime = 0;

// 設定題目方框資料
let questionBox = {};

// 設定選項方框資料
let optionBoxes = [];

// 設定下一題按鈕資料
let nextButton = {};

// 建立畫布
function setup() {
  // 建立符合瀏覽器視窗大小的畫布
  createCanvas(windowWidth, windowHeight);

  // 設定文字水平置中與垂直置中
  textAlign(CENTER, CENTER);

  // 設定文字使用正常樣式
  textStyle(NORMAL);

  // 設定文字使用一般無襯線字型
  textFont("Arial");

  // 設定矩形從左上角開始繪製
  rectMode(CORNER);
}

// 繪製畫面
function draw() {
  // 判斷測驗是否已經完成
  if (quizState === "finished") {
    // 顯示測驗結果
    drawResult();

    // 結束目前繪圖
    return;
  }

  // 設定畫面背景顏色
  background("#fff8fa");

  // 顯示測驗畫面
  drawQuiz();
}

// 繪製測驗畫面
function drawQuiz() {
  // 取得目前題目
  let question = questions[currentQuestion];

  // 設定內容左右邊距
  let margin = min(width * 0.08, 100);

  // 設定題目方框寬度
  let questionWidth = min(width * 0.84, 1000);

  // 設定題目方框高度
  let questionHeight = min(height * 0.16, 140);

  // 設定題目方框 x 座標
  let questionX = width / 2 - questionWidth / 2;

  // 設定題目方框 y 座標
  let questionY = height * 0.18;

  // 儲存題目方框位置
  questionBox = {
    // 儲存題目方框 x 座標
    x: questionX,

    // 儲存題目方框 y 座標
    y: questionY,

    // 儲存題目方框寬度
    w: questionWidth,

    // 儲存題目方框高度
    h: questionHeight
  };

  // 設定標題文字顏色
  fill("#6d435a");

  // 設定標題文字大小
  textSize(min(width * 0.045, 42));

  // 設定文字置中
  textAlign(CENTER, CENTER);

  // 顯示測驗標題
  text("p5.js 簡易指令練習測驗", width / 2, height * 0.06);

  // 設定題數文字顏色
  fill("#9b607f");

  // 設定題數文字大小
  textSize(min(width * 0.025, 24));

  // 顯示目前題數
  text(
    "第 " + (currentQuestion + 1) + " 題，共 " + questions.length + " 題",
    width / 2,
    height * 0.115
  );

  // 設定題目方框背景顏色
  fill("#f4acb7");

  // 設定題目方框外框顏色
  stroke("#b56b86");

  // 設定題目方框外框粗細
  strokeWeight(2);

  // 繪製題目方框
  rect(questionBox.x, questionBox.y, questionBox.w, questionBox.h, 14);

  // 儲存目前繪圖狀態
  push();

  // 移除題目文字外框
  noStroke();

  // 設定題目文字顏色
  fill("#33232d");

  // 設定題目文字大小
  textSize(min(width * 0.032, 32));

  // 設定題目文字水平與垂直置中
  textAlign(CENTER, CENTER);

  // 設定文字使用正常樣式
  textStyle(NORMAL);

  // 將題目文字直接畫在題目方框中心
  text(
    question.question,
    questionBox.x + questionBox.w / 2,
    questionBox.y + questionBox.h / 2
  );

  // 還原繪圖狀態
  pop();

  // 設定選項高度
  let optionHeight = min(height * 0.075, 65);

  // 設定選項間距
  let optionGap = min(height * 0.018, 16);

  // 設定選項起始 y 座標
  let optionStartY = questionBox.y + questionBox.h + height * 0.05;

  // 清除上一幀的選項資料
  optionBoxes = [];

  // 逐一繪製四個選項
  for (let i = 0; i < question.options.length; i++) {
    // 設定選項 x 座標
    let optionX = margin;

    // 設定選項 y 座標
    let optionY = optionStartY + i * (optionHeight + optionGap);

    // 設定選項寬度
    let optionWidth = width - margin * 2;

    // 設定水平動畫位移
    let moveX = 0;

    // 設定垂直動畫位移
    let moveY = 0;

    // 判斷目前選項是否為正確答案
    let isCorrect = i === question.answer;

    // 判斷目前選項是否為使用者選錯的答案
    let isWrong =
      quizState === "feedback" &&
      selectedOption === i &&
      selectedOption !== question.answer;

    // 判斷使用者是否答錯
    let isAnswerWrong =
      quizState === "feedback" &&
      selectedOption !== question.answer;

    // 答錯時，讓正確答案上下跳動
    if (isAnswerWrong && isCorrect) {
      // 使用正弦函式產生上下移動動畫
      moveY = sin((millis() - animationStartTime) * 0.01) * 12;
    }

    // 答錯時，讓錯誤選項左右移動
    if (isWrong) {
      // 使用正弦函式產生左右移動動畫
      moveX = sin((millis() - animationStartTime) * 0.012) * 14;
    }

    // 儲存選項方框資料
    optionBoxes.push({
      // 儲存選項 x 座標
      x: optionX,

      // 儲存選項 y 座標
      y: optionY,

      // 儲存選項寬度
      w: optionWidth,

      // 儲存選項高度
      h: optionHeight,

      // 儲存選項索引
      index: i,

      // 儲存水平位移
      moveX: moveX,

      // 儲存垂直位移
      moveY: moveY
    });

    // 儲存目前繪圖狀態
    push();

    // 套用選項動畫
    translate(moveX, moveY);

    // 設定一般選項背景顏色
    fill("#ffffff");

    // 使用者答對時，正確答案使用淡綠色
    if (
      quizState === "feedback" &&
      selectedOption === question.answer &&
      isCorrect
    ) {
      // 設定答對背景顏色
      fill("#d8f3dc");
    }

    // 使用者答錯時，正確答案使用指定粉紅色
    if (isAnswerWrong && isCorrect) {
      // 設定正確答案背景顏色
      fill("#ffc2d1");
    }

    // 使用者選錯時，錯誤選項使用指定粉紅色
    if (isWrong) {
      // 設定錯誤選項背景顏色
      fill("#fb6f92");
    }

    // 設定選項外框顏色
    stroke("#d8a9bb");

    // 設定選項外框粗細
    strokeWeight(2);

    // 繪製選項方框
    rect(optionX, optionY, optionWidth, optionHeight, 12);

    // 移除文字外框
    noStroke();

    // 設定選項文字顏色
    fill("#33232d");

    // 設定選項文字大小
    textSize(min(width * 0.028, 28));

    // 設定選項文字置中
    textAlign(CENTER, CENTER);

    // 將選項文字放在選項方框中心
    text(
      question.options[i],
      optionX + optionWidth / 2,
      optionY + optionHeight / 2
    );

    // 還原繪圖狀態
    pop();
  }

  // 設定下一題按鈕寬度
  let buttonWidth = min(width * 0.3, 260);

  // 設定下一題按鈕高度
  let buttonHeight = min(height * 0.07, 58);

  // 設定下一題按鈕 x 座標
  let buttonX = width / 2 - buttonWidth / 2;

  // 設定下一題按鈕 y 座標
  let buttonY = height - buttonHeight - height * 0.04;

  // 儲存下一題按鈕資料
  nextButton = {
    // 儲存按鈕 x 座標
    x: buttonX,

    // 儲存按鈕 y 座標
    y: buttonY,

    // 儲存按鈕寬度
    w: buttonWidth,

    // 儲存按鈕高度
    h: buttonHeight
  };

  // 尚未回答時，下一題按鈕使用灰色
  if (quizState === "answering") {
    // 設定尚未回答按鈕背景
    fill("#d9d9d9");
  } else {
    // 設定已回答按鈕背景
    fill("#ff8fab");
  }

  // 設定按鈕外框
  stroke("#b56b86");

  // 設定按鈕外框粗細
  strokeWeight(2);

  // 繪製下一題按鈕
  rect(buttonX, buttonY, buttonWidth, buttonHeight, 12);

  // 移除文字外框
  noStroke();

  // 設定按鈕文字顏色
  fill("#ffffff");

  // 設定按鈕文字大小
  textSize(min(width * 0.028, 28));

  // 設定按鈕文字置中
  textAlign(CENTER, CENTER);

  // 顯示下一題文字
  text("下一題", buttonX + buttonWidth / 2, buttonY + buttonHeight / 2);
}

// 處理滑鼠點擊
function mousePressed() {
  // 如果測驗已經完成，不執行任何操作
  if (quizState === "finished") {
    // 結束滑鼠事件
    return;
  }

  // 如果目前正在等待使用者回答
  if (quizState === "answering") {
    // 逐一檢查四個選項
    for (let option of optionBoxes) {
      // 計算選項目前的 x 座標
      let currentX = option.x + option.moveX;

      // 計算選項目前的 y 座標
      let currentY = option.y + option.moveY;

      // 判斷滑鼠是否點擊選項
      if (
        mouseX >= currentX &&
        mouseX <= currentX + option.w &&
        mouseY >= currentY &&
        mouseY <= currentY + option.h
      ) {
        // 記錄使用者選擇的索引
        selectedOption = option.index;

        // 答對時增加分數
        if (selectedOption === questions[currentQuestion].answer) {
          // 增加答對題數
          score++;
        }

        // 設定進入答案回饋狀態
        quizState = "feedback";

        // 記錄動畫開始時間
        animationStartTime = millis();

        // 停止檢查其他選項
        break;
      }
    }

    // 結束滑鼠事件
    return;
  }

  // 判斷使用者是否點擊下一題按鈕
  if (
    mouseX >= nextButton.x &&
    mouseX <= nextButton.x + nextButton.w &&
    mouseY >= nextButton.y &&
    mouseY <= nextButton.y + nextButton.h
  ) {
    // 判斷是否還有下一題
    if (currentQuestion < questions.length - 1) {
      // 前往下一題
      currentQuestion++;

      // 清除使用者選擇
      selectedOption = -1;

      // 回到答題狀態
      quizState = "answering";
    } else {
      // 設定測驗完成
      quizState = "finished";
    }
  }
}

// 顯示測驗結果
function drawResult() {
  // 設定結果畫面背景顏色
  background("#fff8fa");

  // 設定結果標題顏色
  fill("#6d435a");

  // 設定結果標題大小
  textSize(min(width * 0.07, 64));

  // 設定文字置中
  textAlign(CENTER, CENTER);

  // 顯示測驗結束文字
  text("測驗結束！", width / 2, height * 0.35);

  // 設定分數文字顏色
  fill("#33232d");

  // 設定分數文字大小
  textSize(min(width * 0.05, 46));

  // 顯示答對題數
  text(
    "你答對了 " + score + " / " + questions.length + " 題",
    width / 2,
    height * 0.5
  );

  // 設定鼓勵文字顏色
  fill("#9b607f");

  // 設定鼓勵文字大小
  textSize(min(width * 0.03, 30));

  // 顯示鼓勵訊息
  text(
    "繼續練習，你會越來越熟悉 p5.js！",
    width / 2,
    height * 0.63
  );
}

// 當瀏覽器視窗尺寸改變時執行
function windowResized() {
  // 重新調整畫布大小
  resizeCanvas(windowWidth, windowHeight);
}
```
:::


---

## 學習2：網頁設定為響應式網頁

https://cfchen58.synology.me/115/week4/stage2/

**這個階段的目標：** 讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![動畫](https://hackmd.io/_uploads/S1IVJT4jMl.gif)


### 第一次問 AI

```tex!
將網頁設定為響應式網頁
```

### 第二次問 AI

```tex!
讓網站在電腦、平板、手機（直向與橫向）都能正常顯示，視窗大小改變時版面自動調整。
```

### 第三次問 AI

```tex!

```

### 程式碼內容

:::info
:::spoiler 點開貼上學習2的程式碼
```javascript=
// 建立五題 p5.js 測驗題庫
let questions = [
  {
    // 設定第一題題目
    question: "哪一個指令可以繪製圓形？",

    // 設定第一題選項
    options: ["rect()", "ellipse()", "line()", "triangle()"],

    // 設定第一題正確答案索引
    answer: 1
  },
  {
    // 設定第二題題目
    question: "哪一個指令可以設定背景顏色？",

    // 設定第二題選項
    options: ["background()", "fill()", "stroke()", "noLoop()"],

    // 設定第二題正確答案索引
    answer: 0
  },
  {
    // 設定第三題題目
    question: "哪一個指令可以繪製直線？",

    // 設定第三題選項
    options: ["line()", "point()", "arc()", "quad()"],

    // 設定第三題正確答案索引
    answer: 0
  },
  {
    // 設定第四題題目
    question: "哪一個指令可以繪製矩形？",

    // 設定第四題選項
    options: ["rect()", "square()", "box()", "circle()"],

    // 設定第四題正確答案索引
    answer: 0
  },
  {
    // 設定第五題題目
    question: "哪一個指令可以繪製文字？",

    // 設定第五題選項
    options: ["text()", "print()", "println()", "loadFont()"],

    // 設定第五題正確答案索引
    answer: 0
  }
];

// 設定目前題目索引
let currentQuestion = 0;

// 設定使用者選擇的選項索引
let selectedOption = -1;

// 設定答對題數
let score = 0;

// 設定測驗狀態
let quizState = "answering";

// 設定動畫開始時間
let animationStartTime = 0;

// 儲存題目方框
let questionBox = {};

// 儲存選項方框
let optionBoxes = [];

// 儲存下一題按鈕
let nextButton = {};

// 建立 p5.js 畫布
function setup() {
  // 建立符合目前視窗大小的畫布
  createCanvas(windowWidth, windowHeight);

  // 設定文字水平與垂直置中
  textAlign(CENTER, CENTER);

  // 設定文字使用正常樣式
  textStyle(NORMAL);

  // 設定使用一般無襯線字型
  textFont("Arial");

  // 設定矩形從左上角開始繪製
  rectMode(CORNER);
}

// 每一幀執行一次
function draw() {
  // 判斷測驗是否完成
  if (quizState === "finished") {
    // 顯示結果畫面
    drawResult();

    // 結束目前這一幀
    return;
  }

  // 設定背景顏色
  background("#fff8fa");

  // 顯示測驗畫面
  drawQuiz();
}

// 取得響應式版面資料
function getLayout() {
  // 取得目前畫布寬度
  let currentWidth = width;

  // 取得目前畫布高度
  let currentHeight = height;

  // 判斷畫面是否為橫向
  let landscape = currentWidth > currentHeight;

  // 設定左右邊距
  let margin = constrain(currentWidth * 0.06, 16, 90);

  // 設定內容寬度
  let contentWidth = currentWidth - margin * 2;

  // 設定題目方框寬度
  let questionWidth = min(contentWidth, 1000);

  // 設定題目方框高度
  let questionHeight = landscape
    ? constrain(currentHeight * 0.18, 70, 125)
    : constrain(currentHeight * 0.14, 90, 145);

  // 設定題目方框水平位置
  let questionX = (currentWidth - questionWidth) / 2;

  // 設定題目方框垂直位置
  let questionY = landscape
    ? constrain(currentHeight * 0.18, 105, 145)
    : constrain(currentHeight * 0.18, 125, 175);

  // 設定選項高度
  let optionHeight = landscape
    ? constrain(currentHeight * 0.095, 34, 54)
    : constrain(currentHeight * 0.065, 42, 65);

  // 設定選項間距
  let optionGap = landscape
    ? constrain(currentHeight * 0.012, 5, 10)
    : constrain(currentHeight * 0.014, 7, 15);

  // 設定選項起始位置
  let optionStartY =
    questionY +
    questionHeight +
    (landscape ? 14 : 22);

  // 設定下一題按鈕寬度
  let buttonWidth = constrain(currentWidth * 0.28, 125, 250);

  // 設定下一題按鈕高度
  let buttonHeight = landscape
    ? constrain(currentHeight * 0.1, 36, 52)
    : constrain(currentHeight * 0.065, 40, 58);

  // 設定下一題按鈕 x 座標
  let buttonX = currentWidth / 2 - buttonWidth / 2;

  // 設定下一題按鈕 y 座標
  let buttonY =
    currentHeight -
    buttonHeight -
    (landscape ? 10 : 20);

  // 回傳版面資料
  return {
    // 回傳畫布寬度
    canvasWidth: currentWidth,

    // 回傳畫布高度
    canvasHeight: currentHeight,

    // 回傳畫面方向
    landscape: landscape,

    // 回傳左右邊距
    margin: margin,

    // 回傳內容寬度
    contentWidth: contentWidth,

    // 回傳題目 x 座標
    questionX: questionX,

    // 回傳題目 y 座標
    questionY: questionY,

    // 回傳題目寬度
    questionWidth: questionWidth,

    // 回傳題目高度
    questionHeight: questionHeight,

    // 回傳選項起始位置
    optionStartY: optionStartY,

    // 回傳選項高度
    optionHeight: optionHeight,

    // 回傳選項間距
    optionGap: optionGap,

    // 回傳按鈕 x 座標
    buttonX: buttonX,

    // 回傳按鈕 y 座標
    buttonY: buttonY,

    // 回傳按鈕寬度
    buttonWidth: buttonWidth,

    // 回傳按鈕高度
    buttonHeight: buttonHeight
  };
}

// 繪製測驗畫面
function drawQuiz() {
  // 取得目前題目
  let question = questions[currentQuestion];

  // 取得響應式版面
  let layout = getLayout();

  // 儲存題目方框資料
  questionBox = {
    // 儲存題目方框 x 座標
    x: layout.questionX,

    // 儲存題目方框 y 座標
    y: layout.questionY,

    // 儲存題目方框寬度
    w: layout.questionWidth,

    // 儲存題目方框高度
    h: layout.questionHeight
  };

  // 設定標題文字顏色
  fill("#6d435a");

  // 設定標題文字大小
  textSize(
    constrain(
      min(layout.canvasWidth * 0.045, layout.canvasHeight * 0.06),
      20,
      42
    )
  );

  // 設定文字置中
  textAlign(CENTER, CENTER);

  // 顯示測驗標題
  text("p5.js 簡易指令練習測驗", width / 2, height * 0.055);

  // 設定題數文字顏色
  fill("#9b607f");

  // 設定題數文字大小
  textSize(constrain(width * 0.025, 13, 24));

  // 顯示題數
  text(
    "第 " + (currentQuestion + 1) + " 題，共 " + questions.length + " 題",
    width / 2,
    height * 0.115
  );

  // 設定題目方框背景顏色
  fill("#f4acb7");

  // 設定題目方框外框顏色
  stroke("#b56b86");

  // 設定外框粗細
  strokeWeight(2);

  // 繪製題目方框
  rect(questionBox.x, questionBox.y, questionBox.w, questionBox.h, 14);

  // 儲存繪圖狀態
  push();

  // 移除文字外框
  noStroke();

  // 設定題目文字顏色
  fill("#33232d");

  // 設定題目文字大小
  textSize(
    constrain(
      min(layout.canvasWidth * 0.032, layout.canvasHeight * 0.045),
      16,
      32
    )
  );

  // 設定文字水平與垂直置中
  textAlign(CENTER, CENTER);

  // 將題目文字畫在方框正中央
  text(
    question.question,
    questionBox.x + questionBox.w / 2,
    questionBox.y + questionBox.h / 2
  );

  // 還原繪圖狀態
  pop();

  // 清除選項資料
  optionBoxes = [];

  // 逐一繪製四個選項
  for (let i = 0; i < question.options.length; i++) {
    // 設定選項 x 座標
    let optionX = layout.margin;

    // 設定選項 y 座標
    let optionY =
      layout.optionStartY +
      i * (layout.optionHeight + layout.optionGap);

    // 設定選項寬度
    let optionWidth = layout.contentWidth;

    // 設定水平位移
    let moveX = 0;

    // 設定垂直位移
    let moveY = 0;

    // 判斷是否為正確答案
    let isCorrect = i === question.answer;

    // 判斷是否為使用者選錯的選項
    let isWrong =
      quizState === "feedback" &&
      selectedOption === i &&
      selectedOption !== question.answer;

    // 判斷是否答錯
    let isAnswerWrong =
      quizState === "feedback" &&
      selectedOption !== question.answer;

    // 答錯時讓正確答案上下跳動
    if (isAnswerWrong && isCorrect) {
      // 設定上下動畫
      moveY = sin((millis() - animationStartTime) * 0.01) * 10;
    }

    // 答錯時讓錯誤選項左右移動
    if (isWrong) {
      // 設定左右動畫
      moveX = sin((millis() - animationStartTime) * 0.012) * 12;
    }

    // 儲存選項資料
    optionBoxes.push({
      // 儲存選項 x 座標
      x: optionX,

      // 儲存選項 y 座標
      y: optionY,

      // 儲存選項寬度
      w: optionWidth,

      // 儲存選項高度
      h: layout.optionHeight,

      // 儲存選項索引
      index: i,

      // 儲存水平位移
      moveX: moveX,

      // 儲存垂直位移
      moveY: moveY
    });

    // 儲存繪圖狀態
    push();

    // 套用動畫位移
    translate(moveX, moveY);

    // 設定選項預設背景
    fill("#ffffff");

    // 答對時顯示淡綠色
    if (
      quizState === "feedback" &&
      selectedOption === question.answer &&
      isCorrect
    ) {
      // 設定答對背景
      fill("#d8f3dc");
    }

    // 答錯時正確答案顯示粉色
    if (isAnswerWrong && isCorrect) {
      // 設定正確答案背景
      fill("#ffc2d1");
    }

    // 使用者選錯時顯示深粉色
    if (isWrong) {
      // 設定錯誤答案背景
      fill("#fb6f92");
    }

    // 設定選項外框顏色
    stroke("#d8a9bb");

    // 設定選項外框粗細
    strokeWeight(2);

    // 繪製選項方框
    rect(optionX, optionY, optionWidth, layout.optionHeight, 12);

    // 移除文字外框
    noStroke();

    // 設定選項文字顏色
    fill("#33232d");

    // 設定選項文字大小
    textSize(
      constrain(
        min(layout.canvasWidth * 0.028, layout.canvasHeight * 0.04),
        14,
        28
      )
    );

    // 設定選項文字置中
    textAlign(CENTER, CENTER);

    // 將選項文字放在方框正中央
    text(
      question.options[i],
      optionX + optionWidth / 2,
      optionY + layout.optionHeight / 2
    );

    // 還原繪圖狀態
    pop();
  }

  // 儲存下一題按鈕位置
  nextButton = {
    // 儲存按鈕 x 座標
    x: layout.buttonX,

    // 儲存按鈕 y 座標
    y: layout.buttonY,

    // 儲存按鈕寬度
    w: layout.buttonWidth,

    // 儲存按鈕高度
    h: layout.buttonHeight
  };

  // 尚未回答時顯示灰色
  if (quizState === "answering") {
    // 設定按鈕背景
    fill("#d9d9d9");
  } else {
    // 設定已回答按鈕背景
    fill("#ff8fab");
  }

  // 設定按鈕外框
  stroke("#b56b86");

  // 設定按鈕外框粗細
  strokeWeight(2);

  // 繪製下一題按鈕
  rect(
    layout.buttonX,
    layout.buttonY,
    layout.buttonWidth,
    layout.buttonHeight,
    12
  );

  // 移除文字外框
  noStroke();

  // 設定按鈕文字顏色
  fill("#ffffff");

  // 設定按鈕文字大小
  textSize(
    constrain(
      min(layout.canvasWidth * 0.028, layout.canvasHeight * 0.04),
      15,
      28
    )
  );

  // 設定按鈕文字置中
  textAlign(CENTER, CENTER);

  // 顯示下一題文字
  text(
    "下一題",
    layout.buttonX + layout.buttonWidth / 2,
    layout.buttonY + layout.buttonHeight / 2
  );
}

// 處理滑鼠點擊
function mousePressed() {
  // 測驗完成後不處理點擊
  if (quizState === "finished") {
    // 結束滑鼠事件
    return;
  }

  // 尚未回答時檢查選項
  if (quizState === "answering") {
    // 逐一檢查選項
    for (let option of optionBoxes) {
      // 計算選項目前的 x 座標
      let currentX = option.x + option.moveX;

      // 計算選項目前的 y 座標
      let currentY = option.y + option.moveY;

      // 判斷是否點擊選項
      if (
        mouseX >= currentX &&
        mouseX <= currentX + option.w &&
        mouseY >= currentY &&
        mouseY <= currentY + option.h
      ) {
        // 儲存使用者選擇
        selectedOption = option.index;

        // 判斷使用者是否答對
        if (selectedOption === questions[currentQuestion].answer) {
          // 增加答對題數
          score++;
        }

        // 進入回饋狀態
        quizState = "feedback";

        // 記錄動畫開始時間
        animationStartTime = millis();

        // 停止檢查其他選項
        break;
      }
    }

    // 結束滑鼠事件
    return;
  }

  // 判斷是否點擊下一題按鈕
  if (
    mouseX >= nextButton.x &&
    mouseX <= nextButton.x + nextButton.w &&
    mouseY >= nextButton.y &&
    mouseY <= nextButton.y + nextButton.h
  ) {
    // 判斷是否還有下一題
    if (currentQuestion < questions.length - 1) {
      // 前往下一題
      currentQuestion++;

      // 清除選擇
      selectedOption = -1;

      // 回到答題狀態
      quizState = "answering";
    } else {
      // 設定測驗完成
      quizState = "finished";
    }
  }
}

// 顯示結果畫面
function drawResult() {
  // 設定背景顏色
  background("#fff8fa");

  // 設定文字置中
  textAlign(CENTER, CENTER);

  // 設定結果標題顏色
  fill("#6d435a");

  // 設定結果標題大小
  textSize(constrain(min(width * 0.07, height * 0.08), 28, 64));

  // 顯示測驗結束文字
  text("測驗結束！", width / 2, height * 0.35);

  // 設定分數文字顏色
  fill("#33232d");

  // 設定分數文字大小
  textSize(constrain(min(width * 0.05, height * 0.06), 22, 46));

  // 顯示答對題數
  text(
    "你答對了 " + score + " / " + questions.length + " 題",
    width / 2,
    height * 0.5
  );

  // 設定鼓勵文字顏色
  fill("#9b607f");

  // 設定鼓勵文字大小
  textSize(constrain(min(width * 0.03, height * 0.04), 15, 30));

  // 顯示鼓勵訊息
  text(
    "繼續練習，你會越來越熟悉 p5.js！",
    width / 2,
    height * 0.63
  );
}

// 當視窗尺寸改變時執行
function windowResized() {
  // 重新設定畫布大小
  resizeCanvas(windowWidth, windowHeight);
}
```
:::


---

## 學習3：設定嵌入 Google 字型，網頁文字採用這些字型

https://cfchen58.synology.me/115/week4/stage3/

**這個階段的目標：** 從 Google Fonts 嵌入繁體中文字型，並讓畫布上的題目與選項文字使用這些字型。
**這個階段會修改的檔案：** index.html、sketch.js

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習3截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習3的程式碼
```javascript=
//學習3程式碼所在

```
:::


---

## 學習4：設定題庫並抽題顯示題目網頁（CSV 檔案）

https://cfchen58.synology.me/115/week4/stage4/

**這個階段的目標：** 把題目移到 questions.csv，網站讀取題庫後每次隨機抽出 5 題。
**這個階段會修改的檔案：** index.html、sketch.js、questions.csv

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習4截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習4的程式碼
```javascript=
//學習4程式碼所在

```
:::


---

## 學習5：利用 Google Sheets 當題庫

https://cfchen58.synology.me/115/week4/stage5/

**這個階段的目標：** 把題庫放在 Google 試算表，網站直接讀取，老師改試算表，網站題目就跟著更新。
**這個階段會修改的檔案：** index.html、sketch.js（questions.csv 當備用題庫）

### 執行截圖

（把截圖拖曳到這裡，或貼上圖片連結）

![學習5截圖](請貼上截圖)

### 第一次問 AI

```tex!
（逐字貼上你第一次問 AI 的提示詞）
```

### 第二次問 AI

```tex!
（逐字貼上你第二次問 AI 的提示詞）
```

### 第三次問 AI

```tex!
（逐字貼上你第三次問 AI 的提示詞）
```

### 程式碼內容

:::info
:::spoiler 點開貼上學習5的程式碼
```javascript=
//學習5程式碼所在

```
:::


---

## 我的心得

這五個學習中，哪一個最困難？你是怎麼解決的？（請寫出實際發生的事）

＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿＿
