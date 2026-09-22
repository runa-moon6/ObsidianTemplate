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
<%*
let title = tp.file.title;
if (title.startsWith("Untitled")) {
    title = await tp.system.prompt("Class Name");
    await tp.file.rename(title);
}

tR += ">    fileName: " + title + "\n";
tR += "> ```\n\n";
tR += "## Notice: 各メモの目次は右サイドバーのアウトラインを見ること\n";
tR += "# Notes\n";
tR += "## Notice: ソートはできるだけ空白の部分をクリックすること\n";
tR += "```dataviewjs\n";
tR += "// 1. ノートデータの取得（初期ソート順設定）\n";
tR += "const pages = dv.pages('#" + title +"')\n";
tR += "                .sort(p => p.file.frontmatter?.Title || p.file.name, 'asc');\n";
tR += "\n";
tR += "// 2. 表用データの生成（安全な値の取得）\n";
tR += "const rows = pages.map(p => {\n";
tR += "    const displayTitle = p.file.frontmatter?.Title || p.file.name;\n";
tR += "    const createdDate = p.file.frontmatter?.Created || \"\";\n";
tR += "    const importance = p.file.frontmatter?.Importance || \"\";\n";
tR += "    \n";
tR += "    // Icon Folder等のエラーを回避するための生のリンク文字列\n";
tR += "    const linkStr = `[[${p.file.path}|${displayTitle}]]`;\n";
tR += "    \n";
tR += "    return [\n";
tR += "        linkStr,\n";
tR += "        createdDate,\n";
tR += "        importance\n";
tR += "    ];\n";
tR += "});\n";
tR += "\n";
tR += "// 3. コンテナの作成と表の描画\n";
tR += "const container = dv.el(\"div\", \"\");\n";
tR += "dv.api.table([\"Note\", \"Created\", \"Importance\"], rows, container, dv.component);\n";
tR += "\n";
tR += "// 4. クリックで列ソートを行うスクリプト処理\n";
tR += "setTimeout(() => {\n";
tR += "    const table = container.querySelector(\"table\");\n";
tR += "    if (!table) return;\n";
tR += "\n";
tR += "    const headers = table.querySelectorAll(\"th\");\n";
tR += "    let sortDirections = [false, true, true]; // 3列分用意（1列目は初期ソート済のため次回降順）\n";
tR += "\n";
tR += "    headers.forEach((header, index) => {\n";
tR += "        header.style.cursor = \"pointer\";\n";
tR += "        header.title = \"クリックで並び替え\";\n";
tR += "        \n";
tR += "        header.addEventListener(\"click\", () => {\n";
tR += "            const tbody = table.querySelector(\"tbody\");\n";
tR += "            if (!tbody) return;\n";
tR += "            const trs = Array.from(tbody.querySelectorAll(\"tr\"));\n";
tR += "            const asc = sortDirections[index];\n";
tR += "\n";
tR += "            trs.sort((a, b) => {\n";
tR += "                const cellA = a.children[index]?.textContent.trim() || \"\";\n";
tR += "                const cellB = b.children[index]?.textContent.trim() || \"\";\n";
tR += "                \n";
tR += "                // 数値データの比較（Importance等の数値対応）\n";
tR += "                const numA = Number(cellA);\n";
tR += "                const numB = Number(cellB);\n";
tR += "                if (!isNaN(numA) && !isNaN(numB) && cellA !== \"\" && cellB !== \"\") {\n";
tR += "                    return asc ? numA - numB : numB - numA;\n";
tR += "                }\n";
tR += "                \n";
tR += "                return asc ? cellA.localeCompare(cellB, 'ja') : cellB.localeCompare(cellA, 'ja');\n";
tR += "            });\n";
tR += "\n";
tR += "            // ソート結果を再配置\n";
tR += "            trs.forEach(tr => tbody.appendChild(tr));\n";
tR += "            sortDirections[index] = !asc;\n";
tR += "        });\n";
tR += "    });\n";
tR += "}, 150);\n";
tR += "```\n\n";
%>