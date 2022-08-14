---
layout: default
title: List of repositories
has_children: false
nav_order: 2
---

<div id="main">

Loading data...

If you're using GitHub, please visit this page through [the website](https://recloudstream.github.io) as it loads content dynamically.

</div>

<!-- new lines get replaced with spaces, so remember about ; -->
<script>
function makeTd(text) {
    const td = document.createElement("td");
    td.appendChild(
        document.createTextNode(text)
    );
    td.style.textAlign = 'left';
    return td;
}

async function fetchRow(url) {
    const r = await fetch(url);
    const data = await r.json();
    const row = document.createElement("tr");
    row.appendChild(makeTd(data.name || "unnamed"));
    row.appendChild(makeTd(data.description || "No description provided"));

    const td = document.createElement("td");
    td.style.textAlign = 'center';
    
    const btn = document.createElement("a");
    btn.innerText = "Install";
    btn.classList.add("btn");
    btn.href = `https://cs.repo/${url.replace(/^https?:\/\//, "")}`;
    td.appendChild(btn);
    
    row.appendChild(td);
    return row;
}

async function fetchData() {
    const mainDiv = document.getElementById("main");
    
    const table = document.createElement("table");
    const thead = document.createElement("thead");
    thead.innerHTML = `
    <tr>
        <th style="text-align: left">Name</th>
        <th style="text-align: left">Description</th>
        <th style="text-align: left">Install</th>
    </tr>
`;
    table.appendChild(thead);

    const tbody = document.createElement("tbody");
    const r = await fetch("https://raw.githubusercontent.com/recloudstream/cs-repos/master/repos-db.json");
    const data = await r.json();

    for (const repo of data) {
        const row = await(fetchRow(repo));
        tbody.appendChild(row);
    }
    table.appendChild(tbody);

    mainDiv.innerHTML = '';
    mainDiv.classList.add("table-wrapper");
    mainDiv.appendChild(table);
}

fetchData();
</script>
