---
date: '2025-01-23T23:12:40+08:00'
draft: false
title: '深入了解 Queue（佇列）：概念與應用實作 ✨'
---

## Ｑueue 的概念

### 什麼是 Queue？

Queue（佇列）是一種先進先出（FIFO, First In First Out）的資料結構。這表示最早被放入佇列的元素，會最先被移除，就像排隊買飲料一樣，先來的人先買到飲料，後來的人要排在後面～ 🍹

### Queue 的基本操作
1. Enqueue（入列） ➡️ 將元素加入佇列尾端。
2. Dequeue（出列） ⬅️ 移除佇列前端的元素。
3. Peek（查看） 👀 查看佇列最前端的元素，但不移除它。
4. isEmpty（是否為空） ❓ 判斷佇列是否為空。

<br/>

### 視覺化示意圖

假設現在有一個空佇列：
```
[  ]
```
<br/>

**Enqueue（加入元素）**

我們依序加入 1️⃣、2️⃣、3️⃣：
```
[1, 2, 3]  → 1 是最先加入的，排在最前面
```
<br/>

**Dequeue（移除元素）**

現在執行一次出列操作：
```
Dequeue ➡️ 1 被移除 ➡️ [2, 3]
```

是不是很像排隊買飲料，前面的人先買完離開，後面的人往前移呢？（*´∀`）~♥

### Queue 的常見應用
1. 排隊系統（銀行、超商、遊戲配對）：誰先來就先服務！🏦
2. 資源管理（印表機佇列）：文件按順序列印🖨️
3. 廣度優先搜尋（BFS）：用於圖論與迷宮解法🗺️
4. 異步任務處理（如伺服器請求）：排隊處理任務🛠️

## Queue 的實作範例：遊戲攻擊隊列 🎮
接下來，我們來看一個在遊戲場景中應用 Queue 的例子，展示如何使用 JavaScript 建立一個攻擊順序的佇列系統。

### 範例 ⚔️

以下是一段模擬角色戰鬥的程式碼，展示如何使用 Queue 實現回合制攻擊邏輯：
```javascript
const queue = []; // 建立一個空的攻擊佇列

// 動態生成按鈕，對應玩家的攻擊
emby.attacks.forEach((attack) => {
  const button = document.createElement("button");
  button.innerHTML = attack.name;
  document.querySelector("#attackBox").append(button);
});

// 綁定攻擊按鈕事件
document.querySelectorAll("button").forEach((button) => {
  button.addEventListener("click", (e) => {
    const selectedAttack = attacks[e.currentTarget.innerHTML];

    // 玩家攻擊
    emby.attack({
      attack: selectedAttack,
      recipient: draggle,
      renderedSprites,
    });

    // 若敵人血量歸零，觸發勝利邏輯
    if (draggle.health <= 0) {
      queue.push(() => draggle.faint());
      queue.push(() => endBattle());
    } else {
      // 若敵人未死亡，隨機選擇攻擊並加入隊列
      const randomAttack =
        draggle.attacks[Math.floor(Math.random() * draggle.attacks.length)];

      queue.push(() => {
        draggle.attack({
          attack: randomAttack,
          recipient: emby,
          renderedSprites,
        });

        // 若玩家血量歸零，觸發失敗邏輯
        if (emby.health <= 0) {
          queue.push(() => emby.faint());
          queue.push(() => endBattle());
        }
      });
    }
  });
});

// 點擊對話框，執行佇列中的攻擊
document.querySelector("#dialogueBox").addEventListener("click", () => {
  if (queue.length > 0) {
    queue.shift()(); // 執行佇列中的第一個動作
  } else {
    document.querySelector("#dialogueBox").style.display = "none"; // 若佇列為空，隱藏對話框
  }
});

// 遊戲結束邏輯
function endBattle() {
  gsap.to("#overlappingDiv", {
    opacity: 1,
    onComplete: () => {
      cancelAnimationFrame(battleAnimationId);
      battle.initiated = false;
      animate();
      document.querySelector("#userInterface").style.display = "none";
      gsap.to("#overlappingDiv", { opacity: 0 });
    },
  });
}
```

### 解析 📖
1. 按鈕生成：
根據玩家角色的攻擊列表，動態生成按鈕，讓玩家選擇攻擊。
2. 攻擊佇列：
玩家攻擊後，將敵人的攻擊動作加入 queue，確保動作按照順序執行。
3. 死亡處理：
玩家或敵人血量歸零時，將其死亡動畫與結束邏輯加入 queue，確保事件按順序執行。
4. 點擊觸發佇列：
透過對話框的點擊事件，依序執行 queue 中的動作，模擬回合制的戰鬥流程。

### 應用場景與優勢
1. 順序控制：
Queue 的先進先出特性，使遊戲攻擊的執行邏輯清晰且可預測。
2. 擴展性強：
如果需要新增更多攻擊、動畫或特效，只需將對應函式推入 queue，無需改動核心邏輯。
3. 邏輯分離：
將遊戲邏輯與動畫執行分離，便於維護與調試。

### 結語
Queue 是處理回合制遊戲動作的利器，讓攻擊、特效、遊戲結束邏輯依序執行，既清晰又靈活。
透過這個實作範例，你可以看到 Queue 如何將複雜的遊戲邏輯簡化為一個易於維護的流程。

希望這篇文章能幫助你更好地理解 Queue 的應用！ 🎉
快試試將它應用到你的專案中吧！ヾ(≧▽≦*)o