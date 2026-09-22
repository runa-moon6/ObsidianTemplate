---
tags:
  - Tag
obsidianUIMode: preview
---
> ```meta-bind-button
> style: primary
> label: Add Note
> id: add-project-default
> action: 
>    type: templaterCreateNote
>    templateFile: "999_Templates/Note.md"
>    folderPath: "002_Notes"
>    fileName: Security
> ```

## Notice: 各メモの目次は右サイドバーのアウトラインを見ること
# Notes
## Notice: ソートはできるだけ空白の部分をクリックすること
```dataviewjs
// 1. ノートデータの取得（初期ソート順設定）
const pages = dv.pages('#Security')
                .sort(p => p.file.frontmatter?.Title || p.file.name, 'asc');

// 2. 表用データの生成（安全な値の取得）
const rows = pages.map(p => {
    const displayTitle = p.file.frontmatter?.Title || p.file.name;
    const createdDate = p.file.frontmatter?.Created || "";
    const importance = p.file.frontmatter?.Importance || "";
    
    // Icon Folder等のエラーを回避するための生のリンク文字列
    const linkStr = `[[${p.file.path}|${displayTitle}]]`;
    
    return [
        linkStr,
        createdDate,
        importance
    ];
});

// 3. コンテナの作成と表の描画
const container = dv.el("div", "");
dv.api.table(["Note", "Created", "Importance"], rows, container, dv.component);

// 4. クリックで列ソートを行うスクリプト処理
setTimeout(() => {
    const table = container.querySelector("table");
    if (!table) return;

    const headers = table.querySelectorAll("th");
    let sortDirections = [false, true, true]; // 3列分用意（1列目は初期ソート済のため次回降順）

    headers.forEach((header, index) => {
        header.style.cursor = "pointer";
        header.title = "クリックで並び替え";
        
        header.addEventListener("click", () => {
            const tbody = table.querySelector("tbody");
            if (!tbody) return;
            const trs = Array.from(tbody.querySelectorAll("tr"));
            const asc = sortDirections[index];

            trs.sort((a, b) => {
                const cellA = a.children[index]?.textContent.trim() || "";
                const cellB = b.children[index]?.textContent.trim() || "";
                
                // 数値データの比較（Importance等の数値対応）
                const numA = Number(cellA);
                const numB = Number(cellB);
                if (!isNaN(numA) && !isNaN(numB) && cellA !== "" && cellB !== "") {
                    return asc ? numA - numB : numB - numA;
                }
                
                return asc ? cellA.localeCompare(cellB, 'ja') : cellB.localeCompare(cellA, 'ja');
            });

            // ソート結果を再配置
            trs.forEach(tr => tbody.appendChild(tr));
            sortDirections[index] = !asc;
        });
    });
}, 150);
```

